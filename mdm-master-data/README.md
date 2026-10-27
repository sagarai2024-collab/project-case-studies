# MDM – Supplier & Customer Master Data (React 17 + .NET 6 + SAP via Dell Boomi)

**MDM** is a web-based Master Data Management system for **suppliers** and **customers**. It provides **creation, modification, approval workflows**, and **SAP ERP synchronization** via **Dell Boomi**. Dynamic, conditional forms guide users through partner onboarding, while dashboards expose **drafts**, **pending approvals**, and **blocked** entities. Supplier integration leverages **Ariba** (except the “v006 Employees” case), while **Germany SMR** and **Spain SMR** partners are managed **exclusively** within MDM.

---

## ✨ Key Features
- **Supplier & Customer Management** with modular tabs (Details first; new tabs & Submit appear after initial save).
- **Approval Workflow**: Draft -> Submit (buttons hidden while awaiting decision) -> Approve/Disapprove with audit trail.
- **Synchronization**: Post-approval **SAP** sync via **Dell Boomi**. Active (Company Code) toggle controlled by SAP state.
- **Dashboards**: Draft, Pending/Disapproved, and Blocked lists for visibility and governance.
- **Business Partner Numbering**: BP No. generated per **EntityConfig** (Address Book Type).
- **Integration Rules**: **Ariba API** owns supplier flows except “v006 Employees”; **MDM** owns customer flows; **SMR (DE/ES)** fully in MDM.

---

## 🧱 Technology Stack
- **Frontend**: React.js 17
- **Backend**: .NET 6 Web Application & Web API
- **Database**: SQL Server
- **Integration**: Dell Boomi (SAP), Ariba API
- **DevOps**: SourceTree (Git), environment-based configs
- **Modules**: MDM-SMP and MDM-SMR

---

## 🧭 Documentation
- **HLD**: [`HLD.md`](HLD.md)
- **LLD**: [`LLD.md`](LLD.md)
- **Architecture Diagram**: [`architecture.png`](architecture.png)

---

## 👩‍💻 Credits
**Sagarika Chakraborty** — Full Stack .NET Engineer (React.js, Web API, SQL Server)  
*Responsibilities:* Dynamic BP forms, approval workflow & dashboards, Dell Boomi & SAP integration, Ariba supplier flow alignment, SMR country-specific handling, release management with SourceTree.

---
