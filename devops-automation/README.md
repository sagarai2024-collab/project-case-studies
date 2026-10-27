# DevOps & Automation — Azure DevOps CI/CD, PIM Gates, Slot Swaps, and Observability

This repository documents a production-grade DevOps and automation setup for .NET Core APIs and Azure Functions. It includes Azure DevOps YAML pipelines, gated approvals through Azure AD PIM, slot-based deployments, proxy routing for secure egress, package management with Azure Artifacts, and end-to-end observability with Application Insights.

---

## Highlights
- CI/CD in Azure DevOps for automated build, test, package, and deploy.
- Proxy server integration for secure, controlled access to external APIs.
- PowerShell automation for environment bootstrap and post-deploy tasks.
- Azure AD PIM approvals and audit trails for production elevation.
- Azure Artifacts for versioned internal packages.
- Slot-based deployments (Staging -> Production) with zero-downtime swap.
- Application Insights dashboards, alerts, and autoscale hooks.
- End-to-end YAML pipelines with build, test, artifact creation, staging deploy, PIM approval gate, and production slot swap.

---

## What you will find here
- HLD and LLD describing the architecture, security, and release process.
- Opinionated guidance on rollback, secrets, and observability.
