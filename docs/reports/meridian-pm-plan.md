# Meridian — Product Development PM Plan
## Unified Clinical Trial Intelligence Platform

**Product:** Meridian (by Pivot)
**Document Type:** Product PM Plan
**Prepared by:** Pivot
**Date:** 2026-04-02
**Version:** 2.0 — Compressed (18-Month)
**Horizon:** 18 months (Months 1–18)

---

## 1. Product Overview

Meridian is a unified clinical trial intelligence platform built across five layers:

| Layer | Name | Core Capability |
|---|---|---|
| 5 | AI Intelligence | Predictive patient scoring, dropout risk, revenue forecasting, protocol deviation detection |
| 4 | Communication Hub | AI voice/SMS/video, real-time coordinator coaching, sentiment analysis, call summaries |
| 3 | Clinical Operations | Study/visit management, eSource, eConsent, regulatory binder, payment configuration |
| 2 | Payment Engine | Investigator fees, CRO invoicing, patient reimbursements, cash flow forecasting |
| 1 | Data Ingestion Hub | EHR (FHIR), EDC sync, lab systems, wearables, claims data, registries |

**Design partner / first customer:** Solimed (Croatia-based SMO, 2 sites, 321 active patients, 15+ CROs)
**Market target:** SMOs and independent research sites managing 5–500 active studies
**Year 3 ARR target:** €600K+ (50 sites × €1,000/month average)

---

## 2. How 18 Months Was Achieved

The original plan was 24 months. Six months were removed through four specific changes:

| Change | Months Saved |
|---|---|
| Phase 0 runs in parallel with Sprint 1 — not sequentially | ~1 month |
| Phase 1 scoped to Layers 1–3 MVP only; Layers 4–5 deferred to Phase 2 | ~3 months |
| Second Full-Stack Developer added from Sprint 1 | ~2 months |
| Phase 2 absorbs Layers 4–5 while simultaneously onboarding second site | Enables parallel tracks |

**What was NOT cut:**
- Data quality and migration rigor — rushing data migration creates production incidents
- Mladen knowledge transfer (Sprint 1–2 is still 100% dedicated to this)
- Multi-tenancy rewrite in Phase 2 — this cannot be shortcut
- Security and compliance gates before Phase 3

---

## 3. Development Philosophy

**Build with, not for.** Solimed is not just a client — they are the development partner. Every feature built in Phase 1 is validated against real clinical trial operations before we declare it market-ready.

**Layer-first, not feature-first.** Each layer must be stable before the layer above it is built. You cannot build AI predictions (Layer 5) without reliable data (Layer 1) and a complete operational record (Layers 2–3).

**Ship working software every sprint.** No sprint ends without a working, demonstrable increment. Coordinators and investigators use what we build in production — not in a demo environment.

**Solimed's constraints are the platform's constraints.** If something is too complex for Solimed's coordinators, it's too complex for any site. Build for the least-technical user on the most demanding protocol.

---

## 4. Phases Overview

| Phase | Name | Duration | Primary Deliverable |
|---|---|---|---|
| **Phase 0** | Foundation | Weeks 1–2 (parallel to Sprint 1) | Team, tooling, access, architecture decisions |
| **Phase 1** | Solimed MVP | Months 1–6 / Sprints 1–12 | Layers 1–3 live at Solimed; coordinators fully off Power Apps |
| **Phase 2** | Second Site + AI | Months 7–11 / Sprints 13–22 | Layers 4–5 added; second SMO onboarded; AI models trained |
| **Phase 3** | Market Launch | Months 12–18 / Sprints 23–36 | Self-serve; subscription billing; 5+ sites; mobile app |

---

## 5. Phase 0 — Foundation (Weeks 1–2, Parallel to Sprint 1)

**Goal:** Set the team up to build. Phase 0 runs simultaneously with Sprint 1 — access provisioning and architecture decisions happen while Sprint 1 discovery is underway. No features ship in Phase 0.

**Key change from v1.0:** Phase 0 is 2 weeks, not 4. Week 1 (access) overlaps 100% with Sprint 1. Week 2 (ADRs + environment) overlaps with Sprint 1 tail-end. This eliminates the 4-week dead zone before development starts.

### Week 1: Access & Discovery (Runs Parallel to Sprint 1)

| Task | Owner | Done When |
|---|---|---|
| NDA signed with Solimed | Pivot PM | Signed document on file |
| Phase 1 SOW signed | Pivot PM + Ivan Kruljac | Signed SOW on file |
| Power Apps read access provisioned for Pivot | Mladen Geng | Pivot can view all screens and data |
| Power BI workspace access provisioned | Mladen Geng | Pivot BI engineer can open data model |
| Dataverse read access or full export | Mladen Geng | Schema documented |
| Azure subscription confirmed or provisioned | Ivan Kruljac | Azure tenant ID shared with Pivot |
| Clarify WTE Solutions engagement status | Ivan Kruljac | Written confirmation of status |
| GitHub repo created: `pivot-meridian` | Pivot Tech Lead | Repo live, branch strategy defined |
| Linear / Jira project created | Pivot PM | Backlog created; all team members invited |
| Communication channels set up (Teams/Slack) | Pivot PM | Channels live; Solimed team added |

### Week 2: Architecture Decisions + Environment (Overlaps Sprint 1 Close)

The following decisions must be made and documented before a line of code is written:

| Decision | Options | Deadline |
|---|---|---|
| API language: Node.js (Fastify) vs. Python (FastAPI) | Performance vs. ML team preference | Week 2 |
| Database: PostgreSQL vs. Azure SQL vs. split | Cost, HIPAA, team familiarity | Week 2 |
| Patient profile store: MongoDB vs. PostgreSQL JSON | Flexibility vs. consistency | Week 2 |
| Auth provider: Auth0 vs. Azure AD B2C | Cost at scale, Microsoft ecosystem | Week 2 |
| Communications: Twilio vs. Azure Communication Services | HIPAA BAA, feature set, cost | Week 2 |
| FHIR layer: Azure Health Data Services vs. build own | Cost vs. control | Week 2 |
| Frontend: Next.js only vs. Next.js + React Native | Mobile priority timeline | Week 2 |
| BI reporting: Power BI Embedded vs. build custom | Solimed compatibility, cost | Week 2 |

| Environment Task | Owner | Done When |
|---|---|---|
| Azure Dev + Staging + Prod environments provisioned | Pivot DevOps | All 3 environments live |
| CI/CD pipeline: GitHub Actions → Staging | Pivot Tech Lead | Auto-deploy on merge to `main` |
| Database schemas created (Dev) | Pivot Tech Lead | Migration scripts run cleanly |
| API skeleton running (health check endpoint live) | Pivot Tech Lead | `/health` returns 200 |
| Auth0 / Azure AD B2C tenant configured | Pivot Tech Lead | Test user can log in |
| Sprint 1 backlog groomed and estimated | Pivot PM + team | All Sprint 1 stories estimated and ready |

---

## 6. Phase 1 — Solimed MVP (Months 1–6 / Sprints 1–12)

**Goal:** Build Layers 1–3 in production at Solimed. By end of Sprint 12, coordinators are fully off Power Apps for daily operations. Layers 4 (Communication) and 5 (AI) are deferred to Phase 2 — this is the primary compression decision.

**What is deferred vs. delivered:**

| Layer | Phase 1 | Deferred to |
|---|---|---|
| Layer 1 — Data Ingestion | ✅ Full data migration + EDC sync | — |
| Layer 2 — Payment Engine | ✅ Full fee engine + invoicing + cash flow | — |
| Layer 3 — Clinical Operations | ✅ Full coordinator UI; coordinators off Power Apps | — |
| Layer 4 — Communication Hub | ❌ Deferred | Phase 2 Sprint 13–16 |
| Layer 5 — AI Intelligence | ❌ Deferred (except backlog quality + revenue ladder) | Phase 2 Sprint 17–20 |

**Team:** 2 Full-Stack Developers from Sprint 1 (vs. 1 in original plan) — this is the primary calendar compression.

### Sprint Cadence
- **2-week sprints**
- Sprint planning: Monday Week 1
- Mid-sprint check-in: Wednesday Week 1
- Sprint review / demo to Ivan + Drew: Friday Week 2
- Retrospective: Internal Pivot only, Friday Week 2

---

### Layer 1 — Data Ingestion Hub (Sprints 1–3)

**Goal:** Establish the authoritative data store. Migrate Solimed from Power Apps/Dataverse to Meridian's PostgreSQL backend. Compressed from 4 sprints to 3 via second developer.

| Sprint | Deliverables |
|---|---|
| **Sprint 1** | Knowledge transfer: all Solimed data model, business logic, DAX measures, edge cases documented with Mladen; data quality audit complete; test data flagged |
| **Sprint 1** | Database schema v1: Studies, Site Studies, Patients, Visits, Investigators, Budgets — modelled from Solimed Dataverse ERD |
| **Sprint 2** | Data migration script: full Dataverse export → PostgreSQL import with validation; dry-run on staging; record counts match; no data loss |
| **Sprint 2** | Multi-tenant schema: `tenant_id` + `country_id` + `site_id` on all tables; row-level security enforced from Day 1 |
| **Sprint 3** | Excel time tracking migration: historical time tracking data imported into PostgreSQL `time_entries` table |
| **Sprint 3** | EDC sync adapter v1: read-only pull of visit status from one pilot CRO EDC system (selected with Ivan) |
| **Sprint 3** | Data ingestion API: REST endpoints for external data push; webhook receiver for EDC event notifications |
| **Sprint 3** | FHIR adapter stub: Azure Health Data Services provisioned; basic FHIR R4 patient resource read (for Phase 2 EHR integration) |

**Milestone M1.1:** All Solimed data live in Meridian PostgreSQL; Power Apps reads from Meridian API (not Dataverse) — end Sprint 3

---

### Layer 2 — Payment Engine (Sprints 3–7)

**Goal:** Full investigator fee management, CRO invoicing, patient reimbursements, and cash flow forecasting. Compressed from 6 sprints to 5 via second developer running payment logic in parallel with Layer 1 close-out.

| Sprint | Deliverables |
|---|---|
| **Sprint 3** | Investigator fee engine v1: PI fee, PI cut %, sub-investigator splits, referral doctor fee — all calculated from API |
| **Sprint 4** | Working hours flag: in-hours vs. after-hours toggle on visit record; fee engine applies correct rate per flag |
| **Sprint 4** | Screen fail allotment engine: contractual allotment per study; running counter; over-allotment flag in P&L; billable vs. non-billable split |
| **Sprint 5** | Revenue status ladder: Scheduled → Earned → Approved → Invoiced → Received → Written Off — all visit revenue classified at all times |
| **Sprint 5** | Monthly investigator specification generator: auto-generate PDF spec per investigator from approved visits |
| **Sprint 5** | Automated spec delivery: email each investigator their monthly specification; finance review gate before send |
| **Sprint 6** | Invoice generation: auto-generate CRO invoice when visit reaches Approved; PDF with procedure-level line items |
| **Sprint 6** | One-time fee invoicing: startup, archiving, pharmacy, administrative billing triggers |
| **Sprint 7** | Payment receipt tracking: mark invoice paid; payment date and amount; cash position updated |
| **Sprint 7** | Cash flow forecast: 13-week rolling + 12-month strategic forecast with base/upside/downside scenarios |
| **Sprint 7** | Patient reimbursement v1: travel claim submission; approval workflow; payment record |

**Milestone M1.2:** First automated investigator specification sent via Meridian — end Sprint 5
**Milestone M1.3:** First invoice auto-generated and tracked to payment receipt — end Sprint 7

---

### Layer 3 — Clinical Operations (Sprints 4–11)

**Goal:** Full study/visit management replacing Power Apps coordinator UI. Coordinators use Meridian web app exclusively by end of Sprint 11. Compressed from 8 sprints to 8 by running UI development in parallel with payment engine work.

| Sprint | Deliverables |
|---|---|
| **Sprint 4** | Next.js app scaffold: auth, navigation, role-based menu, responsive layout |
| **Sprint 4** | Studies list screen: create, view, search studies; CRO assignment; status (Draft/Open/On Hold/Stopped/Locked) |
| **Sprint 5** | Site study setup: full configuration (investigators, visit budgets, one-time budgets, payment terms) |
| **Sprint 5** | Patient enrollment: add patient, assign ID, link to site study, initiate visit schedule |
| **Sprint 6** | Visit logs — patient view: full coordinator dashboard; all visit statuses; visit date management; investigator assignment |
| **Sprint 6** | Auto-scheduling at randomization: all protocol visits auto-populated from randomization date |
| **Sprint 6** | Auto-skip on screen fail: future visits set to Skipped; study arm assignment for multi-arm protocols |
| **Sprint 7** | Coordinator to-do dashboard: cross-study upcoming visits; overdue alerts; one-click status update |
| **Sprint 7** | Visit tolerance warning system: colour-coded Green/Yellow/Red per tolerance window |
| **Sprint 8** | Amendment handling: effective-from date on budget changes; multi-site amendment propagation |
| **Sprint 8** | Study show/stop controls: full lifecycle with auto-generated Stop Report |
| **Sprint 8** | Bulk status operations: multi-visit select and update |
| **Sprint 9** | Study progress summary card: % complete, % budget consumed, visits at risk |
| **Sprint 9** | Normita & time tracking: time norms derived from visit budget; planned vs. actual hours; capacity heatmap |
| **Sprint 10** | RBAC v1: Platform Admin / Site Admin / Coordinator / Investigator / Finance / Read-Only — enforced at API and UI |
| **Sprint 10** | PII audit log: every patient data access and modification logged with user, timestamp, action |
| **Sprint 11** | eSource v1: structured visit checklist; digital consultation report; timestamped + attributed entries; query management |
| **Sprint 11** | Coordinator UAT: 2–3 Solimed coordinators use Meridian for live visits; feedback incorporated |

**Milestone M1.4:** Coordinators conducting live Solimed visits in Meridian web app — end Sprint 11

---

### Phase 1 Close (Sprint 12)

| Sprint | Deliverables |
|---|---|
| **Sprint 12** | Multi-arm backlog fix: backlog calculated only from patient's assigned arm; unresolved arm alert |
| **Sprint 12** | Backlog quality engine v1: stratify all backlog into Committed / Probable / At Risk / Excluded; quality % per study |
| **Sprint 12** | Phase 1 hardening: full regression testing, security audit, performance testing at 10x Solimed volume |
| **Sprint 12** | Power Apps decommission plan: Power Apps set to read-only; coordinators 100% on Meridian |

**Milestone M1.5:** Phase 1 complete — Solimed fully live on Meridian; Layers 1–3 operational — end Sprint 12

---

### Phase 1 Power BI Screens (Sprints 5–12)

| Screen | Sprint | Layer |
|---|---|---|
| Enhanced P&L Dashboard | Sprint 5 | Layer 2 |
| Normita & Time Dashboard | Sprint 5 | Layer 3 |
| Investigator Specification v2 | Sprint 5 | Layer 2 |
| Backlog Quality Dashboard | Sprint 6 | Layer 5 (partial) |
| Rolling 13-Week Cash Flow | Sprint 7 | Layer 2 |
| 12-Month Revenue Forecast | Sprint 7 | Layer 2 |
| Revenue Recognition Dashboard | Sprint 10 | Layer 2 |
| Multi-Arm Backlog Fix Dashboard | Sprint 12 | Layer 5 (partial) |

---

## 7. Phase 2 — Second Site + AI (Months 7–11 / Sprints 13–22)

**Goal:** Two parallel tracks running simultaneously — (1) add Layers 4 and 5 to Solimed, and (2) onboard the second site. By end of Phase 2, Meridian has two live sites, full communication intelligence, and AI models trained on real multi-site data.

**Key change from v1.0:** In the original plan, Communication Hub and AI were built in Phase 1 (Sprints 9–18). In this compressed plan, they are built in Phase 2 alongside the second site onboarding — two tracks, not sequential phases. This requires careful dependency management but saves ~3 months of total calendar time.

### Track A — Layer 4: Communication Hub (Sprints 13–16)

| Sprint | Deliverables |
|---|---|
| **Sprint 13** | Twilio integration: outbound/inbound voice calls from within patient record; call logged against patient |
| **Sprint 13** | Real-time call transcription: live transcript during call; speaker labeling (Coordinator / Patient) |
| **Sprint 14** | Post-call AI summary: key topics, action items, next steps — auto-generated via Claude API; linked to patient and visit |
| **Sprint 14** | SMS / MMS: two-way texting from patient record; TCPA-compliant opt-out; automated visit reminder templates |
| **Sprint 14** | Automated visit reminders: SMS 48 hours before visit; pre-visit instructions; post-visit follow-up |
| **Sprint 15** | Omnichannel inbox: unified view of all calls, SMS, emails per patient — chronological thread |
| **Sprint 15** | Real-time coordinator coaching: AI surfaces protocol reminders during call; AE detection prompts |
| **Sprint 16** | Screening call assistant: AI displays inclusion/exclusion criteria during screening calls; records responses |
| **Sprint 16** | Sentiment analysis per patient: per-call sentiment score; trend over time; deteriorating engagement alert |
| **Sprint 16** | Video visits: native video for Virtual visit types; recorded with consent; linked to visit record |

**Milestone M2.1:** First Solimed coordinator patient call through Meridian with AI transcript and post-call summary — end Sprint 14

### Track B — Second Site Onboarding (Sprints 13–19)

**Target second site criteria:**
- EU-based (GDPR already handled)
- Small-to-mid SMO (5–20 active studies; 50–500 active patients)
- Different CRO mix from Solimed (validates CRO-agnostic approach)
- Identified via Drew Domescik network or Solimed CRO referral by Month 6

| Sprint | Deliverables |
|---|---|
| **Sprint 13** | Second site discovery: data model, current tools, pain points documented; onboarding checklist |
| **Sprint 14** | Tenant provisioning: new tenant created via onboarding wizard; all configuration available |
| **Sprint 15** | Data migration: second site's existing data imported to Meridian; validation complete |
| **Sprint 16** | Localisation v1: i18n framework; locale-aware dates, numbers, currencies; second language if required |
| **Sprint 17** | Multi-currency engine: per-site currency config; FX rate integration (ECB API); historical rate preservation |
| **Sprint 18** | Second country regulatory config: country-specific required fields; data residency in correct Azure region |
| **Sprint 19** | Second site live: processing real studies; coordinators using Meridian in production |

**Milestone M2.2:** Second site live and processing real studies — end Sprint 19

### Track C — Layer 5: AI Intelligence (Sprints 17–21)

| Sprint | Deliverables |
|---|---|
| **Sprint 17** | Enrollment velocity tracker: actual vs. target rate; projected completion date; revenue implication |
| **Sprint 17** | Protocol completion projection: per-patient visit forecast; study-level revenue curve 12 months |
| **Sprint 18** | CRO performance scorecard: payment terms adherence, screen fail rate, margin per CRO |
| **Sprint 18** | Unit economics dashboard: revenue per patient, per visit, per coordinator hour; study type profitability |
| **Sprint 19** | Dropout risk model v1: rule-based scoring using missed visits, overdue visits, sentiment trend |
| **Sprint 20** | Dropout risk model v2: ML model trained on combined Solimed + second site data; per-patient risk score |
| **Sprint 20** | Patient qualification engine v1: run protocol eligibility criteria against patient database; ranked candidate list |
| **Sprint 21** | Predictive payment dashboard: cash flow prediction, CRO payment timing model, investigator payment forecast |
| **Sprint 21** | Coordinator performance intelligence: visit efficiency, protocol adherence rate, patient retention per coordinator |

**Milestone M2.3:** Dropout risk model live and scoring all active patients across both sites — end Sprint 20

### Phase 2 Close (Sprint 22)

| Sprint | Deliverables |
|---|---|
| **Sprint 22** | EHR integration pilot: FHIR R4 read from one EHR system used by second site; patient profile enrichment |
| **Sprint 22** | Penetration test: no critical/high findings |
| **Sprint 22** | GDPR compliance review: both countries pass |
| **Sprint 22** | Phase 2 hardening: multi-tenant security audit; performance testing at 2-site scale |
| **Sprint 22** | Self-serve onboarding wizard v1: complete and tested (prerequisite for Phase 3) |

**Milestone M2.4:** Penetration test passed; GDPR review passed; both sites live — end Sprint 22

---

## 8. Phase 3 — Market Launch (Months 12–18 / Sprints 23–36)

**Goal:** Open Meridian to the market. Self-serve onboarding. Subscription billing. 5+ sites live by Month 18.

| Sprint | Deliverables |
|---|---|
| **Sprint 23–24** | Self-serve onboarding wizard v2: new site completes setup without Pivot involvement; guided flow; config screens |
| **Sprint 25** | Subscription billing: Stripe integration; per-site/per-study tier pricing; automated invoicing; usage metering |
| **Sprint 25** | Customer admin portal: site admins manage their own users, studies, CRO connections, billing |
| **Sprint 26** | Recruitment pipeline v1: internal discovery (query patient database against protocol criteria); referral physician network |
| **Sprint 27** | Patient-facing pre-screening: public-facing questionnaire for study-specific pre-screening; qualified leads into pipeline |
| **Sprint 27** | CRO portal: CRO-facing read-only view of all their studies across all Meridian sites; live visit status; payment specs |
| **Sprint 28** | eConsent module: digital ICF management; re-consent on amendment; 21 CFR Part 11 compliant |
| **Sprint 29** | Mobile app v1 (React Native): coordinator schedule view; visit check-in/out; offline logging with sync |
| **Sprint 30** | Mobile app v2: investigator app; push notifications; e-signature on mobile |
| **Sprint 31** | Recruitment engine v2: digital intake forms; campaign lead capture; pipeline conversion; diversity monitoring |
| **Sprint 32** | External registry integration: connect to 1–2 disease registries; opt-in patient discovery |
| **Sprint 33** | AI model marketplace: sites enable/disable AI features per study; model performance dashboards |
| **Sprint 34** | Protocol deviation AI: real-time detection of deviations; classification (minor vs. major); CRO notification workflow |
| **Sprint 35** | Document management: version-controlled doc store per study; expiry tracking; e-signature via DocuSign API |
| **Sprint 35** | Sponsor-facing dashboard: sponsor view of their study performance across all Meridian sites (read-only) |
| **Sprint 36** | Platform hardening: SOC 2 Type II readiness audit; HIPAA BAA documentation; performance at 50-site scale |
| **Sprint 36** | Market launch: public website live; 5+ sites onboarded; press / industry announcement |

**Milestone M3.1:** First self-serve site onboarded without Pivot involvement — end Sprint 24
**Milestone M3.2:** 5 sites live on Meridian — end Sprint 31
**Milestone M3.3:** Mobile app in App Store and Google Play — end Sprint 30
**Milestone M3.4:** SOC 2 Type II audit initiated — end Sprint 36
**Milestone M3.5:** Public market launch — end Sprint 36

---

## 9. Compressed Milestone Map

| Month | Milestone | Phase |
|---|---|---|
| **Month 1** | All Solimed data live in Meridian PostgreSQL | Phase 1 |
| **Month 2.5** | First automated investigator spec sent via Meridian | Phase 1 |
| **Month 3.5** | First CRO invoice auto-generated and tracked | Phase 1 |
| **Month 5.5** | Coordinators live on Meridian for all visits | Phase 1 |
| **Month 6** | Phase 1 complete — Layers 1–3 live at Solimed | Phase 1 |
| **Month 7** | Communication Hub: first Solimed patient call through Meridian | Phase 2 |
| **Month 9.5** | Second site live and processing real studies | Phase 2 |
| **Month 10** | Dropout risk model live across both sites | Phase 2 |
| **Month 11** | Penetration test passed; Phase 2 complete | Phase 2 |
| **Month 12** | First self-serve site onboarded | Phase 3 |
| **Month 15.5** | 5 sites live on Meridian | Phase 3 |
| **Month 17** | Mobile app in App Store and Google Play | Phase 3 |
| **Month 18** | Public market launch; SOC 2 initiated | Phase 3 |

---

## 10. Team Structure

### Core Product Team

| Role | Phase 1 FTE | Phase 2 FTE | Phase 3 FTE | Responsibilities |
|---|---|---|---|---|
| **Product Manager** | 1.0 | 1.0 | 1.0 | Product vision, roadmap, backlog, stakeholder alignment |
| **Technical Lead / Architect** | 1.0 | 1.0 | 1.0 | Architecture decisions, code quality, ADRs, security |
| **Full-Stack Developer** | **2.0** | 2.0 | 3.0 | API, frontend, database, integrations — 2 from Sprint 1 is key compression lever |
| **Data / BI Engineer** | 0.5 | 1.0 | 1.0 | Power BI, data pipeline, Layer 1 ingestion adapters |
| **ML Engineer** | 0.5 | 1.0 | 1.0 | Layer 5 models: qualification, dropout risk, revenue prediction |
| **Mobile Developer** | 0 | 0.5 | 1.0 | React Native iOS/Android app (Phase 3) |
| **QA Engineer** | 0.5 | 1.0 | 1.0 | Test strategy, automation, regression, security testing |
| **UX Designer** | 0.5 | 0.5 | 0.5 | Coordinator / investigator workflows, usability testing |
| **DevOps / Infrastructure** | 0.25 | 0.5 | 0.5 | Azure, CI/CD, monitoring, compliance infrastructure |

**Total FTE Phase 1:** ~6.25 | **Phase 2:** ~8.5 | **Phase 3:** ~10

### Solimed Team (Phase 1 Only)

| Name | Role | Availability |
|---|---|---|
| Mladen Geng | Domain Expert / Knowledge Transfer | 2 days/week Sprint 1; then ad-hoc |
| Ivan Kruljac | Product Sponsor | Sprint reviews + strategic decisions |
| Drew Domescik | CFO Advisor / Financial Features Sponsor | Sprint reviews + financial UAT |
| 2–3 Coordinators | End User Testers | Sprint 11 UAT; periodic usability sessions |
| Finance Role (TBD) | Billing Workflow SME | Sprint 5–7 billing UAT |

---

## 11. Risk Register

| # | Risk | Prob | Impact | Mitigation |
|---|---|---|---|---|
| R1 | Mladen unavailable — knowledge transfer incomplete | Med | Critical | Sprint 1 is 100% knowledge transfer; record all sessions; document before building |
| R2 | Solimed data quality worse than expected | Med | High | Data audit Phase 0; clean before migrating; do not migrate garbage |
| R3 | Power Apps limitations block Layer 3 UI parity | Med | High | Power Apps stays live in parallel until Meridian UI passes coordinator UAT (Sprint 11) |
| R4 | Twilio HIPAA-eligible configuration complexity | Low | High | Engage Twilio enterprise in Sprint 13; validate BAA before any patient calls go through |
| R5 | EHR FHIR integration access denied by hospital IT | High | Med | FHIR stub in Sprint 3; EHR integration is Phase 2 Sprint 22 — not on Phase 1 critical path |
| R6 | Claude API cost at scale | Med | Med | Token monitoring from Sprint 14; per-call token budget; evaluate summary caching |
| R7 | ML model accuracy insufficient for clinical trust | Med | High | Models start rule-based (Sprint 19); ML layer (Sprint 20) is additive — rules are always fallback |
| R8 | Second site not identified by Month 6 | Med | High | Begin second site outreach at Month 4; Drew's network; Solimed CRO introductions |
| R9 | Regulatory requirements in second country unknown | High | High | Regulatory scan starts Phase 1 Sprint 11; legal counsel engaged before Phase 2 |
| R10 | Key Pivot team member departure | Low | High | Pair programming; all code reviewed; ADRs documented; no single-person knowledge silos |
| R11 | Scope creep from Solimed (treating Meridian as custom dev) | High | Med | Clear distinction: Solimed is a platform customer, not a custom dev client; formal change control |
| R12 | Competing platform (Crio, Florence) copies AI features | Med | Med | Speed of execution; network effects; Solimed reference customer with testimonial |
| R13 | Patient data privacy breach | Low | Critical | Security audit Sprint 12; pen test before Phase 3; HIPAA BAA all services; PII audit log Sprint 10 |
| R14 | Two Phase 2 tracks (Communication + Second Site) create coordination overhead | Med | Med | Dedicated sprint lead per track; shared standups; integration points clearly scheduled |

---

## 12. Technology Decision Log

All architecture decisions recorded as ADRs in the GitHub repository at `docs/adr/`. Decisions made in Phase 0 Week 2 and frozen for Phase 1.

### Pre-decided (from platform vision)

| Component | Decision | Rationale |
|---|---|---|
| Frontend | Next.js (React) | SSR performance; React Native code sharing |
| API | Node.js (Fastify) or Python (FastAPI) | **To be decided Week 2** |
| Database — primary | PostgreSQL on Azure | Relational; ACID; HIPAA-capable |
| LLM | Claude API (Anthropic) | Call summaries, protocol extraction, AI coaching |
| Communications | Twilio | Programmable; HIPAA-eligible voice + SMS |
| Video | Daily.co | HIPAA-compliant video; embed in web app |
| Reporting | Power BI Embedded | Solimed compatibility; best-in-class BI |
| Auth | Auth0 or Azure AD B2C | **To be decided Week 2** |
| Infrastructure | Azure | Microsoft ecosystem; HIPAA BAA; global regions |
| CI/CD | GitHub Actions | Free tier sufficient; in-repo workflows |
| Search / eligibility matching | Elasticsearch | Full-text + structured query; protocol criteria matching |
| Payments (platform billing) | Stripe | Industry standard; subscription metering |
| E-signature | DocuSign API | eConsent and document signing |

---

## 13. Quality Gates

Every phase has a quality gate that must pass before the next phase begins:

### Phase 1 → Phase 2 Gate
- [ ] Layers 1–3 operational for Solimed in production
- [ ] Zero critical or high security findings from Phase 1 audit
- [ ] Coordinator NPS ≥ 40 (3+ coordinators surveyed)
- [ ] Automated investigator spec sending running without errors for 2+ consecutive months
- [ ] All Solimed data migrated; Power Apps in read-only mode
- [ ] Ivan Kruljac written sign-off on Phase 1 completion
- [ ] Second site candidate identified and in discovery

### Phase 2 → Phase 3 Gate
- [ ] Second site live and processing real studies
- [ ] Layer 4 Communication Hub live at Solimed with 50+ patient calls processed
- [ ] Penetration test passed (no critical/high findings)
- [ ] GDPR compliance review passed for both countries
- [ ] Dropout model live with >60% precision on 30-day flag
- [ ] Self-serve onboarding wizard tested end-to-end

### Phase 3 Launch Gate
- [ ] 5 sites live on platform
- [ ] SOC 2 Type II audit initiated
- [ ] Mobile app in App Store and Google Play
- [ ] No P1 incident in prior 30 days
- [ ] All customer SLAs met for 60 consecutive days

---

## 14. Communication & Governance

### Cadences

| Meeting | Frequency | Participants | Purpose |
|---|---|---|---|
| Sprint Planning | Bi-weekly (Monday) | Full Pivot team | Sprint backlog commitment |
| Daily Standup | Daily | Pivot team | Blockers, progress, dependencies |
| Sprint Review | Bi-weekly (Friday) | Pivot + Ivan + Drew | Demo working software; get feedback |
| Product Steering | Monthly | Pivot PM + Ivan + Drew | Roadmap, priorities, budget |
| Architecture Review | Monthly | Pivot Tech Lead + Pivot PM | ADR review; technical debt; scaling decisions |
| Retrospective | Bi-weekly | Pivot team only | Process improvement |

### Reporting

| Report | Frequency | Audience | Contents |
|---|---|---|---|
| Sprint Summary | Bi-weekly | Ivan, Drew | Completed features, velocity, next sprint plan, risks |
| Product Metrics Dashboard | Monthly | Ivan, Drew, Pivot leadership | Active users, visits processed, AI model performance, ARR pipeline |
| Financial Tracker | Monthly | Ivan | Pivot hours consumed, budget remaining, Phase forecast |
| Risk Register Update | Monthly | Ivan | New risks, mitigated risks, open items |

---

## 15. Success Metrics

### Phase 1 Metrics (Solimed as design partner)

| Metric | Target | Measurement |
|---|---|---|
| Coordinator adoption | 100% of Solimed coordinators using Meridian daily | Login tracking |
| Visit logging accuracy | Same or better than Power Apps (zero regression) | Comparison audit Sprint 11 |
| Investigator spec automation | Monthly spec emails sent without manual intervention | Email send logs |
| Backlog accuracy | Multi-arm inflation resolved; ≤5% variance from manual | Finance reconciliation |
| Coordinator NPS | ≥40 | Post-Phase-1 survey |
| Calendar compression | Phase 1 complete by Month 6 | Sprint velocity tracking |

### Phase 2 Metrics

| Metric | Target |
|---|---|
| Second site onboarding time | <2 weeks from data import to first live visit |
| AI call summary quality | Ivan + coordinator rating ≥4/5 on 20+ summaries |
| Patient dropout prediction accuracy | >60% precision on 30-day dropout flag |
| Patient qualification engine | >70% of top-10 candidates pass formal screening |

### Phase 3 Metrics (Market)

| Metric | Target |
|---|---|
| Sites live | 5+ by Month 18 |
| ARR | €150K+ by Month 18 (ramp toward €600K Year 3 target) |
| Self-serve onboarding | New site live in <48 hours without Pivot involvement |
| Churn | <10% annual site churn |
| NPS (all customers) | ≥50 |

---

*This plan is Version 2.0 — compressed from the original 24-month plan (v1.0) to 18 months. The compression was achieved by: (1) running Phase 0 in parallel with Sprint 1, (2) scoping Phase 1 to Layers 1–3 MVP only, (3) adding a second developer from Sprint 1, and (4) running Layer 4/5 development in parallel with second-site onboarding in Phase 2. This plan will be updated at the start of each phase based on actual velocity and learnings from the prior phase.*
