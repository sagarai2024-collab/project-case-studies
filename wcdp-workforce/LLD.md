# WCDP – Low-Level Design (LLD)

> **Author:** Sagarika Chakraborty — Full Stack .NET Engineer | React.js | Web API | SQL Server

## 1. Solution Layout
- **src/Wcdp.Mobile** — Xamarin.Forms app
  - Views (XAML), ViewModels (MVVM), Services, Converters, Styles/Themes, Controls
- **src/Wcdp.Admin** — Windows desktop (WPF/UWP) admin app
- **src/Wcdp.Shared** — DTOs, Models, Validation, API client
- **src/Wcdp.Backend** — REST API (controllers, services, auth, DAL)
- **scripts/** — build/release automation (PowerShell, bash)
- **tests/** — unit/integration/UI tests

## 2. MVVM Structure
- **Views (XAML)** bind to **ViewModels** via `BindingContext`.
- **ViewModels** expose `ObservableCollection<T>` and `ICommand` for actions.
- **Services**: `IApiService`, `ISyncService`, `IAuthService`, `ILocalStore` (SQLite).
- **Dependency Injection** via container (e.g., Autofac/Microsoft.Extensions.DependencyInjection).

## 3. Key Pages & ViewModels
- **LoginPage / LoginViewModel** — auth, token caching.
- **PunchPage / PunchViewModel** — punch in/out; uses geotag (optional); offline queue.
- **TimecardPage / TimecardViewModel** — list/paginate history; filters.
- **AttendancePage / AttendanceViewModel** — daily/weekly/monthly views.
- **TasksPage / TasksViewModel** — assigned tasks, status updates, attachments.
- **Admin Dashboard (Desktop)** — workforce KPIs, task assignment, reporting exports.

## 4. Offline Sync
- Local **SQLite** with change tracking tables.
- Outbox pattern: queue writes while offline; replay on connectivity.
- Conflict resolution: last-write-wins or server-precedence with merge hints.

## 5. API Contracts (examples)
```json
// POST /api/punch
{ "employeeId": "guid", "type": "in", "timestamp": "2025-10-05T08:30:00Z", "note": "site A" }

// GET /api/timecards?employeeId=&from=&to=&page=1&size=50
// Response includes paging metadata and entries
```

## 6. Data Model (ER Overview)
**Core Tables**
- `Employees(id, name, email, role, status, createdAt)`
- `Punches(id, employeeId, type, timestamp, note, deviceId, createdAt)`
- `Timecards(id, employeeId, periodStart, periodEnd, regularHours, overtimeHours, approvedBy, approvedAt)`
- `Attendance(id, employeeId, date, status, shift, createdAt)`
- `Tasks(id, assignedTo, title, description, status, dueDate, createdAt)`

Indexes for `Punches(employeeId, timestamp)`, `Attendance(employeeId, date)`, `Tasks(assignedTo, status)`.

## 7. Error Handling & Codes
- `400` invalid input; field error map
- `401/403` auth/authorization
- `404` not found
- `409` conflict (duplicate punch, task state)
- `429` rate limit
- `5xx` transient errors (retry with backoff on client; circuit breaker on server)

## 8. Observability
- Mobile crash analytics (AppCenter/Crashlytics) and logs.
- API: structured logging and distributed tracing; dashboards for error rate and latency.

## 9. CI/CD (Azure DevOps and Jenkins)
- Pipeline runs unit tests and builds artifacts on every commit.
- Mobile: fastlane/App Center or store CLIs; deployments to **Play Store**/**App Store**.
- Desktop: MSI/installer packaged by the release pipeline.
- Pipeline permissions scoped to the minimum; secrets kept in pipeline secret variables.

## 10. Test Strategy
- **Unit**: ViewModels, Services, Validation.
- **Integration**: API endpoints with in-memory DB or containers.
- **UI/E2E**: Xamarin.UITest/Appium for mobile; WinAppDriver for desktop.
- **Load**: API load tests on punch and sync endpoints.

## 11. Runbooks
- Incident response for sync backlog, store release rollback, API autoscale limits, key rotation.
