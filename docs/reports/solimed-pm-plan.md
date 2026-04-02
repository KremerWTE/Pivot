# Solimed Study Tracking — Project Management Plan

**Client:** Solimed
**Contractor:** Pivot
**Date:** 2026-04-02
**Version:** 1.0

---

## 1. Project Overview

Pivot has been engaged to enhance Solimed's Study Tracking application and architect its expansion to multiple countries. This PM plan covers governance, delivery structure, resourcing, risk management, and communication protocols for a 20-month engagement across 5 phases.

---

## 2. Project Governance

### 2.1 Stakeholders

| Name | Role | Responsibility | Involvement |
|---|---|---|---|
| Ivan Kruljac | Solimed Co-founder / Business Owner | Business direction, commercial decisions, sign-off on scope | Weekly sync |
| Drew Domescik | Advisor (CFO background, 20 yrs clinical research SMO/site networks) | Financial feature validation, revenue recognition, UAT | Sprint reviews |
| Mladen Geng | App Developer & Project Manager (3+ yrs on platform) | Technical handover, domain/process knowledge, architecture context | Onboarding + as needed |
| Pivot PM | Project Manager | Day-to-day delivery, risk tracking | Daily |
| Pivot Lead Dev | Technical Lead | Architecture decisions, code quality | Daily |

### 2.2 Decision Authority

| Decision Type | Authority | Escalation Path |
|---|---|---|
| Feature scope changes | Solimed stakeholders | Ivan Kruljac |
| Technical architecture | Pivot Lead Dev | Joint Pivot + Solimed review |
| Timeline changes >1 week | Joint approval | Written confirmation required |
| Budget changes >5% | Ivan Kruljac | Formal change order |
| Country expansion priority | Solimed stakeholders | Drew Domescik |

### 2.3 RACI Matrix

| Activity | Pivot PM | Pivot Dev | Ivan K. | Drew D. | Mladen G. |
|---|---|---|---|---|---|
| Requirements gathering | A | C | R | R | C |
| Technical design | C | A/R | I | I | C |
| Development | I | A/R | I | I | C |
| QA / Testing | A | R | C | C | I |
| UAT | C | C | A/R | R | I |
| Deployment | A | R | I | I | I |
| Training | A/R | C | I | R | I |

*R = Responsible, A = Accountable, C = Consulted, I = Informed*

---

## 3. Delivery Framework

### 3.1 Methodology
**Agile Scrum** with 2-week sprints.

- Sprint planning: Day 1 of each sprint (Monday)
- Daily standup: Async via Teams/Slack (written update)
- Sprint review: Last Friday of each sprint — demo to Solimed stakeholders
- Sprint retrospective: Internal Pivot team only
- Backlog refinement: Wednesday of week 1 each sprint

### 3.2 Definition of Ready (before work starts)
- [ ] User story written with acceptance criteria
- [ ] Designs/wireframes approved (if UI work)
- [ ] Dependencies identified and resolved
- [ ] Story points estimated
- [ ] Assigned to a sprint

### 3.3 Definition of Done (before work closes)
- [ ] Code complete and peer reviewed
- [ ] Unit tests written and passing
- [ ] Integration tests passing
- [ ] QA sign-off
- [ ] Documentation updated
- [ ] Demo-able to stakeholders
- [ ] Deployed to staging environment

---

## 4. Phase-by-Phase Plan

### Phase 1 — Quick Wins (Months 1–2 / Sprints 1–4)

**Goal:** Deliver immediate value in Power Apps without platform migration.

| Sprint | Deliverables |
|---|---|
| Sprint 1 | Discovery & knowledge transfer from Mladen; data model documentation; backlog population; working hours flag design |
| Sprint 2 | Working hours flag + fee calculation engine (confirmed #1 priority); multi-arm study arm assignment UI |
| Sprint 3 | Multi-arm backlog recalculation engine; screen fail ratio tracking + allotment alerts |
| Sprint 4 | Automated investigator specification sending; coordinator to-do dashboard; visit tolerance warnings |
| Sprint 5 | Bulk status operations; study progress summary; site budget fix & override; audit trail |
| Sprint 6 | Power BI: site filter on backlog, patient-level backlog, normita dashboard; Phase 1 UAT + sign-off |

**Milestones:**
- M1.1: Knowledge transfer complete (end Sprint 1)
- M1.2: Phase 1 features live in production (end Sprint 4)

**Key Risks:**
- Mladen Geng availability for knowledge transfer
- Power Apps platform limitations blocking features (mitigation: assess early in Sprint 1)

---

### Phase 2 — Platform Hardening (Months 3–5 / Sprints 5–10)

**Goal:** Build API layer, expand RBAC, establish multi-tenant data model.

| Sprint | Deliverables |
|---|---|
| Sprint 5 | Architecture design document; technology stack confirmed; Azure environment setup |
| Sprint 6 | Multi-tenant data model design; database schema; Azure AD B2C configuration |
| Sprint 7 | Core API: Studies, Sites, Patients endpoints |
| Sprint 8 | Core API: Visits, Budgets, Investigators endpoints; RBAC implementation |
| Sprint 9 | Security baseline: encryption, audit logging, PII access controls; API documentation |
| Sprint 10 | Integration testing; performance testing; Phase 2 UAT + sign-off |

**Milestones:**
- M2.1: Architecture approved by both parties (end Sprint 5)
- M2.2: API v1 complete with full test coverage (end Sprint 9)
- M2.3: Security audit passed (end Sprint 10)

**Key Risks:**
- Data migration complexity from Power Apps data sources
- Azure AD B2C configuration complexity for multi-tenant
- API design requiring significant Solimed business logic documentation

---

### Phase 3 — International Expansion (Months 6–9 / Sprints 11–18)

**Goal:** Deploy to first new country.

| Sprint | Deliverables |
|---|---|
| Sprint 11 | Target country selected; regulatory requirements gathered; i18n framework implemented |
| Sprint 12 | Multi-currency engine; FX rate integration; locale-aware formatting |
| Sprint 13 | Country-specific data residency config; Azure region deployment for new country |
| Sprint 14 | Tax/billing rules engine; country-specific required fields |
| Sprint 15 | Country onboarding wizard; pilot site setup in new country |
| Sprint 16 | Pilot study go-live; hypercare monitoring |
| Sprint 17 | Issues resolution; performance tuning; second locale (if applicable) |
| Sprint 18 | Phase 3 retrospective; documentation; expansion playbook finalized |

**Milestones:**
- M3.1: Target country confirmed + regulatory review complete (end Sprint 12)
- M3.2: First non-Croatia site live on platform (end Sprint 16)
- M3.3: Expansion playbook ready for reuse (end Sprint 18)

**Key Risks:**
- Regulatory requirements in target country unknown until research complete
- Data residency requirements may require significant Azure infrastructure
- Local language translation quality and clinical terminology accuracy

---

### Phase 4 — Ecosystem Integration (Months 10–14 / Sprints 19–28)

**Goal:** EDC integration, automated invoicing, mobile app, document management.

| Sprint | Deliverables |
|---|---|
| Sprint 19–20 | EDC integration scoping; API contracts with target EDC system(s) |
| Sprint 21–22 | EDC sync: visit status bidirectional sync |
| Sprint 23–24 | Automated invoicing engine; invoice template builder; PDF generation |
| Sprint 25–26 | Mobile app MVP: visit logging, schedule view, offline sync |
| Sprint 27 | Document management: upload, version control, expiry tracking |
| Sprint 28 | Phase 4 integration testing; UAT; sign-off |

**Milestones:**
- M4.1: EDC integration live with at least 1 CRO (end Sprint 22)
- M4.2: Automated invoicing reducing manual billing effort by 80% (end Sprint 24)
- M4.3: Mobile app in app stores (end Sprint 26)

**Key Risks:**
- EDC vendor API access and licensing costs
- Mobile app store approval timelines (2–4 weeks)
- Accounting system integration complexity varies by country

---

### Phase 5 — SaaS Productization (Months 15–20 / Sprints 29–40)

**Goal:** Self-service tenant onboarding, subscription billing, AI features.

| Sprint | Deliverables |
|---|---|
| Sprint 29–31 | Self-service onboarding wizard; tenant provisioning automation |
| Sprint 32–33 | Subscription billing engine (Stripe); usage metering |
| Sprint 34–35 | CRO marketplace / shared network directory |
| Sprint 36–37 | AI: visit no-show prediction; budget overrun early warning |
| Sprint 38–39 | Regulatory intelligence layer; country change monitoring |
| Sprint 40 | Platform launch; public documentation; Phase 5 sign-off |

---

## 5. Resourcing Plan

### 5.1 Pivot Team

| Role | Phase 1 | Phase 2 | Phase 3 | Phase 4 | Phase 5 |
|---|---|---|---|---|---|
| Project Manager | 0.5 FTE | 0.5 FTE | 0.75 FTE | 0.75 FTE | 1.0 FTE |
| Technical Lead / Architect | 0.5 FTE | 1.0 FTE | 0.75 FTE | 0.75 FTE | 0.5 FTE |
| Full-Stack Developer | 1.0 FTE | 2.0 FTE | 2.0 FTE | 2.0 FTE | 2.0 FTE |
| Power Apps Developer | 1.0 FTE | 0.5 FTE | 0.25 FTE | 0 FTE | 0 FTE |
| Mobile Developer | 0 FTE | 0 FTE | 0 FTE | 1.0 FTE | 0.5 FTE |
| QA Engineer | 0.5 FTE | 1.0 FTE | 1.0 FTE | 1.0 FTE | 1.0 FTE |
| UX Designer | 0.25 FTE | 0.5 FTE | 0.5 FTE | 0.5 FTE | 0.5 FTE |
| Data / BI Engineer | 0 FTE | 0.5 FTE | 0.5 FTE | 0.25 FTE | 0.25 FTE |

### 5.2 Solimed Requirements
- **Mladen Geng:** Available 2–3 days/week during Sprint 1–2 for knowledge transfer
- **Ivan Kruljac / Drew Domescik:** Available for sprint reviews (bi-weekly, 1 hour) + UAT sign-off
- **Subject matter experts:** Site coordinators available for usability testing (Phase 2 onwards)
- **Legal/compliance contact:** Required before Phase 3 (international regulatory review)

---

## 6. Risk Register

| # | Risk | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|---|
| R1 | Power Apps platform limits block Phase 1 features | Medium | High | Assess in Sprint 1; parallel-track API build if needed | Pivot Lead Dev |
| R2 | Mladen Geng unavailable for knowledge transfer | Medium | High | Record all sessions; document all data models immediately | Pivot PM |
| R3 | Regulatory requirements delay Phase 3 | High | High | Start regulatory research in Phase 2; engage local legal counsel | Solimed + Pivot PM |
| R4 | EDC vendor API access not granted in time | Medium | High | Pre-engage vendors in Phase 2; identify alternatives | Pivot Lead Dev |
| R5 | Scope creep across phases | High | Medium | Strict change control process; all scope changes require written approval | Pivot PM |
| R6 | Clinical data privacy breach | Low | Critical | Security audit Phase 2; penetration testing before each country launch | Pivot Lead Dev |
| R7 | Key Pivot team member departure | Low | High | Cross-train all roles; documentation standards enforced | Pivot PM |
| R8 | Target country market entry blocked | Low | High | Select 2–3 candidate countries; proceed with least-complex first | Solimed |
| R9 | Power BI licensing costs increase at scale | Medium | Medium | Evaluate Power BI Embedded pricing tiers in Phase 2 | Pivot PM |

---

## 7. Communication Plan

### 7.1 Regular Cadences

| Meeting | Frequency | Participants | Duration | Format |
|---|---|---|---|---|
| Sprint Review / Demo | Bi-weekly | All stakeholders | 60 min | Video call |
| Steering Committee | Monthly | Ivan K., Drew D., Pivot PM, Lead Dev | 60 min | Video call |
| Daily Standup | Daily | Pivot team only | 15 min | Async Teams message |
| Backlog Refinement | Bi-weekly | Pivot team + 1 Solimed rep | 60 min | Video call |
| Incident Response | As needed | Pivot PM + relevant Solimed contact | ASAP | Phone/Teams |

### 7.2 Reporting

| Report | Frequency | Recipient | Contents |
|---|---|---|---|
| Sprint Summary | Bi-weekly | Ivan K., Drew D. | Completed items, velocity, risks, next sprint plan |
| Phase Status Report | Monthly | All stakeholders | Phase progress %, milestones, budget, risks |
| Risk Register Update | Monthly | Ivan K. | Updated risk status, new risks, mitigations |
| Budget Tracker | Monthly | Ivan K. | Hours consumed, budget remaining, forecast |

### 7.3 Tools

| Tool | Purpose |
|---|---|
| GitHub (this repo) | All code, documentation, session notes |
| Jira / Linear | Sprint backlog, story tracking |
| Microsoft Teams | Stakeholder communication, meeting recordings |
| Confluence / Notion | Shared documentation, runbooks |
| Figma | UX designs and prototypes |

---

## 8. Quality Assurance Plan

### 8.1 Testing Strategy

| Test Type | When | Who | Threshold |
|---|---|---|---|
| Unit Tests | Every PR | Developer | >80% coverage |
| Integration Tests | Every sprint | QA Engineer | All API endpoints covered |
| Regression Tests | Before every release | QA Engineer | 100% pass |
| UAT | End of each phase | Solimed stakeholders | Signed acceptance |
| Security / Pen Test | Before each country launch | External firm | No critical/high findings |
| Performance Test | Phase 2 and Phase 3 | Pivot Lead Dev | <2s page load at 100 concurrent users |
| Usability Test | Phase 2 and Phase 4 | Pivot UX + 3 coordinators | Task completion >90% |

### 8.2 Release Process
1. Feature complete → merge to `staging`
2. QA regression pass on staging
3. Stakeholder demo / UAT on staging
4. Written sign-off from Solimed
5. Deploy to production (off-hours, with rollback plan)
6. 48-hour hypercare monitoring post-release

---

## 9. Change Management

### 9.1 Change Request Process
1. Change request submitted (email or Jira ticket) with: description, rationale, estimated impact
2. Pivot PM assesses: effort, timeline impact, cost impact within 3 business days
3. If impact < 4 hours: absorbed in current sprint with stakeholder notification
4. If impact 4–16 hours: sprint backlog reprioritization, joint approval required
5. If impact > 16 hours: formal change order, signed by Ivan Kruljac, before work begins

### 9.2 Scope Freeze Periods
- 1 week before each phase UAT: no new scope accepted
- 48 hours before production releases: no changes except critical bug fixes

---

## 10. Success Criteria

The engagement is considered successful when:

- [ ] Phase 1: All quick wins live in production, Solimed coordinator team using daily
- [ ] Phase 2: API v1 complete, security audit passed, multi-tenant model live
- [ ] Phase 3: First new country site live and processing real studies
- [ ] Phase 4: EDC integration active; mobile app live; automated invoicing processing real invoices
- [ ] Phase 5: 5+ countries live; self-serve onboarding working; platform operating without Pivot intervention

---

*This plan will be updated following the full transcript review and initial discovery sessions with Solimed.*
