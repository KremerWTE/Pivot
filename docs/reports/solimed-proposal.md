# Pivot — Proposal for Solimed Study Tracking Enhancement & International Expansion

**Prepared for:** Solimed (Ivan Kruljac, Drew Domescik)
**Prepared by:** Pivot
**Date:** April 2, 2026
**Proposal Reference:** PIVOT-SOL-2026-001
**Valid for:** 60 days

---

## Executive Summary

Solimed has built a working clinical trial site management platform — **Study Tracking** — that handles the operational and financial complexity of running a Site Management Organization (SMO). The platform currently serves 2 sites in Croatia and demonstrates strong domain-specific capabilities: multi-CRO study management, visit lifecycle tracking, granular budget management, and sophisticated Power BI financial reporting.

Pivot proposes to become Solimed's technology partner to:

1. **Enhance** the existing platform with immediate quality-of-life improvements
2. **Re-architect** the foundation for enterprise-grade scale, security, and multi-tenancy
3. **Expand** the platform to new international markets
4. **Integrate** the platform with the broader clinical trial ecosystem
5. **Productize** the platform as a scalable SaaS offering

This is not a rebuild-from-scratch engagement. We will build on what works, modernize what limits growth, and scale what has proven value — all while keeping Solimed operational throughout.

---

## The Opportunity

### Where Solimed Is Today

Solimed's Study Tracking app manages:
- **15+ CROs** across hundreds of clinical studies
- **2 sites** (Solimed Clinic, Medico RI) in Croatia
- **321 active enrolled patients** across all current studies
- **~€900K current-year backlog** and **€2.1M total backlog** across studies running through 2032
- **Detailed visit workflows** across Regular, Virtual, SCR, EOT, and procedure-specific visit types
- **Investigator fee management** with PI cut, PI fee, sub-investigator splits, and referral doctor fees
- **Two separate PNL dashboards** — one for Solimed, one shared live with Medico RI management

The platform was built deliberately — existing CTMS vendors were "too bloated" for Solimed's specific model. It was built incrementally from Excel → Power Apps, and it is working. The fundamentals are right. The opportunity is in scale and in closing the specific gaps the team has already identified.

### The Gap

The platform is built on Microsoft Power Apps — an excellent low-code tool for single-organization use, but one that creates a ceiling for:
- **International expansion** (no localization, single currency, single-region data)
- **Enterprise integrations** (EDC systems, accounting platforms, CRO portals)
- **Mobile use** (field investigators cannot log visits without a desktop)
- **Automation** (visit approvals, invoicing, and alerts are largely manual)
- **Scale** (Power Apps performance degrades with volume and concurrent users)

### The Market Potential

The global Clinical Trial Management System (CTMS) market is projected to reach **$3.5B by 2028** (CAGR ~13%). SMOs with proprietary technology platforms command significant competitive advantages in:
- CRO partnerships (technology-enabled SMOs win more studies)
- Site acquisition (smaller sites join networks with proven tools)
- International expansion (platform-as-a-service model)

Solimed's platform, enhanced and scaled, becomes both an operational tool and a competitive differentiator.

---

## What We Propose

### Approach: Evolutionary, Not Disruptive

We will not hand Solimed a blank page and ask them to wait 18 months for a result. Our approach:

- **Deliver value in weeks**, not quarters — starting with Phase 1 improvements that go live within 2 months
- **Preserve continuity** — the Power Apps interface remains operational throughout the transition
- **Build incrementally** — each phase delivers standalone value; no phase requires the next to succeed
- **Stay in the Microsoft ecosystem** — Azure, Power BI, and Azure AD are retained; we extend, not replace

---

## Scope of Work

### Phase 1 — Quick Wins (2 Months)
*Working within the current Power Apps platform*

**Deliverables (prioritized by confirmed pain points from Jan 23, 2026 demo):**

- **Working Hours Flag** *(#1 confirmed priority)* — In-hours vs. after-hours toggle on every visit; auto-applies correct investigator fee rate and site compensation; retroactively correctable before Approved status
- **Multi-Arm Study Backlog Fix** *(highest financial impact)* — Arm assignment per patient post-randomization; backlog only counts assigned arm; eliminates the inflated €2.1M figure for branching protocols
- **Screen Fail Ratio Tracking** *(revenue recognition control)* — Contractual allotment per study; running counter; alert when exceeded; billable vs. over-allotment split in PNL — addresses Drew's concern about high screen-fail gastroenterology studies
- **Automated Investigator Specification Sending** *(confirmed ready to automate)* — Monthly spec auto-generated and emailed to each investigator; finance review gate optional; investigator discrepancy correction flow
- **Coordinator To-Do Dashboard** — Personal cross-study view of upcoming and overdue visits; one-click status update
- **Visit Tolerance Warning System** — Color-coded flags (Green/Yellow/Red) for visits approaching or past their scheduling window
- **Bulk Status Operations** — Select and update multiple visits simultaneously
- **Study Progress Summary** — Per-study card: % visits complete, % budget consumed, visits at risk
- **Site Budget Fix & Override** — Site-level budget defaults; lock/unlock controls; budget correction log with approver tracking
- **Demand & Time Tracking** — 90-day visit demand forecast; time normita (derived from visit budget using Ivan's confirmed approach: budget → time estimate); planned vs. actual hours comparison
- **Study Show / Stop Controls** — Full lifecycle: Draft → Open → On Hold → Stopped → Locked; auto-generated Stop Report
- **Overall Budget, Revenue & Normita Dashboard** — Platform-wide financials; 12-month cash flow forecast; site filter on backlog (noted live by Ivan during demo); patient-level backlog drill-down (requested by Drew); normita-based visit negotiation tool
- **Enhanced Power BI Dashboards** — CRO profitability comparison; PDF/Excel export
- **Audit Trail** — Full change history for budgets and visit status

**Outcome:** Working hours fee calculation is accurate; backlog figures are trustworthy; screen fail revenue is correctly recognized; investigator specifications send automatically; coordinators and management have the visibility they described needing in the demo.

---

### Phase 2 — Platform Hardening (3 Months)
*Building the foundation for international scale*

**Deliverables:**
- **REST API Layer** — A proper API exposing all core entities, enabling mobile, integrations, and third-party access
- **Multi-Tenant Architecture** — Data model updated to support multiple countries, sites, and organizations with full isolation
- **Expanded RBAC** — 6-tier role model (Platform Admin → Country Admin → Site Admin → Coordinator → Investigator → Read-Only)
- **Security Baseline** — Encryption, PII audit logging, GDPR-ready consent tracking, session management
- **Azure Infrastructure** — Environments: Development, Staging, Production, with CI/CD pipelines

**Outcome:** Solimed's platform is enterprise-ready, secure, and can support expansion to any geography.

---

### Phase 3 — International Expansion (4 Months)
*First new country live*

**Deliverables:**
- **Localization (i18n) Framework** — All UI strings externalized; locale-aware dates, numbers, currencies
- **Multi-Currency Engine** — Per-site currency configuration with live FX rates; historical rate preservation
- **Country Configuration Layer** — Per-country regulatory fields, data residency (Azure region), tax rules
- **Country Onboarding Playbook** — Repeatable process for launching each new country in <2 weeks
- **Pilot Country Deployment** — First non-Croatia site live and processing real studies

**Outcome:** Solimed can enter a new country and have a site operational in under 2 weeks.

---

### Phase 4 — Ecosystem Integration (5 Months)
*Connecting to the clinical trial technology ecosystem*

**Deliverables:**
- **EDC Integration** — Bidirectional sync with at least one major EDC system (Medidata, Veeva, or REDCap) — eliminates double-entry
- **Automated Invoicing** — Invoice generation triggered by approved visits; PDF creation; delivery to CROs
- **Mobile Application** — iOS and Android app for investigators: view schedules, log visits offline, sync when connected
- **Document Management** — Protocol documents, consent forms, regulatory filings with version control and expiry tracking

**Outcome:** Solimed eliminates manual data entry between systems; investigators can log from anywhere; billing is automated.

---

### Phase 5 — SaaS Productization (6 Months)
*Turning Solimed's tool into a scalable product*

**Deliverables:**
- **Self-Service Onboarding** — New sites and countries can onboard without Solimed staff intervention
- **Subscription Billing** — Per-site or per-study pricing with automated billing
- **CRO Network Portal** — CROs access their study data across all connected SMO sites
- **AI-Powered Insights** — Visit no-show prediction, budget overrun early warning, scheduling optimization
- **Regulatory Intelligence** — Monitoring of country-specific regulatory changes affecting active studies

**Outcome:** Solimed's platform becomes a product it can sell/license to other SMOs — a new revenue stream.

---

## Engagement Model

### Our Team

| Role | Responsibility |
|---|---|
| Project Manager | Delivery ownership, stakeholder communication, risk management |
| Technical Architect | System design, technology decisions, code quality oversight |
| Full-Stack Developers (2) | Feature development, API, database |
| Power Apps Developer | Phase 1 and transition work |
| Mobile Developer | Phase 4 iOS/Android app |
| QA Engineer | Testing, quality gates, release management |
| UX Designer | User experience design and usability testing |
| BI/Data Engineer | Power BI enhancements, data model optimization |

### Working Rhythm
- **2-week sprints** with working demos every sprint
- **Bi-weekly sprint reviews** with Solimed stakeholders (1 hour)
- **Monthly steering committee** for strategic direction
- **All decisions documented** — no verbal-only agreements

### Transparency
- Full access to our sprint backlog at all times
- Weekly written status reports
- Budget tracking with actuals vs. forecast updated monthly
- All code hosted in your GitHub repositories — you own everything we build

---

## Investment

### Phase Estimates

| Phase | Duration | Team Size (avg) | Estimated Investment |
|---|---|---|---|
| Phase 1 — Quick Wins | 2–3 months | 3.5 FTE | €38,000 – €50,000 |
| Phase 2 — Platform Hardening | 3 months | 5.5 FTE | €72,000 – €90,000 |
| Phase 3 — International Expansion | 4 months | 5.75 FTE | €96,000 – €120,000 |
| Phase 4 — Ecosystem Integration | 5 months | 6.0 FTE | €130,000 – €160,000 |
| Phase 5 — SaaS Productization | 6 months | 5.5 FTE | €140,000 – €175,000 |
| **Total (full engagement)** | **20–21 months** | | **€476,000 – €595,000** |

> **Note:** Phases are independently scoped and can be contracted separately. Solimed is not committed to all phases by engaging for Phase 1.

### What's Included
- All design, development, testing, and deployment
- Code documentation and technical handover documentation
- Knowledge transfer sessions at each phase end
- 30-day warranty period per phase (bug fixes at no charge)
- Monthly infrastructure cost estimates (Azure, Power BI licensing)

### What's Not Included
- Azure infrastructure costs (estimated €500–€2,000/month depending on phase, billed directly to Solimed)
- Third-party software licenses (EDC vendor API access, e-signature, etc.)
- Legal/regulatory counsel for country-specific compliance
- Translation services for languages beyond English/Croatian
- Ongoing support/maintenance after engagement (quoted separately)

### Payment Structure
- 20% upon contract signing
- 30% at mid-phase milestone
- 50% upon phase completion and sign-off
- Invoiced monthly for time-and-materials phases with cap per phase

---

## Why Pivot

**We understand clinical research.** We analyzed Solimed's platform in depth before writing this proposal — we understand CRO relationships, visit tolerance windows, investigator fee structures, PI cuts, and the operational complexity of running a multi-study SMO. We will not need six weeks to "get up to speed."

**We build for real use, not demos.** Every phase delivers working software that Solimed coordinators, investigators, and finance teams use day-to-day. We do not build prototypes that never reach production.

**We work in your ecosystem.** We will not rip out Power BI or force you off Microsoft tools. We extend what you have, add what you need, and replace only what holds you back.

**We are transparent partners.** You will always know what we are building, why, and what it costs. No surprises.

---

## Next Steps

1. **Kick-off call** (this week) — walkthrough of this proposal, questions, scope alignment
2. **Discovery session** (week 2) — structured interviews with coordinators and Mladen Geng; data model review
3. **Phase 1 SOW** — formal Statement of Work for Phase 1 signed within 2 weeks of kick-off
4. **Sprint 1 start** — within 1 week of SOW signature

We are ready to begin.

---

## Acceptance

By signing below, Solimed engages Pivot for Phase 1 of this proposal under the terms described.

| | Pivot | Solimed |
|---|---|---|
| **Signed** | _________________ | _________________ |
| **Name** | | Ivan Kruljac |
| **Title** | | |
| **Date** | | |

---

*Appendices available upon request: Detailed sprint plans, technology architecture diagrams, reference client list, team CVs.*

---

**Pivot** | Proposal Reference PIVOT-SOL-2026-001 | Valid through June 2, 2026
