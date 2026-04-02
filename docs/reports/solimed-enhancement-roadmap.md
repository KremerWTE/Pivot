# Solimed Study Tracking — Enhancement Roadmap

**Client:** Solimed (SMO — Site Management Organization)
**Platform:** Microsoft Power Apps + Power BI
**Prepared by:** Pivot (Contractor)
**Date:** 2026-04-02
**Version:** 1.1 (updated with full transcript analysis — Jan 23, 2026 demo recording)

---

## Executive Summary

Solimed's Study Tracking application is a functional clinical trial site management system currently serving 2 sites in Croatia (Solimed Clinic and Medico RI). The platform handles study lifecycle management, patient visit tracking, site budget configuration, investigator fee management, and financial reporting via Power BI. As of the January 2026 demo: **321 active patients** are enrolled across all studies, with a **current-year backlog of ~€900K** and a **total foreseeable backlog of €2.1M** (studies running through 2032).

The goal of this roadmap is to enhance the existing system and architect a scalable path to multi-country, multi-site deployment as a SaaS platform.

---

## Confirmed Pain Points (from Jan 23, 2026 Demo)

These were explicitly discussed or demonstrated during the stakeholder demo. They represent the highest-priority items for Phase 1.

| # | Pain Point | Impact | Transcript Quote |
|---|---|---|---|
| P1 | **Multi-arm study backlog inflation** | High | "The app will sum all possibilities. We'll have a study with a huge overall budget because there are several arms and the app has automatically summed all arms." |
| P2 | **Visit time normatives missing** | High | "We don't have a forecasting system within the app or any sort of normative... where we can add, for instance, visit three should be performed within one hour." |
| P3 | **Working hours tracking (in-hours vs. after-hours)** | High | "We need to upgrade the app by adding the option so that the coordinator can track whether the visit has been performed within regular working hours or outside — this will differ when it comes to investigators payment." |
| P4 | **Procedure-level drill-down missing** | Medium | "The next level would be to go level deeper to procedures and actually map out all of the procedures for visits." Workaround: extract procedures as separate visits. |
| P5 | **Screen fail ratio not tracked** | Medium | "Four screen fails for every one [enrollment]. That's not captured in the system." Drew: "You have to be careful about the screening revenue you're recognizing." |
| P6 | **Automated investigator specification sending** | Medium | "Once our finance role exports this and sends it to an investigator — now that the numbers have been correct, I think we're safe to automate this." |
| P7 | **Site filter missing on backlog dashboard** | Low | Ivan (live during demo): "It would be very useful to put in a filter here for site. I'm going to do that after the call." |
| P8 | **Patient-level backlog view missing** | Low | "Is there a view where you can look at the patient level versus the study level backlog? — Not currently, but we can build it easily." |
| P9 | **Revenue recognition for screen fails** | Medium | "Where you have high screen fail studies like gastroenterology, you have to be careful about the screening revenue you're recognizing." |
| P10 | **Time tracking is in Excel, not in the app** | Medium | "This is a spreadsheet, but it's going to be easy to update the app so we can also have time tracking... but I think it would be too much for coordinators at this point." |

---

## Current State Assessment

### Key Facts (confirmed from demo)
- **Sites:** Solimed Clinic + Medico RI (Croatia)
- **Active patients:** 321 enrolled across all studies at one site
- **Backlog:** ~€900K current year; €2.1M total across all future study periods (through 2032)
- **Studies:** Hundreds, with 15+ CROs; some studies run to 2031–2032
- **Investigators paid:** 3x per year (every 4 months); monthly specification reports sent
- **Source documentation:** Paper-based (deliberate — CROs advised against digitalizing source docs due to regulatory complexity)
- **EDC:** Solimed does NOT replicate EDC data — they track financial/operational layer only; all patient clinical data goes into CRO-owned EDC systems

### Strengths
- Working end-to-end clinical trial tracking (studies → patients → visits → billing)
- Sophisticated financial reporting (margin tracking, multi-year forecasting, CRO profitability)
- Multi-CRO support already in place (15+ CROs visible)
- Visit status workflow established (Scheduled → Done → Approved → Billed)
- Budget granularity: site budget, investigator budget, procedure budget, travel tracked separately
- "Effective from" date system for budget amendments — recalculates historical visits correctly
- Auto-scheduling: when randomization visit is marked done, all future protocol visits auto-populate with dates
- Auto-skip: when patient screen fails, all future visits auto-set to Skipped
- Calendar view built in Power BI for coordinator scheduling (not in app — pragmatic approach)
- Separate PNL dashboards for Solimed vs. Medico RI — Medico management has real-time access to their numbers
- Investigator fee model handles PI cut, PI fee, sub-investigator splits, and referral doctor fees

### Limitations (confirmed from demo)
- **Platform ceiling:** Power Apps will constrain at scale — Ivan acknowledged this and noted they plan to move to next level when needed
- **Multi-arm backlog:** App sums all arms → inflated revenue forecast for branching protocols (P1)
- **No time normatives:** Visit duration estimates not in app — currently in Excel (P2)
- **No working hours flag:** Can't distinguish in-hours vs. after-hours visits for fee calculation (P3) — *planned for February 2026*
- **No procedure drill-down:** Can't click into a visit and itemize procedures — workaround is extracting as separate visits (P4)
- **Screen fail ratio:** Not tracked — significant for revenue recognition on high screen-fail studies like gastroenterology (P5)
- **Manual investigator spec sending:** Finance exports and emails manually — automation is the next step (P6)
- **Time tracking in Excel:** Coordinators log hours in a spreadsheet connected to Power BI — not integrated into the app (P10)
- **Single locale:** Currency (€), language, and date formatting not localized
- **No mobile-first design:** Field investigators have no offline/mobile capability
- **No patient portal:** Patients are anonymous IDs only
- **No document management:** Protocol documents, consent forms, regulatory filings not tracked
- **Manual approval workflows:** Visit status changes appear manual — no automated triggers
- **No EDC integration:** No connection to industry-standard Electronic Data Capture systems
- **Limited RBAC:** Basic admin vs. user role model — insufficient for multi-country compliance

---

## Roadmap Overview

| Phase | Name | Timeline | Focus |
|---|---|---|---|
| Phase 1 | Quick Wins | Months 1–2 | High-value improvements within current platform |
| Phase 2 | Platform Hardening | Months 3–5 | Architecture for scale, security, multi-tenancy |
| Phase 3 | International Expansion | Months 6–9 | Multi-country deployment, localization |
| Phase 4 | Ecosystem Integration | Months 10–14 | EDC, billing, patient portal, mobile |
| Phase 5 | SaaS Productization | Months 15–20 | Full multi-tenant SaaS with marketplace |

---

## Phase 1 — Quick Wins (Months 1–3)

**Goal:** Deliver immediate value without platform migration. All work within Power Apps + Power BI. Priority order is driven directly by confirmed pain points from the Jan 23, 2026 demo.

### 1.1 Working Hours Flag on Visits *(P3 — Planned for Feb 2026, accelerate)*
Ivan confirmed this is the **#1 in-progress feature**. The coordinator must be able to indicate whether a visit was performed within or outside regular working hours, as this directly affects investigator fees.
- Add a binary "In hours / After hours" toggle on each visit log entry
- Fee calculation engine: apply the correct investigator rate based on the flag
- Site budget impact: some sites are also compensated differently for after-hours use
- Retroactive correction support: flag can be set/changed before visit reaches "Approved" status

### 1.2 Multi-Arm Study Backlog Fix *(P1 — Highest financial impact)*
The current backlog inflates revenue for studies with multiple protocol arms by summing all possible arms per patient. This makes the €900K / €2.1M backlog figures unreliable for studies with branching protocols.
- Add arm assignment per patient: coordinator marks which arm a patient entered after randomization
- Backlog calculation only includes the assigned arm's visits — not all arms
- For unassigned patients (pre-randomization): show a configurable split estimate or flag as "unassigned arm"
- Dashboard flag: studies with unresolved multi-arm patients are highlighted until arms are assigned

### 1.3 Screen Fail Ratio Tracking *(P5 — Revenue recognition risk)*
Raised by Drew (CFO background) as a critical financial control point, especially for gastroenterology-type studies with high screen fail rates.
- Track screen fail allotment per study per contract (configurable field on site study setup)
- Running counter: screen fails used vs. allotment remaining
- Alert: when screen fails exceed allotment, flag coordinator and finance that CRO may not cover further screening costs
- Screen fail revenue recognition report: separate "billable screen fails" from "over-allotment screen fails" in PNL

### 1.4 Coordinator Dashboard (To-Do View)
- Personal task list: all upcoming visits across all studies due in next 7/14/30 days
- Overdue visit alerts (visits past tolerance window still in Scheduled status)
- One-click status update from dashboard

### 1.5 Visit Tolerance Warning System
- Visual flags when a visit date is approaching or past its tolerance window (e.g., ±7 days)
- Color-coded status: Green (on time), Yellow (within 3 days of tolerance edge), Red (out of window)

### 1.6 Bulk Status Operations
- Select multiple visits → bulk mark as Done / Approved / Billed
- Currently requires one-by-one updates — significant time sink for coordinators

### 1.7 Power BI Enhancements *(includes P7, P8 — Site filter + patient-level backlog)*
- **Site filter on backlog dashboard** (Ivan noted live during call: "I'm going to do that after the call" — we do it properly)
- **Patient-level backlog view**: drill from study-level backlog down to individual patient's scheduled visits and expected revenue
- Add gross margin % trend line per study over time
- CRO comparison dashboard (side-by-side profitability across CROs)
- Investigator workload balance report (hours/visits per investigator vs. budget)
- Export to PDF/Excel buttons on all dashboards

### 1.8 Automated Investigator Specification Sending *(P6 — Ready to automate)*
Ivan confirmed: "In the last two or three months all of the numbers have been correct. I think we're now safe to automate this." Currently finance exports manually and emails each investigator.
- Auto-generate monthly investigator payment specification from approved visits
- Each investigator has contact details already in the app — use these for delivery
- Email template: investigator name, month, list of patients × visits × fees, total owed
- Delivery: auto-send on a configured date each month (e.g., last day of month)
- Finance review gate: optional — finance can review before send, or configure auto-send
- Investigator self-verification loop: when investigator replies with a discrepancy, create a correction ticket in the app for finance to resolve

### 1.9 Audit Trail
- Log all budget changes: who changed what, when, from/to values
- Log all visit status changes with timestamp and user

### 1.10 Study Progress Summary Card
- Per-study: % visits completed, % budget consumed, days remaining, visits at risk
- Visible on the Studies list screen

### 1.7 Site Budget Management — Fix & Override
Currently, site budgets are configured per-study in the Admin Area and require navigating deep into each study's config. This enhancement adds:
- **Site-level budget editor:** Set or override base budget values at the site level (e.g., Solimed Clinic default archiving fee, default startup fee) that auto-populate for new studies — reducing manual re-entry per study
- **Budget correction log:** When a budget value is corrected mid-study, log the original value, corrected value, reason, and approver
- **Budget lock / unlock control:** Sites can lock a study's budget to prevent edits after CRO confirmation; explicit unlock required to make further changes (mirrors the existing Study "Locked/Open" status model but applied to budgets)
- **Cross-study budget comparison:** Side-by-side view of budget values across studies at the same site — quickly identify outliers or misconfigured studies

### 1.8 Demand & Time Tracking
The app tracks investigator hours at a high level but lacks forward-looking demand visibility:
- **Visit demand forecast:** Based on scheduled visits across all studies, show expected coordinator and investigator hours required per week/month for the next 90 days
- **Time-per-visit norms:** Configure expected time per visit type (e.g., Regular = 1.5 hrs, Virtual = 0.5 hrs, SCR = 2 hrs) and auto-calculate planned hours from scheduled visits
- **Actual vs. planned time:** Compare logged hours against the norm for each visit type — flag over/under-time patterns
- **Capacity planning view:** Per-coordinator and per-investigator: available hours vs. committed hours vs. visited hours — surfaced as a weekly heatmap

### 1.9 Study Show / Stop Controls
Currently, study status is either "Open" or "Locked" with no intermediate states. Add a proper lifecycle control:

| Status | Meaning | Who Can Set |
|---|---|---|
| **Draft** | Study created but not yet active | Admin |
| **Open** | Active — visits can be scheduled and logged | Admin |
| **On Hold (Show)** | Temporarily paused — visits visible but no new scheduling | Admin |
| **Stopped** | Study halted — no new activity; all data read-only | Admin |
| **Locked** | Completed and archived — immutable | Admin |

- **Show (On Hold):** Study is visible in all dashboards and reports but coordinators cannot log new visits or change status — used during protocol amendments or regulatory holds
- **Stop:** Immediately halts all activity; triggers notification to all assigned coordinators and investigators; generates a Stop Report (date, reason, open visits at time of stop)
- **Stop Report:** Auto-generated document listing all visits that were Scheduled or In Progress at the time of stop — enables clean reconciliation with CROs

### 1.10 Overall Budget, Revenue & Budget Normita Dashboard
The existing Power BI dashboards show financial data per site, per CRO, and per year — but there is no single unified view. Add:

**Overall Budget & Revenue Summary:**
- **Platform-wide financial overview:** Total budget committed vs. total revenue expected vs. actual revenue received — across all sites, all CROs, all studies, all years
- **Revenue waterfall:** Breakdown of total revenue by: Visit Revenue, Procedure Revenue, Site Fees, Referral Fees, Travel Reimbursements
- **Budget utilization rate:** % of total committed budget that has been invoiced, approved, and paid — per site and in total
- **Cash flow timeline:** Month-by-month projection of expected revenue based on scheduled visits — 12-month rolling forecast

**Budget Normita (Time-Derived Visit Norms):**
Confirmed definition from Ivan in the demo: *"The normatives can be derived from the budget of each visit — because obviously if the visit has more procedures, it's demanding, it will take a longer time."* Drew (CFO background) confirmed this aligns with his approach: *"That's how traditionally I've tried to build budgets — using time. As you're negotiating with the CRO and sponsor, you build up the visit from the bottom up. Visit one might have five procedures — total time tells you what to charge."*

This feature bridges budget configuration and time planning:
- **Normita baseline per visit type:** Derive an expected duration from the visit's investigator budget + procedure budget using a configurable €/hour rate — gives coordinators a time expectation without requiring them to estimate independently
- **Complexity tier as starting point:** Ivan's team already categorizes visits as complex/non-complex. Add a third tier. Map each tier to a time range (e.g., Simple: 30–60 min, Standard: 1–2 hrs, Complex: 2–4 hrs). Normita refines within that range using actual budget.
- **Actual vs. Normita tracking:** Once coordinators log hours per visit (Phase 1.12), compare actual time vs. normita — flag outliers for review
- **Normita-based negotiation tool:** Generate a visit-by-visit time breakdown from budget → use as input for CRO/sponsor fee negotiation (Drew's request)
- **Normita change history:** Track when normita values were updated, who updated them, and the business justification

---

## Phase 2 — Platform Hardening (Months 3–5)

**Goal:** Establish the technical foundation needed to support multiple countries and enterprise-grade security.

### 2.1 Architecture Decision: Extend vs. Migrate

**Recommendation:** Begin a **parallel build** of a proper web application backend (API layer) while keeping Power Apps as the UI for continuity. This avoids a big-bang migration and lets Solimed continue operating.

**Target architecture:**
```
Frontend: Power Apps (near-term) → React/Next.js (medium-term)
API Layer: Azure Functions or Node.js/FastAPI REST API
Database: Azure SQL (already likely in use via Power Apps connectors)
Reporting: Power BI (retain — it's best-in-class for this use case)
Auth: Azure AD B2C (multi-tenant, multi-country identity)
Storage: Azure Blob (documents, attachments)
```

### 2.2 Multi-Tenant Data Model
- Introduce `tenant_id` / `country_id` / `site_id` hierarchy throughout data model
- Each country/site gets isolated data with shared schema
- Role model: Platform Admin → Country Admin → Site Admin → Coordinator → Investigator → Read-Only

### 2.3 Role-Based Access Control (RBAC) Expansion
| Role | Capabilities |
|---|---|
| Platform Admin | All tenants, all config |
| Country Admin | All sites in their country |
| Site Admin | Their site: budget config, user management |
| Coordinator | Their studies: visit logging, patient management |
| Investigator | Their assigned visits only |
| Finance | Read-only financial dashboards |
| CRO User | Read-only view of their studies |

### 2.4 Security & Compliance Baseline
- Data encryption at rest and in transit
- PII audit logging (who accessed patient data, when)
- Session timeout policies
- GDPR consent tracking framework (required for EU expansion)

### 2.5 API Layer
- RESTful API exposing all core entities (studies, sites, patients, visits, budgets)
- Enables future mobile app, EDC integrations, and third-party access
- OpenAPI/Swagger documentation

---

## Phase 3 — International Expansion (Months 6–9)

**Goal:** Deploy to first new country with full localization and regulatory compliance.

### 3.1 Localization Framework (i18n)
- All UI strings externalized to language files
- Priority languages: English, Croatian (current), + target country language
- Date format: locale-aware (DD/MM/YYYY vs. MM/DD/YYYY)
- Number format: locale-aware (1.000,00 vs. 1,000.00)

### 3.2 Multi-Currency Support
- Currency stored per site/country (not hardcoded to €)
- FX rate table with daily/weekly refresh from ECB or similar
- All financial reports show both local currency and base currency (€ or $)
- Historical FX rates preserved for accurate historical reporting

### 3.3 Country-Specific Regulatory Configuration
- Per-country data residency setting (Azure region selection per tenant)
- Country-specific required fields for regulatory submissions
- Configurable visit protocol templates per country/therapeutic area
- Privacy law flags: GDPR (EU), HIPAA (US), LGPD (Brazil), etc.

### 3.4 Tax & Billing Rules Engine
- Configurable tax rates per country
- Country-specific invoicing format requirements
- VAT handling for EU countries

### 3.5 Country Onboarding Playbook
- Standardized checklist for launching in a new country:
  - Regulatory review
  - Data residency configuration
  - Currency setup
  - User provisioning
  - CRO mapping
  - Pilot study selection

---

## Phase 4 — Ecosystem Integration (Months 10–14)

**Goal:** Connect Solimed to the broader clinical trial ecosystem.

### 4.1 EDC System Integration
- **Target systems:** Medidata Rave, Veeva Vault EDC, REDCap, Oracle Clinical One
- Sync patient visit status from EDC → Solimed (eliminate double entry)
- Push financial/billing events from Solimed → EDC

### 4.2 Automated Invoicing
- Generate invoices automatically when visits reach "Approved" status
- Invoice templates per CRO/country
- PDF generation and delivery via email
- Integration with accounting systems (QuickBooks, SAP, local ERP per country)

### 4.3 Mobile Application (Investigator)
- Native iOS/Android app for investigators
- Offline-first: log visits without internet, sync when connected
- Key screens: My scheduled visits today, Visit check-in/check-out, Notes, Photo capture for source documents
- Push notifications for upcoming visits

### 4.4 Patient Portal (Optional — regulatory review required)
- Patient-facing portal for: upcoming visit reminders, consent re-confirmation, travel reimbursement requests
- SMS/email notification system
- Travel expense claim submission with receipt upload

### 4.5 Document Management
- Protocol documents, informed consent forms, regulatory approvals stored per study
- Version control on all documents
- Expiry tracking (ethics approvals, insurance certificates)
- E-signature integration (DocuSign or Adobe Sign)

---

## Phase 5 — SaaS Productization (Months 15–20)

**Goal:** Transform Solimed from a custom app into a scalable, sellable SaaS product.

### 5.1 Self-Service Tenant Onboarding
- New site/country can sign up, configure, and be operational without manual intervention
- Guided setup wizard: site details → CROs → users → first study

### 5.2 Subscription & Billing Model
- Tiered pricing: per-site, per-study, or per-active-patient
- Automated billing (Stripe or similar)
- Usage dashboards for clients

### 5.3 Marketplace / CRO Network
- CROs can view their studies across all SMO sites on the platform
- Shared CRO directory: new sites can onboard existing CROs without re-entry

### 5.4 AI/ML Features
- Visit no-show prediction (based on historical patterns)
- Budget overrun early warning
- Protocol deviation detection
- Automated visit scheduling optimization

### 5.5 Regulatory Intelligence Layer
- Country-specific regulatory change monitoring
- Alerts when a regulation changes that affects active studies

---

## Success Metrics

| Phase | Key Metrics |
|---|---|
| Phase 1 | Coordinator time-on-task reduced by 30%; zero missed tolerance windows |
| Phase 2 | API test coverage >80%; RBAC fully enforced; zero cross-tenant data leakage |
| Phase 3 | First new country live; <2 week onboarding time for new site |
| Phase 4 | EDC sync eliminates double-entry; invoicing time reduced 80% |
| Phase 5 | 5+ countries live; self-serve onboarding <24hrs; NPS >50 |

---

## Technology Stack Recommendation

| Layer | Recommendation | Rationale |
|---|---|---|
| Frontend | React / Next.js | Scalable, component-based, SSR for performance |
| Mobile | React Native | Code share with web frontend |
| API | Node.js (Express/Fastify) or Python (FastAPI) | Fast development, strong ecosystem |
| Database | Azure SQL + Azure Cosmos DB | Relational for transactions, document store for config |
| Auth | Azure AD B2C | Already in Microsoft ecosystem, enterprise RBAC |
| Reporting | Power BI Embedded | Best-in-class, already in use |
| Storage | Azure Blob Storage | Documents, attachments |
| Search | Azure Cognitive Search | Study/patient search at scale |
| Messaging | Azure Service Bus | Async events (visit status changes, invoice triggers) |
| Hosting | Azure App Service / AKS | Scale per region |

---

*This roadmap will be updated when the full meeting transcript is available to incorporate Solimed's stated priorities and pain points.*
