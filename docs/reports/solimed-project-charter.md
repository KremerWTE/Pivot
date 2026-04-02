# Solimed Study Tracking Enhancement — Project Charter

**Project Name:** Solimed Study Tracking — Enhancement & International Expansion
**Project Sponsor:** Ivan Kruljac (Solimed)
**Project Manager:** TBD (Pivot)
**Technical Lead:** TBD (Pivot)
**Charter Version:** 1.0 — DRAFT
**Date:** 2026-04-02
**Status:** Pending signature

---

## 1. Project Purpose & Justification

Solimed operates a purpose-built SMO (Site Management Organization) platform — Study Tracking — that manages clinical trial operations across 2 sites in Croatia (Solimed Clinic and Medico RI). The platform currently manages 321 active enrolled patients, 15+ CRO relationships, and €2.1M in tracked financial backlog across studies running through 2032.

The platform was built deliberately over 3+ years and is working well at its current scale. It is now approaching the ceiling of its Power Apps architecture — a ceiling that will prevent international expansion, third-party integrations, and enterprise-grade security and compliance.

**The purpose of this project is to:**
1. Resolve confirmed operational gaps in the current platform (Phase 1)
2. Re-architect the foundation for scale, security, and multi-tenancy (Phase 2)
3. Expand the platform to new international markets (Phase 3)
4. Integrate the platform with the broader clinical trial ecosystem (Phase 4)
5. Productize the platform as a scalable SaaS offering (Phase 5)

**Business justification:**
- The €2.1M backlog figure is currently unreliable due to multi-arm study inflation
- Investigator fee calculations are incomplete without working hours differentiation
- Screen fail revenue recognition is untracked — creating billing exposure
- International expansion is commercially blocked by the current single-tenant, single-locale architecture
- The SaaS opportunity in the CTMS/SMO space (€3.5B market, 13% CAGR) is directly accessible to Solimed if the platform is productized

---

## 2. Project Scope

### In Scope

**Phase 1 — Quick Wins (Months 1–3):**
- Working hours flag and fee recalculation engine
- Multi-arm study arm assignment and backlog correction
- Screen fail allotment tracking and revenue recognition split
- Automated investigator specification generation and sending
- Coordinator to-do dashboard and visit tolerance warnings
- Bulk visit status operations
- Study progress summary cards
- Site budget fix, override, and lock/unlock controls
- Demand and time tracking with normita baseline
- Study show/stop lifecycle controls with auto-generated stop reports
- Overall budget, revenue, and normita Power BI dashboard
- Revenue status ladder and cash flow forecast dashboards
- Backlog quality score dashboard

**Phase 2 — Platform Hardening (Months 3–5):**
- REST API layer (all core entities)
- Multi-tenant data model with country/site isolation
- 6-tier RBAC implementation
- PII audit logging (GDPR-ready)
- Azure infrastructure setup (Dev, Staging, Production)
- CI/CD pipeline and automated test suite

**Phase 3 — International Expansion (Months 6–9):**
- i18n/localization framework
- Multi-currency engine with FX rates
- Country-specific regulatory configuration
- Data residency per country (Azure region)
- Country onboarding playbook
- First non-Croatia site live

**Phase 4 — Ecosystem Integration (Months 10–14):**
- EDC bidirectional sync (minimum 1 CRO system)
- Automated invoicing engine
- Mobile application (iOS + Android)
- Document management system

**Phase 5 — SaaS Productization (Months 15–20):**
- Self-service tenant onboarding
- Subscription billing engine
- CRO network portal
- AI-driven predictive features

### Explicitly Out of Scope

- Patient-facing portal, patient recruitment engine, or patient communication system (CRO/sponsor domain)
- Electronic Data Capture (EDC) — Solimed does not and will not replicate clinical data from CRO EDC systems
- Source documentation digitalization — paper source docs remain per CRO advisory
- Replacement of Power BI reporting with a different BI tool
- Any changes to CRO-owned systems or sponsor systems
- Clinical data entry or modification of any kind
- 21 CFR Part 11 compliance (unless a US expansion is formally scoped in Phase 3+)
- Hardware procurement or on-premise infrastructure

### Assumptions

1. Mladen Geng is available for a minimum of 2 days/week during Sprints 1–2 for knowledge transfer
2. Ivan Kruljac and Drew Domescik are available for bi-weekly sprint reviews (1 hour each)
3. At least 2–3 site coordinators are available for usability testing in Phase 1
4. Solimed has or will provision necessary Azure licenses and subscriptions
5. The existing Power Apps/Dataverse environment remains operational throughout Phase 1–2
6. All existing data in the platform is accurate and suitable for migration (data quality to be validated in Sprint 1)
7. Solimed has legal authority to store patient data (anonymized IDs) in the current platform

### Constraints

1. Coordinators cannot be disrupted during active study periods — production releases must be scheduled during low-activity windows
2. Investigator payment specifications must continue to run correctly throughout all phases — no disruption to the monthly payment cycle
3. Power Apps must remain the UI through Phase 2 at minimum (no big-bang rewrite)
4. All development must remain within the Microsoft Azure ecosystem

---

## 3. Project Objectives & Success Criteria

| Objective | Measurable Success Criterion |
|---|---|
| Fix financial forecast integrity | Multi-arm backlog inflation resolved; €2.1M figure reconciled to arm-assigned value within 5% |
| Fix fee calculation accuracy | Working hours flag live; zero investigator payment disputes attributable to hours-type calculation errors for 3 consecutive months |
| Automate investigator specs | Monthly specification emails sending automatically; finance team spends <30 minutes/month on investigator payment prep vs. current baseline |
| Establish API layer | API v1 live with >80% test coverage; all Phase 1 Power Apps screens reading/writing through API |
| Achieve multi-tenant architecture | Second site or country added to platform without schema changes |
| First international expansion | First non-Croatia site live, processing real studies within Phase 3 timeline |
| Revenue intelligence | Drew Domescik confirms the backlog quality dashboard and cash flow forecast meet CFO-level reporting needs |
| Platform NPS | Coordinator NPS ≥40 post Phase 1 launch |

---

## 4. Stakeholders

| Name | Organization | Role | Engagement Level |
|---|---|---|---|
| Ivan Kruljac | Solimed | Project Sponsor / Business Owner | High — weekly involvement |
| Drew Domescik | Advisor | CFO Advisor / Financial Feature Sponsor | Medium — sprint reviews + financial UAT |
| Mladen Geng | Solimed | Current Developer / Domain Expert | High — Sprints 1–2; then as-needed |
| Site Coordinators (2–3) | Solimed | End Users — primary app users | Medium — usability testing and UAT |
| Finance Role (TBD) | Solimed | Billing and investigator payment owner | Medium — billing workflow UAT |
| Site Managers (TBD) | Solimed | Study setup and budget configuration | Low-Medium — admin feature UAT |
| Pivot PM | Pivot | Delivery Owner | High — daily |
| Pivot Technical Lead | Pivot | Architecture and development | High — daily |
| Pivot BI Engineer | Pivot | Power BI development | High — Sprint 6+ |

---

## 5. High-Level Timeline

| Phase | Duration | Target Start | Target End |
|---|---|---|---|
| Phase 1 — Quick Wins | 3 months | Month 1 | Month 3 |
| Phase 2 — Platform Hardening | 3 months | Month 3 | Month 5 |
| Phase 3 — International Expansion | 4 months | Month 6 | Month 9 |
| Phase 4 — Ecosystem Integration | 5 months | Month 10 | Month 14 |
| Phase 5 — SaaS Productization | 6 months | Month 15 | Month 20 |
| **Total** | **20 months** | | |

---

## 6. Budget Authorization

| Phase | Budget Range | Authorization |
|---|---|---|
| Phase 1 | €38,000 – €50,000 | Required before Sprint 1 start |
| Phase 2 | €72,000 – €90,000 | Required before Phase 2 start |
| Phase 3 | €96,000 – €120,000 | Required before Phase 3 start |
| Phase 4 | €130,000 – €160,000 | Required before Phase 4 start |
| Phase 5 | €140,000 – €175,000 | Required before Phase 5 start |
| **Total** | **€476,000 – €595,000** | |

Budget for each phase is authorized separately. Solimed is not committed to subsequent phases by authorizing Phase 1.

Infrastructure costs (Azure, Power BI Embedded, third-party APIs) are billed directly to Solimed and are not included above. Estimated at €500–€2,000/month depending on phase.

---

## 7. Risks

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Mladen unavailable for knowledge transfer | Medium | Critical | Prioritize in Sprint 1; record all sessions |
| Power Apps limitations block Phase 1 features | Medium | High | Assess feasibility Sprint 1; parallel API track if needed |
| Scope creep across phases | High | Medium | Formal change control; all additions require written approval |
| Data quality issues discovered during migration | Medium | High | Data audit Sprint 1 before any migration begins |
| Coordinator resistance to new features | Medium | Medium | Paced rollout per Ivan's preference; change management plan |
| Regulatory requirements unknown for target expansion country | High | High | Regulatory review begins Phase 2; legal counsel engaged |
| Clinical data privacy breach | Low | Critical | Security audit before Phase 3; penetration testing |

---

## 8. Governance

**Decision authority:**
- Feature scope changes: Ivan Kruljac (Solimed) + Pivot PM joint approval
- Technical architecture: Pivot Technical Lead with Ivan review
- Budget changes >5%: Ivan Kruljac written approval
- Production deployments: Pivot PM + Solimed written sign-off

**Escalation path:**
Pivot PM → Ivan Kruljac → Drew Domescik (if financial/strategic escalation)

**Change control:**
All scope changes submitted in writing. Pivot assesses impact within 3 business days. Changes >16 hours require a formal change order signed by Ivan Kruljac before work begins.

---

## 9. Charter Approval

By signing below, both parties agree to the project purpose, scope, objectives, timeline, and governance described in this charter. This charter authorizes Phase 1 work to begin.

| | Solimed | Pivot |
|---|---|---|
| **Signed** | _________________ | _________________ |
| **Name** | Ivan Kruljac | |
| **Title** | Co-founder | |
| **Date** | | |

---

*This charter authorizes Phase 1 only. Subsequent phases require separate budget authorization and updated scope confirmation before commencement.*
