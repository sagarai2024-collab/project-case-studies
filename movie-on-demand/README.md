# Movie on Demand – OTT Platform (React + .NET Core)

**Movie on Demand** is an OTT-style platform that manages **subscribers**, **movie catalogues**, and **slot bookings**, with **real-time sync across DishTV and D2H**. The system exposes a clean **.NET Core REST API** consumed by a **React** frontend, stores media metadata in **SQL Server**, and persists images in **Azure Blob Storage**. Background processing and event-driven sync are powered by **Azure Functions**, while CI/CD is handled via **Azure DevOps** and PowerShell.

---

## ✨ Key Features
- **Subscriber Management**: Registration/Login, profile, status, booking history, recommendations.
- **Movies & Events**: Create/update/delete movies and events; **slot booking** with conflict prevention.
- **Media Handling**: Upload movie images to **Azure Blob Storage**; secure URLs saved to DB.
- **Sync**: Real-time **DishTV & D2H** synchronization using **Azure Functions** (timer/queue/webhook).
- **RESTful API**: Proper HTTP verbs/status codes; structured JSON; validation; JWT-based security.

---

## 🧱 Technology Stack
- **Frontend**: React.js 18, React Router, Axios/Fetch
- **Backend**: .NET Core Web API (C#), FluentValidation/DataAnnotations, Polly, JWT
- **Database**: SQL Server 2019/2022+
- **Cloud**: Azure Blob Storage (media), Azure Functions (automation)
- **CI/CD & Ops**: Azure DevOps Pipelines, PowerShell, Artifacts, App Insights

---

## 🧭 Documentation
- **HLD**: [`HLD.md`](HLD.md)
- **LLD**: [`LLD.md`](LLD.md)
- **Architecture Diagram**: [`architecture.png`](architecture.png)

---

## 📦 Deployment Notes (Azure)
- Use **Azure DevOps Pipelines**; package to **Artifacts**; PowerShell for infra hooks.
- Prefer **Managed Identity** for Blob access; otherwise store secrets in **Key Vault**.
- Enable **App Insights**; set alerts on error rate, p95 latency, queue depth.
- Blue/green or canary deploy to API + Functions; SQL backup/restore and DR tested.

---

## 👩‍💻 Credits
**Sagarika Chakraborty** — Full Stack .NET Engineer (React.js, Web API, SQL Server)  
*Responsibilities:* API design, React UI, Azure Blob integration, DishTV/D2H sync, Azure Functions, CI/CD.

---
