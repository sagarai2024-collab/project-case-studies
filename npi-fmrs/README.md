# NPI-FMRS — National Property Inspection – Field Management & Reporting System

> **Client:** National Property Inspection, Nebraska (USA)  
> **Domain:** Real Estate & Home/Commercial Inspection

**NPI-FMRS** is a web-based enterprise platform that streamlines **end-to-end inspection workflows** for home and commercial properties: user management, inspection scheduling, **bidding**, **feedback**, **notifications**, and **reporting**. It implements a **four-layer Clean Architecture** (Presentation, Application, Domain, Infrastructure) with **role-based experiences** for **Owners**, **Inspectors**, and **Administrators**. Background tasks and near real-time cross-system pushes run via **Azure Functions**, with CI/CD powered by **Azure DevOps** (and Jenkins where applicable). Shared dependencies are managed through **Azure Artifacts**.

---

## ✨ Key Features
- **Owner module:** manage properties, request inspections, participate in bidding, view reports.
- **Inspector module:** view assignments, bid on jobs, submit field reports.
- **Bidding module:** auction-style interface with rules, min increments, SLAs.
- **Feedback & Notifications:** capture service feedback; real-time updates on bids/inspections.
- **Auth & Security:** ASP.NET Core Identity, Authorization Handlers, DI + custom Middleware.
- **Data:** EF Core over SQL Server with stored procedures where appropriate; optimized LINQ queries.
- **Ops:** CI/CD with Azure DevOps (and Jenkins), Azure Functions for background tasks; artifacts sharing.

---

## 🧱 Technology Stack
- **Backend:** .NET 6, ASP.NET Core Identity, DI, Middleware, Authorization Handlers, jQuery/AJAX endpoints
- **Database:** Microsoft SQL Server 2019 (EF Core + Stored Procedures + LINQ)
- **Architecture:** Four-layer Clean Architecture (Presentation, Application, Domain, Infrastructure)
- **CI/CD & Tools:** Visual Studio 2022, Azure DevOps Pipelines, Jenkins, Azure Functions, Azure Artifacts
- **Telemetry:** Application Insights (optional), structured logs

---

## 🧭 Documentation
- **HLD:** [`HLD.md`](HLD.md) – architecture, flows, NFRs, security, CI/CD
- **LLD:** [`LLD.md`](LLD.md) – endpoints, DTOs, DB schema, sequences, error codes
- **Diagram:** [`architecture.png`](architecture.png)

---

## 👩‍💻 Credits
**Sagarika Chakraborty** — Full Stack .NET Engineer (React.js, Web API, SQL Server)  
*Responsibilities:* Clean Architecture, Identity/Authorization, EF Core & SPs, modules (Owner/Inspector/Bidding/Feedback/Notifications), performance tuning, CI/CD, Azure Functions.

---
