+++
title = "eGarage"
description = "The modern digital vehicle checklist & management platform for automotive workshops, featuring live UK DVSA MOT integration. A core sub-project of Veylor HQ."
weight = 0
[extra]
local_image = "/projects/egarage.png"
+++

# eGarage — Digital Workshop Operations & DVSA MOT Platform
- **Official Website**: [https://egarageapp.uk/](https://egarageapp.uk/)
  
**Part of the Veylor HQ Ecosystem | Built for UK Garages • Live DVSA MOT Integration**

[eGarage](https://egarageapp.uk/) is the B2B workshop management application within Veylor's dual-tier automotive suite. It transforms how automotive workshops and mobile mechanics inspect vehicles, manage customer records, and share digital reports. Paper inspection sheets are lost, difficult to read, and lack transparency. eGarage modernises shop floor workflows with fast digital checklists, instant WhatsApp dispatch, and automated DVSA MOT reminders.

It's a **successful and profitable** project that has been deployed for more than half a year, through active development and having paying customer(**1 paying customer completely covers all infrastructure expenses**). Velo used primarily only by myself and my friends, but I have a massive list of interested people who want to use it. 
eGarage now requires almost zero effort to maintain it how it is, main objective would be gaining more customers and polishing the app to make it even better, or plan to unroll full-scale Garage Management System with more features and capabilities.

<img alt="eGarage Mobile & DVSA MOT Interface" async src="/projects/egarage.png" style="width: 100%; border-radius: 8px; margin: 1.5em 0;" />

---

## Core Capabilities

- **Digital Checklists**: Create custom inspection forms with tick boxes, comments, Green/Amber/Red status indicators, and dedicated tyre tread depth selector widgets (with automatic threshold highlighting: Red ≤ 1.6mm, Amber ≤ 3.0mm).
- **Vehicle History Logs**: A persistent chronological record of all checklist inspections, repairs, and odometer logs linked directly to each vehicle's license plate.
- **Customer CRM**: Link vehicles to customer profiles, featuring automatic uppercase license plate formatting and history tracking.
- **Team Workspaces**: Multi-mechanic collaboration under a shared workshop profile with custom garage logo, branding colors, and role-based permissions.
- **Photo Evidence Integrations**: Capture vehicle conditions directly on the shop floor via camera or native picker to document component wear or damage.
- **Branded Web Link Sharing**: Dispatch interactive digital inspection reports to clients via WhatsApp or SMS.
- **Automated DVSA MOT Reminders**: Schedule self-driving customer email alerts (30 days, 7 days, 3 days before expiration) backed by automatic background DVSA MOT API status validation.

---

## Workshop Operational Impact

- **Paperless Workflow**: Eliminates carbon copy pads, grease-stained notes, and lost files.
- **Time Savings**: Saves **30+ minutes per job** on administrative paperwork.
- **Instant Client Approval**: Clear visual status badges and photo evidence build client trust and accelerate job authorizations.
- **Shop Floor Hardware Ready**: Built for tablets and smartphones with high-contrast UI and touch-optimised inputs.

---

## Customer Case Study: W M Autos Mobile Mechanic

> *"eGarage saved me over 30 minutes of paperwork on every single job. I don't have to keep duplicate paper carbon copies anymore, and my customers love getting an interactive report with photos and MOT countdowns right on their phones."*
> 
> — **Westley Miller**, Owner of W M Autos Mobile Mechanic

---

## Architecture & Technical Deep Dive

### High-Level Architecture Overview
eGarage operates as part of the monorepo ecosystem sharing core infrastructure with Velo. It utilizes a modular multi-tenant architecture with strict data isolation, role-based workspace boundaries, and real-time synchronization between client applications and backend micro-services.

<img alt="eGarage High-Level System Architecture Diagram" async src="/projects/egarage_architecture.png" style="width: 100%; border-radius: 8px; margin: 1.5em 0;" />

### Technology Stack

#### Backend & Infrastructure
- **Framework**: Python 3.11+ using **FastAPI** & **Uvicorn** for async HTTP throughput.
- **Database & ODM**: **MongoDB** with **Beanie ODM** (Async Pydantic-backed data mapping) powering document collections for Workspaces, Checklists, Vehicles, and Service Records.
- **Caching & Rate Limiting**: **Redis 7** handling access rate limits, token revocation checks, and lookup query caching.
- **Containerization & Deployment**: Dockerized runtime (`docker-compose.yml`) integrated into CI/CD workflows via GitHub Actions (`deploy-egarage.yml`) and proxy routing via Dokploy / Nginx.
- **Monitoring & Resilience**: Integrated **Sentry SDK** for error tracking, structured Logging, and healthcheck endpoints.

#### Frontend & Cross-Platform Clients
- **Mobile & Web App**: Built with **Flutter 3.x** and **Dart**, leveraging **Riverpod 3** for state management, **GoRouter** for routing, and cross-platform native hardware integration (Camera, File Picker, Secure Storage).
- **Shared Code Architecture**: Shared business logic, models, and API client layers extracted into a local pub package (`apps/shared`) reused across eGarage and companion apps.
- **Internal Admin & Reporting**: Light Jinja2/HTML5 server-side rendered admin panel (`/admin`) for workspace provisioning, feature flags (Flagsmith), and metric tracking.

---

## Infrastructure, DevOps & Zero-Trust Deployment

eGarage employs a cost-efficient, high-performance infrastructure design engineered to minimize CPU overhead on production nodes while maintaining zero-trust security and high availability.

### Production Topology & Deployment Pipeline

<img alt="eGarage Infrastructure, DevOps & Zero-Trust Deployment Topology" async src="/projects/egarage_devops.png" style="width: 100%; border-radius: 8px; margin: 1.5em 0;" />

### Infrastructure Highlights

- **Hetzner CPX22 Host Server**: Hosted on a 2 vCPU / 4 GB RAM Hetzner CPX22 instance managed via Dokploy PaaS, hosting the FastAPI monolith, static Nginx PWA container, MongoDB, and Redis.
- **Resource-Optimized Offloaded CI/CD**: Flutter web compilation is notoriously resource-heavy. To keep the primary CPX22 production node performant under load, Flutter PWA compilation is offloaded to GitHub Actions using a 2-stage build (`cirruslabs/flutter:stable` → `nginx:alpine`). The compiled artifact is pushed to an offloaded Docker Registry hosted on an Oracle Cloud Free Tier Instance, after which a webhook triggers Dokploy on Hetzner to pull the pre-built image.
- **Zero-Trust Security & DB Inspection**: Production databases (MongoDB and Redis) have no exposed public ports. All administrative SSH access, database inspection, and debugging occur securely through a private Tailscale VPN mesh network.
- **Edge Routing & SSL via Cloudflare**: All client-facing traffic to `egarageapp.uk` is proxied through Cloudflare for edge SSL/TLS encryption, DDoS protection, rate limiting, and static asset caching.
- **Reliable Transactional Emails**: Email notification dispatches and automated MOT reminders utilize Brevo SMTP (formerly Sendinblue) with fallback error routing and delivery tracking.

---

## Technical & Ecosystem Context

- **Platform Architecture**: Operates on a shared high-performance backend alongside Velo, utilizing [Veylor Relay](https://github.com/veylor-hq/relay) for zero-trust PII isolation and identity-decoupled notification proxying.
- **Official Website**: [https://egarageapp.uk/](https://egarageapp.uk/)