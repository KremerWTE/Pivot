# Solimed — Engagement Readiness Assessment

**Prepared by:** Pivot
**Date:** 2026-04-02
**Purpose:** Assess whether Solimed has the organizational, technical, and commercial readiness to begin the engagement and what gaps need to be addressed before Sprint 1 starts.
**Source:** January 23, 2026 demo recording + transcript analysis

---

## Overview

A readiness assessment protects both parties. If Solimed is not ready on key dimensions, the engagement will stall, waste budget, or produce deliverables nobody uses. This assessment scores Solimed across six dimensions and identifies what needs to be in place before work begins.

**Overall readiness: CONDITIONALLY READY**
Solimed can proceed with Phase 1 provided the gaps identified in this assessment are addressed, primarily around Mladen's availability, coordinator access for testing, and budget authorization.

---

## Dimension 1 — Organizational Readiness

*Can the organization support an active engagement?*

| Factor | Status | Score | Notes |
|---|---|---|---|
| Identified executive sponsor | **Ready** | ✅ | Ivan Kruljac — clear business owner with decision authority |
| CFO/financial sponsor | **Ready** | ✅ | Drew Domescik — actively engaged, validated the platform |
| Day-to-day Pivot contact available | **Partial** | ⚠️ | Ivan is engaged but availability for weekly touchpoints not yet confirmed |
| Internal bandwidth for sprint reviews | **Unknown** | ⚠️ | Ivan and Drew need to confirm bi-weekly 1-hour availability |
| End users (coordinators) available for testing | **Unknown** | ⚠️ | Not confirmed — need 2–3 coordinators for Phase 1 UAT |
| Finance team available for billing UAT | **Unknown** | ⚠️ | Finance role not identified by name in the demo |
| Decision-making speed | **Partial** | ⚠️ | Ivan appears decisive but no formal approval process defined |
| Change management champion | **Partial** | ⚠️ | Ivan is careful about pacing features — good instinct, but no formal change champion identified |

**Score: 4/8 Ready**

**Gaps to close before Sprint 1:**
- Confirm Ivan's weekly availability for Pivot touchpoints
- Confirm Drew's availability for bi-weekly sprint reviews and financial UAT
- Identify 2–3 coordinator names and confirm their availability for Sprint 5–6 UAT
- Identify the finance role owner responsible for investigator payment workflow

---

## Dimension 2 — Technical Readiness

*Does the team have what's needed for Pivot to do the work?*

| Factor | Status | Score | Notes |
|---|---|---|---|
| Current app developer available | **Partial** | ⚠️ | Mladen confirmed as the developer but availability for 2 days/week Sprint 1–2 not yet formally committed |
| Access to Power Apps environment | **Unknown** | ⚠️ | Pivot will need admin access to the Power Apps environment for Sprint 1 discovery |
| Access to Power BI data model | **Unknown** | ⚠️ | Pivot BI engineer needs access to the Power BI Desktop files and dataset |
| Dataverse / data source access | **Unknown** | ⚠️ | Pivot needs read access to the underlying data tables for schema documentation |
| Azure subscription exists | **Unknown** | ⚠️ | Power Apps is Microsoft-hosted but it's unclear if Solimed has an Azure subscription for new infrastructure |
| Microsoft 365 / Azure AD tenant | **Ready** | ✅ | Implied by Power Apps + Power BI usage — already in Microsoft ecosystem |
| Development environment (staging) | **Not Ready** | ❌ | No staging environment confirmed — all changes go to production |
| Code / version history accessible | **Not Ready** | ❌ | Power Apps has version history but it is not git-based; no export of logic exists |
| Data documentation exists | **Not Ready** | ❌ | No data model ERD, no business logic documentation — all in Mladen's head |
| Known data quality issues | **Unknown** | ⚠️ | No data audit has been done; test studies are mixed in with real data (noted in demo) |

**Score: 1/10 Ready**

This is the highest-risk dimension. Almost everything is unknown or not ready. This is normal for an engagement beginning with a low-code system built by a single developer — but it means Sprint 1 must be entirely focused on access, documentation, and discovery before any development begins.

**Gaps to close before Sprint 1:**
- Formally commit Mladen to 2 days/week for Sprints 1–2; get written confirmation from Ivan
- Solimed provisions Pivot with Power Apps read/admin access
- Solimed provisions Pivot with Power BI workspace access
- Solimed provisions Pivot with Dataverse read access (or exports)
- Confirm Azure subscription status — if none exists, provision before Phase 2
- Create Power Apps staging environment (this is the first deliverable of Sprint 1)
- Data audit scope agreed — what test data needs to be separated from real data?

---

## Dimension 3 — Commercial Readiness

*Is Solimed ready to commit commercially?*

| Factor | Status | Score | Notes |
|---|---|---|---|
| Budget confirmed for Phase 1 | **Unknown** | ⚠️ | Proposal shared but no budget commitment confirmed |
| Decision maker for contract sign-off | **Ready** | ✅ | Ivan Kruljac has this authority |
| Payment terms acceptable | **Unknown** | ⚠️ | 20% / 30% / 50% payment structure not yet confirmed |
| Legal review capacity | **Unknown** | ⚠️ | SOW/contract will need Solimed legal review — timeline unknown |
| IP ownership understood | **Unknown** | ⚠️ | All code and deliverables belong to Solimed — needs to be in the SOW |
| NDA in place | **Unknown** | ⚠️ | No NDA confirmed — Pivot has seen proprietary financial data, patient workflow data, and CRO relationships |
| Competing engagements / other vendors | **Partial** | ⚠️ | WTE Solutions / Eric Garrison produced a competing assessment — is that engagement active or dormant? |

**Score: 1/7 Ready**

**Gaps to close before Sprint 1:**
- NDA signed immediately — this should have happened before the demo
- Phase 1 budget verbally confirmed by Ivan
- SOW drafted and legal review timeline established
- Clarify the WTE Solutions / Eric Garrison relationship — is their engagement competitive or complementary?

---

## Dimension 4 — Process Readiness

*Are Solimed's internal processes compatible with an agile delivery model?*

| Factor | Status | Score | Notes |
|---|---|---|---|
| Stakeholders understand agile sprints | **Unknown** | ⚠️ | Ivan described an incremental approach — likely compatible — but not confirmed |
| UAT process defined | **Not Ready** | ❌ | No UAT process documented; no acceptance criteria exist for any feature |
| Production deployment approval process | **Not Ready** | ❌ | Currently Mladen deploys directly; no formal approval chain |
| Feedback mechanism for coordinators | **Partial** | ⚠️ | Ivan said he works "constantly with coordinators to improve UX" — informal but exists |
| Backlog prioritization process | **Partial** | ⚠️ | Ivan manages a backlog ("park it in our backlog and push it forward") but it's informal |
| Sprint review attendance commitment | **Unknown** | ⚠️ | Not confirmed |
| Change request process understood | **Not Ready** | ❌ | No formal change request process in place |

**Score: 0/7 Ready** (partially 3/7)

This is expected for a small organization transitioning from informal to structured delivery. The processes don't need to exist before Sprint 1 — but they need to be agreed and documented in Sprint 1.

**Gaps to close during Sprint 1:**
- Agree on sprint review format and attendance expectations
- Agree on UAT process: who tests, what constitutes acceptance, how feedback is submitted
- Agree on production deployment approval: Pivot proposes, Solimed approves in writing
- Document the existing informal backlog process; migrate to Jira/Linear

---

## Dimension 5 — Data Readiness

*Is the data in a state suitable for migration and API integration?*

| Factor | Status | Score | Notes |
|---|---|---|---|
| Single source of truth established | **Partial** | ⚠️ | Power Apps is the operational SOR but Excel time tracking is a separate, disconnected source |
| Data completeness | **Unknown** | ⚠️ | Demo showed test studies mixed with real studies; unknown what else is mixed |
| Data consistency | **Unknown** | ⚠️ | Visit status values, budget values, date formats — consistency not assessed |
| Historical data integrity | **Partial** | ⚠️ | "Effective from" system handles amendments but historical corrections may have been done manually |
| PII / sensitive data handling | **Partial** | ⚠️ | Patients are anonymized by ID — good. But no access log exists for who has queried patient data |
| Data volume | **Known** | ✅ | 321 active patients; hundreds of studies; manageable volume for migration |
| Backup / recovery process | **Unknown** | ❌ | No backup process confirmed for Power Apps/Dataverse |
| Time tracking data in Excel | **Not Ready** | ❌ | Excel-based time tracking is fragile, not concurrent-safe, and migration path is undefined |

**Score: 2/8 Ready**

**Gaps to close in Sprint 1:**
- Data audit: export all Dataverse tables; document record counts, null rates, value distributions
- Identify and flag all test data records — separate from production before migration begins
- Confirm backup schedule for Power Apps environment — implement daily backup before any changes
- Assess Excel time tracking data quality — how far back does it go? how many records?

---

## Dimension 6 — Regulatory & Compliance Readiness

*Is Solimed operating within the regulatory frameworks that apply to them?*

| Factor | Status | Score | Notes |
|---|---|---|---|
| GDPR compliance (current Croatia operations) | **Partial** | ⚠️ | Patient data is anonymized but no data processing agreement, PII audit log, or data retention policy confirmed |
| ICH-GCP compliance for coordinators | **Assumed** | ⚠️ | Coordinators are trained (implied by demo) but no system-level GCP audit trail |
| CRO audit readiness | **Partial** | ⚠️ | Ivan said sponsors can audit — but the app has no immutable audit log |
| Investigator contract compliance | **Partial** | ⚠️ | Fee structures are in the app but no system-enforced contract term tracking |
| Data residency (Croatia) | **Assumed OK** | ✅ | Power Platform is Microsoft-hosted; EU data residency assumed |
| Insurance / ethics approval tracking | **Not Ready** | ❌ | No document management or expiry tracking confirmed |
| Legal entity for expansion countries | **Unknown** | ⚠️ | Solimed will need legal entities in each expansion country — not yet in scope but a planning dependency |

**Score: 1/7 Ready** (partially 4/7)

**Gaps to close before Phase 3 (international expansion):**
- GDPR data processing agreement documented
- Basic PII access log implemented (Phase 2)
- Confirm Power Platform data residency for current Croatian operations
- Identify legal counsel for expansion country regulatory review

---

## Summary Scorecard

| Dimension | Score | Status |
|---|---|---|
| Organizational Readiness | 4/8 | ⚠️ Partial |
| Technical Readiness | 1/10 | ❌ Not Ready |
| Commercial Readiness | 1/7 | ❌ Not Ready |
| Process Readiness | 0–3/7 | ❌ Not Ready |
| Data Readiness | 2/8 | ❌ Not Ready |
| Regulatory & Compliance | 1–4/7 | ⚠️ Partial |

---

## Overall Assessment: CONDITIONALLY READY

**Solimed can begin the engagement provided the following are completed before Sprint 1 starts:**

### Must-Have Before Sprint 1 (Blockers)

| # | Action | Owner | Timeline |
|---|---|---|---|
| 1 | NDA signed | Both parties | This week |
| 2 | Phase 1 budget verbally confirmed | Ivan Kruljac | This week |
| 3 | SOW Phase 1 drafted and sent for review | Pivot PM | Within 5 business days |
| 4 | Mladen's Sprint 1–2 availability formally confirmed | Ivan Kruljac | Before Sprint 1 |
| 5 | Power Apps + Power BI + Dataverse access provisioned for Pivot | Mladen / Ivan | Before Sprint 1 |
| 6 | 2–3 coordinator names confirmed for UAT | Ivan Kruljac | Before Sprint 1 |
| 7 | Clarify WTE Solutions / Eric Garrison engagement status | Ivan Kruljac | Before Sprint 1 |

### Should-Have by End of Sprint 1

| # | Action | Owner | Timeline |
|---|---|---|---|
| 8 | Power Apps staging environment created | Mladen + Pivot | Sprint 1 |
| 9 | Daily backup of Power Apps environment confirmed | Mladen | Sprint 1 |
| 10 | Data audit complete | Pivot + Mladen | Sprint 1 |
| 11 | As-Is process maps documented | Pivot + Mladen | Sprint 1 |
| 12 | Sprint review and UAT process agreed | Both parties | Sprint 1 |
| 13 | Finance role owner identified for billing UAT | Ivan Kruljac | Sprint 1 |
| 14 | Azure subscription confirmed or provisioned | Ivan Kruljac | Sprint 1 |

---

*This assessment is based on information from the January 23, 2026 demo. It should be updated after the initial discovery session with Mladen Geng and after commercial terms are confirmed.*
