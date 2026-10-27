# WCDP – Mobile & Windows Application (Workforce Management)

> **Client:** Dry Wall, West Coast, California, USA  
> **Domain:** Employee Mobile and Admin Desktop Application (Time Tracking, Attendance & Task Management)  

**WCDP** is a cross-platform workforce management suite built with **Xamarin.Forms** (Android, iOS, Windows desktop) using **C#** and **MVVM**. It enables employees to punch timesheets, track timecards and attendance, and receive task assignments, while admins manage tasks, users, and reports on a Windows desktop app. The solution integrates with secure APIs, supports data synchronization, and is built and released through **Azure DevOps** and **Jenkins** pipelines.

---

## ✨ Key Features
- **Mobile (Android/iOS)**: Punch in/out, timecards, attendance, assigned tasks, offline cache & sync.
- **Windows Admin App**: Task assignment/monitoring, workforce analytics dashboards, attendance and timecard reporting.
- **MVVM & XAML**: Clean separation, reusable styles/themes, converters & templates for consistent UI/UX.
- **Secure API Integration**: Auth tokens, role-based views; robust sync with conflict handling.
- **CI/CD**: Azure DevOps and Jenkins pipelines for automated build, test and release; app distribution via Play Store and App Store, plus a Windows installer for the admin app.

---

## 🧱 Technology Stack
- **UI/Framework**: Xamarin.Forms (Android, iOS, Windows) with MVVM
- **Language**: C#
- **Build & Release**: Azure DevOps pipelines, Jenkins
- **Auth & Security**: JWT/OAuth tokens, role-based access, HTTPS
- **Data**: REST/GraphQL APIs; SQLite for local cache; SQL Server/PostgreSQL backend (pluggable)

---

## 🧭 Documentation
- **HLD**: [`HLD.md`](HLD.md) — architecture, modules, flows, security, CI/CD
- **LLD**: [`LLD.md`](LLD.md) — MVVM layers, page/viewmodel map, sync contracts, error codes
- **Diagram**: [`architecture.png`](architecture.png)

---

## 📦 Distribution
- **Mobile**: Google Play Store, Apple App Store (via App Store Connect/TestFlight).
- **Desktop**: Windows installer built by the release pipeline.

---

## 👩‍💻 Credits
**Sagarika Chakraborty** — Cross-platform development (Xamarin.Forms + MVVM), secure API integration & sync, Azure DevOps and Jenkins CI/CD, store releases, and admin desktop tooling.
