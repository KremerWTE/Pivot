# Meridian — Product Development PM Plan
## Unified Clinical Trial Intelligence Platform

**Product:** Meridian (by Pivot)
**Document Type:** Product PM Plan
**Prepared by:** Pivot
**Date:** 2026-04-02
**Version:** 1.0
**Horizon:** 24 months (Months 1–24)

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

## 2. Development Philosophy

**Build with, not for.** Solimed is not just a client — they are the development partner. Every feature built in Phase 1 is validated against real clinical trial operations before we declare it market-ready.

**Layer-first, not feature-first.** Each layer must be stable before the layer above it is built. You cannot build AI predictions (Layer 5) without reliable data (Layer 1) and a complete operational record (Layers 2–3).

**Ship working software every sprint.** No sprint ends without a working, demonstrable increment. Coordinators and investigators use what we build in production — not in a demo environment.

**Solimed's constraints are the platform's constraints.** If something is too complex for Solimed's coordinators, it's too complex for any site. Build for the least-technical user on the most demanding protocol.

---

## 3. Phases Overview

| Phase | Name | Duration | Primary Deliverable |
|---|---|---|---|
| **Phase 0** | Foundation | Weeks 1–4 | Team, tooling, access, architecture decisions |
| **Phase 1** | Solimed Build | Months 1–9 | Layers 1–5 built for Solimed; platform validated in production |
| **Phase 2** | Second Site | Months 10–15 | Multi-tenancy live; second SMO onboarded; AI trained on real data |
| **Phase 3** | Market Launch | Months 16–24 | Self-serve onboarding; subscription billing; 5+ sites live |

---

## 4. Phase 0 — Foundation (Weeks 1–4)

**Goal:** Set the team up to build. No features ship in Phase 0 — only infrastructure, access, and decisions.

### Week 1: Access & Discovery

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

### Week 2: Architecture Decision Records (ADRs)

The following decisions must be made and documented before a line of code is written:

| Decision | Options | Deadline |
|---|---|---|
| API language: Node.js (Fastify) vs. Python (FastAPI) | Performance vs. ML team preference | Week 2 |
| Database: PostgreSQL vs. Azure SQL vs. split | Cost, HIPAA, team familiarity | Week 2 |
| Patient profile store: MongoDB vs. PostgreSQL JSON | Flexibility vs. consistency | Week 2 |
| Auth provider: Auth0 vs. Azure AD B2C | Cost at scale, Microsoft ecosystem | Week 2 |
| Communications: Twilio vs. Azure Communication Services | HIPAA BAA, feature set, cost | Week 2 |
| FHIR layer: Azure Health Data Services vs. build own | Cost vs. control | Week 2 |
| Frontend: Next.js only vs. Next.js + React Native | Mobile priority in Phase 1 | Week 2 |
| BI reporting: Power BI Embedded vs. build custom | Solimed compatibility, cost | Week 2 |

### Week 3: Data Model & Knowledge Transfer

| Task | Owner | Done When |
|---|---|---|
| Full Solimed data model documented (all Dataverse tables, relationships, field types) | Pivot + Mladen | ERD diagram approved |
| All Power Apps business logic documented (fee calculations, visit status rules, auto-scheduling logic) | Pivot + Mladen | Logic document reviewed by Ivan |
| All Power BI DAX measures documented | Pivot BI Engineer + Mladen | All 30+ measures documented |
| Known workarounds and edge cases documented | Mladen | List reviewed by Ivan |
| Test data vs. production data identified and flagged | Mladen | Clean data baseline established |
| Data quality audit: null rates, duplicates, value distributions | Pivot + Mladen | Audit report complete |
| Power Apps staging environment created | Mladen + Pivot | Staging env live and accessible |

### Week 4: Environment Setup & Sprint 1 Planning

| Task | Owner | Done When |
|---|---|---|
| Azure Dev + Staging + Prod environments provisioned | Pivot DevOps | All 3 environments live |
| CI/CD pipeline: GitHub Actions → Staging | Pivot Tech Lead | Auto-deploy on merge to `main` |
| Database schemas created (Dev) | Pivot Tech Lead | Migration scripts run cleanly |
| API skeleton running (health check endpoint live) | Pivot Tech Lead | `/health` returns 200 |
| Auth0 / Azure AD B2C tenant configured | Pivot Tech Lead | Test user can log in |
| Sprint 1 backlog groomed and estimated | Pivot PM + team | All Sprint 1 stories estimated and ready |
| Kickoff meeting with Ivan + Drew | Pivot PM | Demo of Phase 0 deliverables; Sprint 1 plan reviewed |

---

## 5. Phase 1 — Solimed Build (Months 1–9 / Sprints 1–18)

**Goal:** Build a complete, production-grade version of Meridian using Solimed as the design partner. By the end of Phase 1, Solimed is fully off Power Apps and running on Meridian. All 5 layers are functional for Solimed's specific use case.

### Sprint Cadence
- **2-week sprints**
- Sprint planning: Monday Week 1
- Mid-sprint check-in: Wednesday Week 1
- Sprint review / demo to Ivan + Drew: Friday Week 2
- Retrospective: Internal Pivot only, Friday Week 2

---

### Layer 1 — Data Ingestion Hub (Sprints 1–4)

**Goal:** Establish the authoritative data store. Migrate Solimed from Power Apps/Dataverse to Meridian's PostgreSQL backend.

| Sprint | Deliverables |
|---|---|
| **Sprint 1** | Database schema v1: Studies, Site Studies, Patients, Visits, Investigators, Budgets — modelled from Solimed data model ERD |
| **Sprint 1** | Data migration script: full Dataverse export → PostgreSQL import with validation |
| **Sprint 2** | Migration dry-run on staging: all Solimed data migrated; record counts match; no data loss |
| **Sprint 2** | EDC sync adapter v1: read-only pull of visit status from one CRO's EDC system (pilot CRO selected with Ivan) |
| **Sprint 3** | Multi-tenant schema: `tenant_id` + `country_id` + `site_id` on all tables; row-level security enforced |
| **Sprint 3** | Excel time tracking migration: import historical time tracking data from Excel into PostgreSQL `time_entries` table |
| **Sprint 4** | FHIR adapter stub: Azure Health Data Services provisioned; basic FHIR R4 patient resource read (for Phase 2 EHR integration) |
| **Sprint 4** | Data ingestion API: REST endpoints for external data push; webhook receiver for EDC event notifications |

**Milestone M1.1:** All Solimed data live in Meridian PostgreSQL; Power Apps reads from Meridian API (not Dataverse) — end Sprint 4

---

### Layer 2 — Payment Engine (Sprints 3–8)

**Goal:** Full investigator fee management, CRO invoicing, patient reimbursements, and cash flow forecasting — replacing Solimed's current manual billing process entirely.

| Sprint | Deliverables |
|---|---|
| **Sprint 3** | Investigator fee engine v1: PI fee, PI cut %, sub-investigator splits, referral doctor fee — all calculated from API (not Power Apps formulas) |
| **Sprint 4** | Working hours flag: in-hours vs. after-hours toggle on visit record; fee engine applies correct rate per flag |
| **Sprint 5** | Monthly investigator specification generator: auto-generate PDF spec per investigator from approved visits for selected month |
| **Sprint 5** | Automated spec delivery: email each investigator their monthly specification on configured schedule; finance review gate (approve before send / auto-send) |
| **Sprint 6** | Screen fail allotment engine: contractual allotment per study; running counter; over-allotment flag in P&L; billable vs. non-billable split |
| **Sprint 6** | Revenue status ladder: Scheduled → Earned → Approved → Invoiced → Received → Written Off — all visit revenue classified at all times |
| **Sprint 7** | Invoice generation: auto-generate CRO invoice when visit reaches Approved; PDF with procedure-level line items; invoice record stored in system |
| **Sprint 7** | One-time fee invoicing: startup, archiving, pharmacy, administrative — billing triggers and invoice generation |
| **Sprint 8** | Payment receipt tracking: mark invoice paid; payment date and amount recorded; cash position updated |
| **Sprint 8** | Cash flow forecast: 13-week rolling forecast from scheduled visits + payment terms; 12-month strategic forecast with base/upside/downside scenarios |
| **Sprint 8** | Patient reimbursement v1: travel claim submission (coordinator-entered); approval workflow; payment record |

**Milestone M1.2:** First automated investigator specification sent to Solimed investigators via Meridian — end Sprint 5
**Milestone M1.3:** First invoice auto-generated and tracked through to payment receipt — end Sprint 8

---

### Layer 3 — Clinical Operations (Sprints 5–12)

**Goal:** Full study/visit management replacing Power Apps coordinator UI. Coordinators use Meridian web app exclusively by end of Sprint 12.

| Sprint | Deliverables |
|---|---|
| **Sprint 5** | Next.js app scaffold: auth, navigation, role-based menu, responsive layout |
| **Sprint 5** | Studies list screen: create, view, search studies; CRO assignment; status (Draft/Open/On Hold/Stopped/Locked) |
| **Sprint 6** | Site study setup: full configuration (investigators, visit budgets, one-time budgets, payment terms); site-level default budgets |
| **Sprint 6** | Patient enrollment: add patient, assign ID, link to site study, initiate visit schedule |
| **Sprint 7** | Visit logs — patient view: full coordinator dashboard; all visit statuses; visit date management; investigator assignment |
| **Sprint 7** | Auto-scheduling at randomization: all protocol visits auto-populated from randomization date |
| **Sprint 7** | Auto-skip on screen fail: future visits set to Skipped; study arm assignment for multi-arm protocols |
| **Sprint 8** | Coordinator to-do dashboard: cross-study upcoming visits; overdue alerts; one-click status update |
| **Sprint 8** | Visit tolerance warning system: colour-coded Green/Yellow/Red per tolerance window |
| **Sprint 9** | Amendment handling: effective-from date on budget changes; multi-site amendment propagation; coordinator notification |
| **Sprint 9** | Study show/stop controls: full lifecycle with auto-generated Stop Report |
| **Sprint 9** | Bulk status operations: multi-visit select and update |
| **Sprint 10** | Study progress summary card: % complete, % budget consumed, visits at risk |
| **Sprint 10** | Normita & time tracking: time norms derived from visit budget; planned vs. actual hours; capacity heatmap |
| **Sprint 11** | RBAC v1: Platform Admin / Site Admin / Coordinator / Investigator / Finance / Read-Only — enforced at API and UI |
| **Sprint 11** | PII audit log: every patient data access and modification logged with user, timestamp, action |
| **Sprint 12** | eSource v1: structured visit checklist; digital consultation report; timestamped + attributed entries; query management |
| **Sprint 12** | Coordinator UAT sprint: 2–3 Solimed coordinators use Meridian for live visits; feedback incorporated |

**Milestone M1.4:** Coordinators conducting live Solimed visits in Meridian web app — end Sprint 12

---

### Layer 4 — Communication Hub (Sprints 9–14)

**Goal:** All patient communication — voice, SMS, video — flows through Meridian with AI transcription and clinical context.

| Sprint | Deliverables |
|---|---|
| **Sprint 9** | Twilio integration: outbound/inbound voice calls from within patient record; call logged against patient |
| **Sprint 10** | Real-time call transcription: live transcript during call; speaker labeling (Coordinator / Patient) |
| **Sprint 10** | Post-call AI summary: key topics, action items, next steps — auto-generated via Claude API; linked to patient record and upcoming visit |
| **Sprint 11** | SMS / MMS: two-way texting from patient record; TCPA-compliant opt-out; automated visit reminder templates |
| **Sprint 11** | Automated visit reminders: SMS sent 48 hours before visit; pre-visit instructions; post-visit follow-up |
| **Sprint 12** | Omnichannel inbox: unified view of all calls, SMS, emails per patient — chronological thread |
| **Sprint 13** | Real-time coordinator coaching: AI surfaces protocol reminders during call ("Visit W144 due in 5 days — confirm appointment"); AE detection prompts |
| **Sprint 13** | Screening call assistant: during screening calls, AI displays inclusion/exclusion criteria; prompts coordinator to ask each question; records responses |
| **Sprint 14** | Sentiment analysis per patient: per-call sentiment score; sentiment trend over time; deteriorating engagement alert |
| **Sprint 14** | Video visits: native video for Virtual visit types; recorded with consent; linked to visit record |
| **Sprint 14** | CRO / sponsor communication channel: per-study channel for monitor communications; meeting intelligence (AI summary + action items) |

**Milestone M1.5:** First Solimed coordinator patient call conducted through Meridian with AI transcript and post-call summary — end Sprint 10

---

### Layer 5 — AI Intelligence (Sprints 11–18)

**Goal:** Predictive capabilities built on top of the operational data and communication data accumulated in Layers 1–4.

| Sprint | Deliverables |
|---|---|
| **Sprint 11** | Backlog quality engine: stratify all backlog into Committed / Probable / At Risk / Excluded; quality % per study |
| **Sprint 12** | Multi-arm backlog fix: backlog calculated only from patient's assigned arm; unresolved arm alert |
| **Sprint 13** | Enrollment velocity tracker: actual vs. target enrollment rate; projected completion date; revenue implication of off-pace enrollment |
| **Sprint 13** | Protocol completion projection: per-patient visit forecast; study-level revenue curve for next 12 months |
| **Sprint 14** | Revenue recognition dashboard: billable vs. at-risk vs. written-off split; screen fail revenue recognition |
| **Sprint 14** | CRO performance scorecard: payment terms adherence, screen fail rate, margin per CRO |
| **Sprint 15** | Dropout risk model v1: rule-based scoring using missed visits, overdue visits, sentiment trend, social determinants |
| **Sprint 16** | Dropout risk model v2: ML model trained on Solimed's historical visit + communication data; per-patient risk score with contributing factors |
| **Sprint 16** | Patient qualification engine v1: run protocol eligibility criteria against Solimed's patient database; ranked candidate list |
| **Sprint 17** | Unit economics dashboard: revenue per patient, per visit, per coordinator hour; study type profitability |
| **Sprint 17** | Coordinator performance intelligence: visit efficiency, protocol adherence rate, patient retention rate per coordinator |
| **Sprint 18** | Predictive payment dashboard: cash flow prediction, CRO payment timing model, investigator payment forecast |
| **Sprint 18** | Phase 1 hardening: full regression testing, security audit, performance testing at 10x Solimed volume |

**Milestone M1.6:** Dropout risk model live and scoring all 321 active Solimed patients — end Sprint 16
**Milestone M1.7:** Phase 1 complete — Solimed fully live on Meridian; all 5 layers operational — end Sprint 18

---

### Phase 1 Power BI Screens (Delivered Across Sprints 6–18)

| Screen | Sprint | Layer |
|---|---|---|
| Enhanced P&L Dashboard | Sprint 6 | Layer 2 |
| Backlog Quality Dashboard v2 | Sprint 6 | Layer 5 |
| Normita & Time Dashboard | Sprint 6 | Layer 3 |
| Investigator Specification v2 | Sprint 6 | Layer 2 |
| Rolling 13-Week Cash Flow | Sprint 8 | Layer 2 |
| 12-Month Revenue Forecast | Sprint 8 | Layer 2 |
| Enrollment Velocity Tracker | Sprint 13 | Layer 5 |
| Protocol Completion Projection | Sprint 13 | Layer 5 |
| Revenue Recognition Dashboard | Sprint 14 | Layer 2 |
| CRO Performance Scorecard | Sprint 14 | Layer 5 |
| Unit Economics Dashboard | Sprint 17 | Layer 5 |
| Coordinator Performance | Sprint 17 | Layer 5 |

---

## 6. Phase 2 — Second Site (Months 10–15 / Sprints 19–30)

**Goal:** Prove multi-tenancy by onboarding a second SMO. AI models trained on Solimed data are applied to new site data. Validate the platform is a product, not a custom build.

### Target Second Site Criteria
- EU-based (GDPR already handled by platform)
- Small-to-mid SMO (5–20 active studies; 50–500 active patients)
- Different CRO mix from Solimed (validates CRO-agnostic approach)
- Ideally Drew Domescik network introduction or Solimed CRO referral

### Sprint Plan

| Sprint | Deliverables |
|---|---|
| **Sprint 19** | Second site discovery: data model, current tools, pain points documented; onboarding checklist completed |
| **Sprint 20** | Tenant provisioning: new tenant created in 48 hours via onboarding wizard; all configuration options available |
| **Sprint 21** | Data migration: second site's existing data imported to Meridian; validation complete |
| **Sprint 22** | Localisation v1: i18n framework; locale-aware dates, numbers, currencies; second language added if required |
| **Sprint 23** | Multi-currency engine: per-site currency config; FX rate integration (ECB API); historical rate preservation |
| **Sprint 24** | Second country regulatory config: country-specific required fields; data residency in correct Azure region |
| **Sprint 25** | Patient qualification engine v2: trained on combined Solimed + second site data; cross-tenant model (anonymised) |
| **Sprint 26** | EHR integration pilot: FHIR R4 read from one EHR system used by second site (Epic or Cerner); patient profile enrichment |
| **Sprint 27** | Recruitment pipeline v1: internal discovery (query patient database against protocol criteria); referral physician network management |
| **Sprint 28** | Patient-facing pre-screening: public-facing questionnaire for study-specific pre-screening; qualified leads enter recruitment pipeline |
| **Sprint 29** | eConsent module: digital ICF management; re-consent automation on amendment; 21 CFR Part 11 compliant |
| **Sprint 30** | Phase 2 hardening: multi-tenant security audit; penetration testing; GDPR compliance review; Phase 2 sign-off |

**Milestone M2.1:** Second site tenant live and processing real studies — end Sprint 22
**Milestone M2.2:** First EHR-sourced patient identified as a qualified study candidate — end Sprint 26
**Milestone M2.3:** Security / penetration test passed — end Sprint 30

---

## 7. Phase 3 — Market Launch (Months 16–24 / Sprints 31–48)

**Goal:** Open Meridian to the market. Self-serve onboarding. Subscription billing. Target 5+ sites live by Month 24.

### Sprint Plan

| Sprint | Deliverables |
|---|---|
| **Sprint 31–32** | Self-serve onboarding wizard: new site completes setup without Pivot involvement; all config screens; guided flow |
| **Sprint 33** | Subscription billing: Stripe integration; per-site/per-study tier pricing; automated invoicing to customers; usage metering |
| **Sprint 34** | Customer admin portal: site admins manage their own users, studies, CRO connections, billing |
| **Sprint 35–36** | CRO portal: CRO-facing read-only view of all their studies across all Meridian sites; live visit status; payment specs |
| **Sprint 37** | Recruitment engine v2: digital intake forms (public landing pages); campaign lead capture; pipeline conversion tracking; diversity monitoring |
| **Sprint 38** | External registry integration: connect to 1–2 disease registries; opt-in patient discovery from registry data |
| **Sprint 39** | Regulatory intelligence v1: country regulatory change monitoring; alert when a change affects active studies on the platform |
| **Sprint 40** | Mobile app v1 (React Native): coordinator schedule view; visit check-in/out; offline visit logging with sync |
| **Sprint 41** | Mobile app v2: investigator app; push notifications; e-signature on mobile |
| **Sprint 42** | AI model marketplace: sites can enable/disable AI features per study; model performance dashboards |
| **Sprint 43–44** | Protocol deviation AI: real-time detection of deviations from protocol schedule; deviation classification (minor vs. major); CRO notification workflow |
| **Sprint 45** | Document management: version-controlled doc store per study; expiry tracking; e-signature via DocuSign API |
| **Sprint 46** | Sponsor-facing dashboard: sponsor view of their study performance across all Meridian sites (read-only; separate from CRO portal) |
| **Sprint 47** | Platform hardening: SOC 2 Type II readiness audit; HIPAA BAA documentation; performance testing at 50-site scale |
| **Sprint 48** | Market launch: public website live; 5+ sites onboarded; press / industry announcement |

**Milestone M3.1:** First self-serve site onboarded without Pivot involvement — end Sprint 32
**Milestone M3.2:** 5 sites live on Meridian — end Sprint 40
**Milestone M3.3:** Mobile app in App Store and Google Play — end Sprint 41
**Milestone M3.4:** SOC 2 Type II audit initiated — end Sprint 47
**Milestone M3.5:** Public market launch — end Sprint 48

---

## 8. Team Structure

### Core Product Team

| Role | Phase 1 FTE | Phase 2 FTE | Phase 3 FTE | Responsibilities |
|---|---|---|---|---|
| **Product Manager** | 1.0 | 1.0 | 1.0 | Product vision, roadmap, backlog, stakeholder alignment |
| **Technical Lead / Architect** | 1.0 | 1.0 | 1.0 | Architecture decisions, code quality, ADRs, security |
| **Full-Stack Developer** | 2.0 | 2.0 | 3.0 | API, frontend, database, integrations |
| **Data / BI Engineer** | 0.5 | 1.0 | 1.0 | Power BI, data pipeline, Layer 1 ingestion adapters |
| **ML Engineer** | 0.5 | 1.0 | 1.0 | Layer 5 models: qualification, dropout risk, revenue prediction |
| **Mobile Developer** | 0 | 0.5 | 1.0 | React Native iOS/Android app (Phase 3) |
| **QA Engineer** | 0.5 | 1.0 | 1.0 | Test strategy, automation, regression, security testing |
| **UX Designer** | 0.5 | 0.5 | 0.5 | Coordinator / investigator workflows, usability testing |
| **DevOps / Infrastructure** | 0.25 | 0.5 | 0.5 | Azure, CI/CD, monitoring, compliance infrastructure |

### Solimed Team (Phase 1 Only)

| Name | Role | Availability |
|---|---|---|
| Mladen Geng | Domain Expert / Knowledge Transfer | 2 days/week Sprints 1–2; then ad-hoc |
| Ivan Kruljac | Product Sponsor | Sprint reviews + strategic decisions |
| Drew Domescik | CFO Advisor / Financial Features Sponsor | Sprint reviews + financial UAT |
| 2–3 Coordinators | End User Testers | Sprint 12 UAT; periodic usability sessions |
| Finance Role (TBD) | Billing Workflow SME | Sprint 5–8 billing UAT |

---

## 9. Risk Register

| # | Risk | Prob | Impact | Mitigation |
|---|---|---|---|---|
| R1 | Mladen unavailable — knowledge transfer incomplete | Med | Critical | Sprint 1 is 100% knowledge transfer; record all sessions; document everything before building |
| R2 | Solimed data quality worse than expected | Med | High | Data audit in Phase 0; clean before migrating; do not migrate garbage |
| R3 | Power Apps limitations block Layer 3 UI parity | Med | High | Power Apps UI stays live in parallel until Meridian UI is UAT-approved by coordinators |
| R4 | Twilio HIPAA-eligible configuration complexity | Low | High | Engage Twilio enterprise team in Sprint 9; validate BAA before any patient calls go through |
| R5 | EHR FHIR integration access denied by hospital IT | High | Med | FHIR stub in Layer 1 Sprint 4; EHR integration is Phase 2 — not on critical path for Phase 1 |
| R6 | Claude API (call summaries) cost at scale | Med | Med | Token usage monitoring from Sprint 10; set per-call token budget; evaluate caching summaries |
| R7 | ML model accuracy insufficient for clinical trust | Med | High | Models start rule-based (Sprint 15); ML layer (Sprint 16) is additive — rules are always the fallback |
| R8 | Second site not identified by Month 10 | Med | High | Begin second site outreach at Month 6; Drew's network; Solimed CRO introductions |
| R9 | Regulatory requirements in second country unknown | High | High | Regulatory scan begins Phase 1 Sprint 14; legal counsel engaged before Phase 2 |
| R10 | Key Pivot team member departure | Low | High | Pair programming standard; all code reviewed; ADRs documented; no single-person knowledge silos |
| R11 | Scope creep from Solimed (treating Meridian as custom dev) | High | Med | Clear distinction: Solimed is a platform customer, not a custom dev client; change requests evaluated against platform fit |
| R12 | Competing platform (Crio, Florence) copies AI features | Med | Med | Speed of execution; network effects (more sites = better AI models); Solimed reference customer |
| R13 | Data privacy breach (patient data) | Low | Critical | Security audit Phase 1 Sprint 18; pen test before Phase 2; HIPAA BAA all services; PII audit log from Sprint 11 |

---

## 10. Technology Decision Log

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

## 11. Quality Gates

Every phase has a quality gate that must pass before the next phase begins:

### Phase 1 → Phase 2 Gate
- [ ] All 5 layers operational for Solimed in production
- [ ] Zero critical or high security findings from Phase 1 audit
- [ ] Coordinator NPS ≥ 40 (3+ coordinators surveyed)
- [ ] Automated investigator spec sending running without errors for 2+ consecutive months
- [ ] All Solimed data migrated; Power Apps decommissioned or in read-only mode
- [ ] Ivan Kruljac written sign-off on Phase 1 completion

### Phase 2 → Phase 3 Gate
- [ ] Second site live and processing real studies
- [ ] Penetration test passed (no critical/high findings)
- [ ] GDPR compliance review passed for both countries
- [ ] Patient qualification engine scoring candidates at second site
- [ ] Solimed site-specific dropouts predicted with >65% accuracy (30-day look-ahead)
- [ ] Self-serve onboarding wizard complete and tested

### Phase 3 Launch Gate
- [ ] 5 sites live on platform
- [ ] SOC 2 Type II audit initiated
- [ ] Mobile app in App Store and Google Play
- [ ] No P1 incident in prior 30 days
- [ ] All customer SLAs met for 60 consecutive days

---

## 12. Communication & Governance

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

## 13. Success Metrics

### Phase 1 Metrics (Solimed as design partner)

| Metric | Target | Measurement |
|---|---|---|
| Coordinator adoption | 100% of Solimed coordinators using Meridian daily | Login tracking |
| Visit logging accuracy | Same or better than Power Apps (zero regression) | Comparison audit Sprint 12 |
| Investigator spec automation | Monthly spec emails sent without manual intervention | Email send logs |
| Backlog accuracy | Multi-arm inflation resolved; ≤5% variance from manual calculation | Finance reconciliation |
| Coordinator NPS | ≥40 | Post-Phase-1 survey |
| AI call summary quality | Ivan + coordinator rating ≥4/5 on 20+ summaries | Manual rating survey |
| Dropout prediction accuracy | >60% precision on 30-day dropout flag | Retrospective model evaluation |

### Phase 2 Metrics (Second site)

| Metric | Target |
|---|---|
| Onboarding time for second site | <2 weeks from data import to first live visit |
| Patient qualification engine | >70% of top-10 candidates pass formal screening |
| EHR-sourced candidates | >20% of screening patients sourced via EHR integration |

### Phase 3 Metrics (Market)

| Metric | Target |
|---|---|
| Sites live | 5+ by Month 24 |
| ARR | €150K+ by Month 24 (ramp toward €600K Year 3 target) |
| Self-serve onboarding | New site live in <48 hours without Pivot involvement |
| Churn | <10% annual site churn |
| NPS (all customers) | ≥50 |

---

*This PM plan is a living document. It will be updated at the start of each phase based on what was learned in the prior phase. The sprint-level detail for Phase 2 and Phase 3 will be refined as Phase 1 progresses.*
