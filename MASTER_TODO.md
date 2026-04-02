# MASTER TODO — Pivot

**Last Updated:** 2026-04-02
**Overall Completion:** 22% (infrastructure + Solimed discovery + Meridian planning complete; engagement pending commercial close)

---

## Project Status

| Phase | Status | Completion |
|-------|--------|-----------|
| Phase 0: Infrastructure | ✅ Complete | 100% |
| Solimed: Discovery & Planning | ✅ Complete | 100% |
| Solimed: Commercial Close | ⏳ In Progress | 20% |
| Solimed: Sprint 1 | ⏳ Blocked (pre-requisites) | 0% |
| Meridian: Platform Vision | ✅ Complete | 100% |
| Meridian: PM Plan (18-month) | ✅ Complete | 100% |
| Meridian: Phase 0 (Team Setup) | ⏳ Not Started | 0% |
| Meridian: Phase 1 (Layers 1–3 MVP) | ⏳ Not Started | 0% |

---

## Phase 0: Infrastructure ✅

- [x] Create project folder structure
- [x] Initialize git repository
- [x] Configure remote (origin → KremerWTE/Pivot)
- [x] Set `main` as default branch
- [x] Create `Kremer-dev` branch
- [x] Add `.claude/` directives
- [x] Add `CLAUDE.md`
- [x] Add `.gitignore`
- [x] Add GitHub Actions workflows
- [x] Add PR and issue templates

---

## Solimed Engagement ⏳

### Discovery & Planning ✅ Complete
- [x] Analyze demo recording (85-minute MP4 transcribed via Whisper)
- [x] Document as-is process maps (8 flowcharts, 20 confirmed gaps)
- [x] Create enhancement roadmap (5 phases, 20 months)
- [x] Create PM plan (Phase 1 fully sprinted, all phases structured)
- [x] Create client proposal (€476K–€595K, 5 phases)
- [x] Create CTO technology assessment (6 dimensions, stack recommendation)
- [x] Create CTO draft gap analysis (vs Eric Garrison/WTE Solutions draft)
- [x] Create CFO revenue intelligence document (tailored to Drew Domescik)
- [x] Create project charter (Phase 1 authorization)
- [x] Create readiness assessment (CONDITIONALLY READY — 7 blockers identified)

### Commercial Close ⏳ In Progress
- [ ] NDA signed by both parties
- [ ] Phase 1 budget verbally confirmed by Ivan Kruljac
- [ ] Phase 1 SOW drafted and sent
- [ ] WTE Solutions / Eric Garrison engagement status clarified
- [ ] Contract signed

### Pre-Sprint 1 Prerequisites ⏳ Blocked on Commercial Close
- [ ] Mladen Geng 2 days/week formally confirmed
- [ ] Power Apps access provisioned
- [ ] Power BI workspace access provisioned
- [ ] Dataverse read access provisioned
- [ ] Azure subscription confirmed
- [ ] 2–3 coordinator names for UAT confirmed
- [ ] Finance role owner identified

### Sprint 1 (Discovery & Foundation) ⏳ Not Started
- [ ] Power Apps staging environment created
- [ ] Daily backup confirmed
- [ ] Full data audit completed
- [ ] All Dataverse tables documented
- [ ] Business logic extracted from Mladen
- [ ] Sprint review and UAT process agreed

### Phase 1 Features (Sprints 2–6) ⏳ Not Started
- [ ] Working hours flag + fee recalculation
- [ ] Multi-arm study arm assignment + backlog correction
- [ ] Screen fail allotment tracking
- [ ] Automated investigator spec generation
- [ ] Coordinator to-do dashboard + visit tolerance warnings
- [ ] Bulk visit status operations
- [ ] Study progress summary cards
- [ ] Site budget fix, override, lock/unlock
- [ ] Demand and time tracking with normita baseline
- [ ] Study show/stop lifecycle controls
- [ ] Overall budget/revenue/normita Power BI dashboard
- [ ] Revenue status ladder + cash flow forecast dashboards
- [ ] Backlog quality score dashboard

---

## Meridian Platform ⏳

### Vision & Planning ✅ Complete
- [x] Define platform concept (Crio + Dialpad + AI/predictive merged)
- [x] Name the platform: **Meridian**
- [x] Write full platform vision document (`pivot-platform-vision.md`)
- [x] Write 18-month PM plan for building Meridian (`meridian-pm-plan.md` v2.0)
- [x] Assess Claude Code feasibility — saves ~4–6 sprint-weeks; 18-month timeline stands
- [x] Identify Solimed as design partner / first customer

### Key Architecture Decisions ⏳ Phase 0 Week 2
- [ ] API layer: Node.js (Fastify) vs Python (FastAPI)
- [ ] Auth: Auth0 vs Azure AD B2C
- [ ] Database: PostgreSQL vs Azure SQL vs split
- [ ] Communications: Twilio vs Azure Communication Services
- [ ] FHIR layer: Azure Health Data Services vs build own
- [ ] Frontend mobile: Next.js only vs Next.js + React Native

### Phase 0: Foundation (Weeks 1–2, Parallel to Sprint 1) ⏳ Not Started
- [ ] Hire second Full-Stack Developer (critical to 18-month plan)
- [ ] Assemble core team (Product Lead, Tech Lead, BI Engineer, ML Engineer)
- [ ] Create GitHub repo: `pivot-meridian`
- [ ] Set up project management: Linear or Jira
- [ ] Write Architecture Decision Records (ADRs)
- [ ] Design multi-tenant data model
- [ ] Provision dev/staging/production Azure environments
- [ ] Set up CI/CD pipeline

### Phase 1: Solimed MVP — Layers 1–3 (Months 1–6) ⏳ Not Started
- [ ] Layer 1: Data Ingestion (Sprints 1–3) — migrate Solimed to PostgreSQL
- [ ] Layer 2: Payment Engine (Sprints 3–7) — full fee + invoicing + cash flow
- [ ] Layer 3: Clinical Operations (Sprints 4–11) — coordinator UI, off Power Apps
- [ ] Phase 1 close + security audit (Sprint 12)

### Phase 2: Second Site + AI — Layers 4–5 (Months 7–11) ⏳ Not Started
- [ ] Layer 4: Communication Hub (Sprints 13–16) — Twilio, AI transcription, coaching
- [ ] Second site onboarding (Sprints 13–19) — multi-tenancy, localization, second country
- [ ] Layer 5: AI Intelligence (Sprints 17–21) — dropout risk, patient qualification, forecasting
- [ ] Penetration test + GDPR review (Sprint 22)

### Phase 3: Market Launch (Months 12–18) ⏳ Not Started
- [ ] Self-serve onboarding wizard (Sprints 23–24)
- [ ] Subscription billing — Stripe (Sprint 25)
- [ ] Mobile app iOS/Android (Sprints 29–30)
- [ ] 5+ sites live (Sprint 31)
- [ ] SOC 2 Type II audit initiated (Sprint 36)
- [ ] Public market launch (Sprint 36)

---

## Backlog

- [ ] README.md — write project description for Meridian
- [ ] Architecture doc for Meridian in `docs/architecture/`
- [ ] Second site pipeline — identify candidates via Drew's network by Month 4
- [ ] Legal entity review for expansion countries (Phase 2 dependency)

---

## Recently Completed

| Date | Item |
|------|------|
| 2026-04-02 | Meridian PM Plan v2.0 — 18-month compressed plan (down from 24) |
| 2026-04-02 | Claude Code feasibility assessment for Meridian build |
| 2026-04-02 | Platform named: Meridian |
| 2026-04-02 | Full Solimed discovery — 10 deliverables (MD + DOCX each) |
| 2026-02-27 | Project infrastructure and folder structure created |
| 2026-02-27 | Git initialized, remote configured, branches set up |

---

## Notes

- Solimed is the first client engagement AND the Meridian design partner
- 18-month plan requires: 2 full-stack devs from Sprint 1; second site identified by Month 4
- Claude Code used throughout — accelerates code writing but timeline bottlenecks are elsewhere
- All deliverables in `docs/reports/` — both `.md` and `.docx` versions
- Active development branch: `Kremer-dev` — PRs to `main` only

---

**Next Review:** Before Solimed commercial close meeting
