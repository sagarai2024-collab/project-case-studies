# Smart Visa — Low‑Level Design (LLD)

> **Author:** Sagarika Chakraborty — Full Stack .NET Engineer | React.js | Web API | SQL Server

## 1) Solution Layout (DNN Modules)
- **DesktopModules/SmartVisa.Applications** — visa application wizard (views, presenters, services)
- **DesktopModules/SmartVisa.Documents** — document uploads, previews, verification checklists
- **DesktopModules/SmartVisa.Status** — status timeline, notifications
- **DesktopModules/SmartVisa.Admin** — admin review queues, notes, decisions
- **SmartVisa.Core** — shared models (DTOs), validation, utilities, DAL

## 2) Pages & Components
- **Applicant Pages**:
  - `Apply.aspx` — wizard (Telerik TabStrip/Stepper); sections: Personal, Travel, Documents, Review.
  - `MyApplications.aspx` — grid of applications with statuses, actions.
- **Admin Pages**:
  - `ReviewQueue.aspx` — Telerik Grid with filters, bulk actions.
  - `ApplicationDetail.aspx` — details, documents, comments, decision panel.
- **Shared**: layout controls, notifications, profile edit, reference data management.

## 3) Data Model (ER Overview)
**Tables**
- `Applications(id, userId, visaType, country, status, createdAt, submittedAt, decidedAt)`
- `ApplicationSteps(id, applicationId, step, dataJson, completedAt)`
- `Documents(id, applicationId, kind, url, checksum, size, uploadedAt, verifiedBy, verifiedAt)`
- `Notes(id, applicationId, authorId, text, at)`
- `Audit(id, entity, entityId, action, byUserId, at, detailJson)`

Indexes: `Applications(userId, status)`, `Documents(applicationId)`, `Audit(entity, entityId)`.

## 4) Status Machine
`Draft → Submitted → UnderReview → AdditionalInfoRequested → Approved / Rejected`  
Rules enforce valid transitions; audit records on every change.

## 5) APIs/Handlers (examples)
- `POST /SmartVisa/Application/SaveDraft`
- `POST /SmartVisa/Application/Submit`
- `POST /SmartVisa/Documents/Upload`
- `POST /SmartVisa/Admin/Decision` (`Approve|Reject|RequestInfo`)
- `GET /SmartVisa/Status/{applicationId}`

## 6) Validation & Error Handling
- Server‑side validation attributes; client‑side Telerik validators.
- File type/size checks; duplicate document detection by checksum.
- Friendly error pages; exception logging with correlation id.

## 7) Performance
- Server‑side paging/sorting on Telerik grids.
- Cache reference data (countries/visa types).
- Async streaming for uploads/downloads.

## 8) Observability
- Centralized log provider (DNN logging + custom).
- Audit trail for admin actions and status transitions.
- Health check endpoint for monitoring.

## 9) Deployment
- Build DNN module packages (`.zip`) and install via DNN Host → Extensions.
- IIS app pool recycling strategy; warm‑up scripts if required.
