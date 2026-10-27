# NPI-FMRS — Low-Level Design (LLD)

> **Author:** Sagarika Chakraborty — Full Stack .NET Engineer | React.js | Web API | SQL Server

## 1. Solution Layout (Clean Architecture)
- **src/NpiFmrs.Presentation** — MVC/Razor + jQuery/AJAX; controllers; view models; static assets
- **src/NpiFmrs.Application** — use cases, validators, DTOs, mediators, policies
- **src/NpiFmrs.Domain** — entities, value objects, aggregates, domain services
- **src/NpiFmrs.Infrastructure** — EF Core context, repositories, migrations, SPs, external providers
- **jobs/NpiFmrs.Functions** — Azure Functions for background tasks (notifications, exports, schedulers)
- **tests/** — unit/integration/contract tests

## 2. Core Endpoints (sample)
### Auth & Users
- `POST /api/auth/login` → `{ email, password }` → `{ accessToken, roles }`
- `POST /api/auth/refresh`
- `POST /api/users` (Admin) — create user; assign roles (Owner/Inspector/Admin)

### Properties & Inspections
- `POST /api/properties` (Owner/Admin)
- `GET /api/properties/{id}`
- `POST /api/inspections` (Owner/Admin) — create request
- `GET /api/inspections?status=&ownerId=&inspectorId=&from=&to=`
- `PUT /api/inspections/{id}/schedule` (Admin) — set date/time

### Bidding
- `POST /api/bids` (Inspector) — `{ inspectionId, amount }`
- `GET /api/bids?inspectionId=`
- `POST /api/bids/{id}/accept` (Owner/Admin) — finalize winner

### Reports & Feedback
- `POST /api/reports` (Inspector) — submit report (with media links)
- `GET /api/reports/{id}`
- `POST /api/feedback` (Owner) — `{ inspectionId, rating, comments }`

### Notifications
- `POST /api/notifications/test` (Admin) — send test
- **Functions:** `NotifyStatusChange`, `EscalationTimer`, `ExportJob`

## 3. DTOs (examples)
```json
// InspectionCreateRequest
{
  "propertyId": "guid",
  "requestedBy": "guid",
  "preferredDate": "2025-10-10T10:00:00Z",
  "scope": "Full home inspection including roof, plumbing, HVAC."
}

// BidCreateRequest
{
  "inspectionId": "guid",
  "amount": 250.00,
  "notes": "Can do within 48 hours."
}
```

## 4. Validation Rules
- Property: address fields required; geo validation optional.
- Inspection: preferredDate >= now; scope length limits.
- Bidding: amount >= min; enforce increments; prevent self-bidding where restricted.
- Reports: file size/type limits; media stored by provider; references persisted.
- Feedback: rating 1..5; comments length <= 2k chars.

## 5. Persistence (ER Overview)
**Tables**
- `Users(id, email, hash, roles, isActive, createdAt)`
- `Properties(id, ownerId, address1, address2, city, state, zip, type, createdAt)`
- `Inspections(id, propertyId, ownerId, inspectorId, status, preferredDate, scheduledAt, createdAt)`
- `Bids(id, inspectionId, inspectorId, amount, status, createdAt)`
- `Reports(id, inspectionId, inspectorId, url, notes, createdAt)`
- `Feedback(id, inspectionId, ownerId, rating, comments, createdAt)`
- `Notifications(id, userId, kind, payloadJson, status, createdAt)`
Indexes: `Inspections(status, ownerId)`, `Bids(inspectionId)`, `Properties(ownerId)`, `Users(email) unique`.

## 6. Sequences
```mermaid
sequenceDiagram
  participant UI as Presentation (jQuery/AJAX)
  participant API as .NET 6 API
  participant SQL as SQL Server
  participant FX as Azure Functions

  UI->>API: POST /api/inspections (create request)
  API->>SQL: Insert Inspection (Pending)
  API-->>UI: 201 Created
  FX->>SQL: Pick new Pending inspections
  FX-->>API: Notify eligible inspectors
  UI->>API: POST /api/bids (Inspector)
  API->>SQL: Insert Bid; evaluate rules
  UI->>API: POST /api/bids/{id}/accept (Owner/Admin)
  API->>SQL: Update winner; set schedule
  UI->>API: POST /api/reports (Inspector)
  API->>SQL: Save report; notify owner
```

## 7. Error Handling
- `400` validation; field errors map
- `401/403` JWT/RBAC
- `404` not found
- `409` bid conflict or invalid state transition
- `413` payload too large (media)
- `5xx` transient failures → retry/backoff with circuit breaker

## 8. Observability
- Correlation ID middleware; structured logs
- (Optional) Application Insights traces/metrics
- Dashboards: p95 latency, bid success rate, inspection throughput, export SLAs

## 9. Test Strategy
- **Unit:** validators, use cases, bidding rules, report aggregation
- **Integration:** controllers + repos + SQL testcontainer; stored procedure coverage where used
- **Contract:** notification provider/webhook stubs
- **Load:** k6/Locust on bidding and listing endpoints
- **Security:** role matrix; authZ handler tests; rate limits

## 10. Deployment & Runbooks
- CI: build/test → publish to **Azure Artifacts**
- CD: deploy API + Functions via Jenkins/Azure DevOps; blue/green supported
- Backups: SQL backups + PITR; export job resumes on restart
- Runbooks: stuck notifications queue, bid spam, slow queries, export failure triage
