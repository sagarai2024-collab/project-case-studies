# EIS AMERICAS‑FMRS — Equipment Installation Services (Contractor & Installer Management). 

> **Client:** EIS Americas, USA  
> **Domain:** Equipment Installation Services & Job Management for Contractors and Installers  

**EIS AMERICAS‑FMRS** is a web-based contractor and installer management platform for **equipment installations** (e.g., Air Conditioners, large cold‑storage units, industrial equipment). It delivers the complete workflow from **customer order → job creation → assignment → on‑site progress → completion & review**, plus before/after image validation, feedback, and an integrated **TeleTech** communication module for transparent collaboration among customers, contractors, and installers.

---

## ✨ Highlights
- **Contractor and installer portals** on a .NET 6 Web API, with a React 17 admin and contractor portal.
- **Job lifecycle tracking**: `ToBeAssigned → Assigned → InProgress → Completed → Reviewed`.
- **Media & Feedback**: before/after image uploads and customer feedback capture for quality checks.
- **Clean Architecture (4‑layer)** with Repository + Service patterns.
- **Azure Functions** for background tasks; **YAML pipelines** for CI/CD; environment-based deployments (Dev/QA/Prod).

---

## 🧱 Tech Stack
- **Backend**: .NET Core 6, Web API, ASP.NET Core Middleware, Swagger
- **Data**: SQL Server, EF Core (Code‑First), LINQ
- **Utilities**: JSON, `.resx` resources (messages/localization), helpers/validators
- **Dev & Tools**: Visual Studio 2022, Postman
- **Cloud**: Azure Functions (CI/CD via Azure DevOps YAML)

---

## 🧭 Documentation
- **HLD**: [`HLD.md`](HLD.md) — architecture, flows, security, CI/CD
- **LLD**: [`LLD.md`](LLD.md) — endpoints, DTOs, schema, sequences, error codes
- **Portal (React) HLD / LLD**: [`portal-HLD.md`](portal-HLD.md), [`portal-LLD.md`](portal-LLD.md)
- **Diagram**: [`architecture.png`](architecture.png)

---

## 👩‍💻 Credits
**Sagarika Chakraborty** — Architecture & development of Web API, status workflow, TeleTech module, EF Core models, Swagger/Postman, Azure Functions + YAML pipelines.

---
