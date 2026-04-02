# Solimed CTO Assessment — Draft Gap Analysis
## Additions & Changes Required Based on Video Demo + Transcript

**Document purpose:** This compares Eric Garrison's draft CTO assessment (April 2, 2026 — written pre-video) against the confirmed findings from the January 23, 2026 demo recording (visual analysis + Whisper transcription). Each item is classified as a **CORRECTION** (draft is factually wrong), **ADDITION** (important context the draft is missing), or **CONFIRM** (draft is right — keep as-is).

**Prepared by:** Pivot  
**Date:** 2026-04-02  
**Reference documents:**
- `Solmed_CTO_Assessment_DRAFT.docx` — Eric Garrison, WTE Solutions
- `solimed-cto-assessment.md` — Pivot analysis based on demo transcript

---

## SECTION 1 — CORRECTIONS (Draft is factually wrong — must change)

---

### CORRECTION 1 — Section 1.1: Severity of "lightweight tracking tool" characterization

**Draft says:**
> "The organization does not currently operate a clinical platform. What exists is a lightweight tracking tool built on PowerApps and Dataverse, surrounded by manual processes."
> Maturity table rates them at: Stage 0 → Stage 1

**What the transcript reveals:**
This significantly undersells the platform and will alienate the client. The demo shows a functioning, domain-complete operational system that is actively used daily by coordinators managing:
- **321 active enrolled patients** across hundreds of studies
- **15+ CROs** across multiple study types
- **€2.1M in tracked, structured financial backlog** (studies through 2032)
- A sophisticated investigator fee model (PI cut, PI fee, sub-investigator splits, referral doctor fees — all configured per study)
- An "effective from" date system for budget amendments that correctly recalculates historical visits
- Auto-scheduling of all protocol visits the moment a randomization visit is marked done
- Auto-skip of future visits when a patient screen fails
- Separate real-time Power BI PNL dashboards for each site, one of which is shared live with Medico RI hospital management

Ivan built this incrementally over 3+ years. Calling it a "lightweight tracking tool" will read as dismissive and damage the relationship before the engagement begins.

**Recommended change:**
Replace the "lightweight tracking tool" framing with a more accurate characterization:

> "Solimed operates a purpose-built, domain-specific SMO management platform built on Microsoft Power Apps and Power BI. The platform is functionally complete for its current operational scope — it manages study lifecycle, patient visit tracking, investigator fee distribution, budget management, and financial reporting for 321 active patients across 15+ CROs. It was built deliberately over 3+ years, starting from Excel and evolving to Power Apps — a pragmatic approach that delivered real operational value. The platform is not broken. It is approaching the ceiling of what its current architecture can support."

**Maturity table correction:**

| Area | Draft Rating | Corrected Rating | Notes |
|---|---|---|---|
| Data Capture | Manual / Paper — HIGH RISK | **Structured digital — MEDIUM** | Power Apps IS the primary data capture. Paper is source docs only — deliberate CRO-approved decision |
| System of Record | None — HIGH RISK | **Exists but single-tenant — MEDIUM** | Power Apps/Dataverse IS the system of record for the financial/operational layer |
| Audit / Compliance | Critical Gap — HIGH | **Partial gap — MEDIUM-HIGH** | No PII audit log is correct. But billing audit trail exists (visit status history). Severity is lower than stated. |
| Scheduling | Spreadsheet-Based — HIGH | **App-managed — LOW-MEDIUM** | Visit scheduling is in the app. The spreadsheet is TIME TRACKING only. |
| Recruitment | Ad Hoc / ClickUp — HIGH | **Not applicable to Solimed's model** | See Correction 3 below |
| Billing Integrity | Weak Controls — MEDIUM | **Functional with known gaps — MEDIUM** | Billing workflow (Done → Approved → Billed) exists. Main gap is the after-hours fee calculation. |
| Architecture | Low-Code Prototype — MEDIUM | **Production system at scale ceiling — MEDIUM** | This is not a prototype. It is in production managing real studies and real money. |

---

### CORRECTION 2 — Section 1.2 Risk 1: Paper Forms as Primary Data Capture

**Draft says:**
> "Paper forms serve as the primary data capture mechanism"

**What the transcript reveals:**
This is factually wrong. Paper is used **only for source documentation** (patient consultation reports) — and this is a deliberate architectural decision made in consultation with CROs. Ivan explicitly stated:

> "We were also discussing this with the CROs and their input was not to complicate it with the processes too much because such platform, which contains source documents, this is something that requires far more approvals, regulations and stuff like that."

The **primary data capture is Power Apps**. Coordinators log visits, investigator assignments, dates, statuses, and budget details in the app. Paper exists only because CROs advised against digitalizing source documents at this stage.

**Recommended change:**
Remove the bullet "Paper forms serve as the primary data capture mechanism" from Risk 2. Replace Risk 2's opening with:

> "Solimed's Power Apps platform is the system of record for operational and financial data. Source documentation (patient consultation reports) is maintained on paper by deliberate choice — CROs advised against digitalization of source documents due to the additional regulatory approvals required. This is not a gap; it is a considered decision. The actual system of record gap is the absence of a proper API layer and multi-tenant data model, not the paper source docs."

---

### CORRECTION 3 — Section 2.2 Layer 4 & 5: Patient Engagement and Recruitment Pipeline

**Draft says:**
> "Layer 4: Patient Engagement — SMS and email automation via Twilio, automated reminders"
> "Layer 5: Recruitment Pipeline — digital intake forms, CRM-style tracking for prospective participants"

**What the transcript reveals:**
Solimed is an **SMO (Site Management Organization)**, not a patient recruitment company. Patient engagement and recruitment are managed by the CROs and sponsors — not by Solimed. Their platform explicitly does NOT touch the patient-facing layer. Ivan confirmed:

> "All of the data that's filled in the EDCs is not connected to the app that we currently use. So we do not gather the same data."

Drew (the CFO advisor with 20 years in site networks) did not raise patient engagement or recruitment as a need at any point in the 85-minute demo.

Building a patient engagement layer and recruitment CRM would:
1. Duplicate capabilities already owned by the CRO/sponsor
2. Require Solimed to take on regulatory responsibility for patient-facing systems they currently don't hold
3. Be unwanted — nobody asked for it in the demo

**Recommended change:**
Remove Layers 4 and 5 from the target architecture. Replace with:

> "Patient engagement and recruitment are the domain of CROs and sponsors. Solimed's platform sits at the SMO operational layer — between the sponsor/CRO and the clinical site. The correct integration investment is in the CRO-facing direction (EDC sync, automated invoicing, CRO portal access) not the patient-facing direction."

---

### CORRECTION 4 — Section 2.2 Layer 2: EDC as a Replacement

**Draft says:**
> "Eliminate paper-based data capture as the first operational priority. Options include: Lightweight EDC platforms, custom web/mobile forms"

**What the transcript reveals:**
Solimed explicitly does **not** want to be in the EDC business. They work alongside CRO-owned EDC systems. Ivan:

> "All of the data that's filled in the EDCs is not connected to the app that we currently use. So we do not gather the same data."

The correct EDC recommendation is **integration with CRO-owned EDC systems** (bidirectional sync of visit status), not building or procuring their own EDC.

**Recommended change:**
Replace Layer 2 with:

> "Layer 2: EDC Integration (not replacement). Solimed does not operate its own EDC and has no intention of doing so — clinical data capture is the CRO's responsibility. The correct investment is a bidirectional sync adapter between Solimed's platform and the EDC systems used by their CRO partners (Medidata Rave, Veeva Vault EDC, REDCap). This eliminates the manual double-entry burden without requiring Solimed to take on regulatory responsibility for clinical data capture."

---

### CORRECTION 5 — Section 5: Actions to Avoid

**Draft says:**
> "Do not attempt to extend PowerApps into a clinical platform"

**What the transcript reveals:**
This framing is correct in spirit but will land wrong with the client. The existing Power Apps system IS functioning as their clinical operations platform and Mladen built it well for the current scale. Telling them "don't extend Power Apps" without acknowledging that what they have works risks sounding dismissive.

**Recommended change:**
Soften to:

> "Do not continue to add complex business logic into Power Apps formulas — fee calculations, normative time tracking, and multi-arm backlog logic should be migrated to a tested, version-controlled API layer. Power Apps can remain as the coordinator UI during transition but should not be the home for business-critical calculations."

---

## SECTION 2 — ADDITIONS (Missing from draft — must add)

---

### ADDITION 1 — Why Solimed built their own (critical context)

The draft treats the Power Apps system as a symptom of immaturity. The transcript reveals it is the result of a deliberate, informed strategic choice. This context must be added early in Section 1 to frame the entire assessment correctly.

**Add to Section 1.1:**

> **Strategic context:** Solimed evaluated existing CTMS vendors and chose to build their own. Ivan's reasoning (from the Jan 23 demo): "Our model is a bit specific. Our needs are also a bit specific... it would be mostly too bloated. We wouldn't be using all of the functionalities." This was not a naive decision — it was made by a team that understood the alternatives. Any recommendation to evaluate off-the-shelf CTMS platforms must account for the fact that this was already considered and rejected, and must clearly articulate what is now different that would change that conclusion.

---

### ADDITION 2 — Power BI as a Strength (Section 1 — missing entirely)

The draft does not mention Power BI at all. Power BI is the strongest component of the current stack and a significant asset.

**Add as new Section 1.4 — Reporting Strengths:**

> The Power BI reporting layer is sophisticated and genuinely valuable. Current dashboards include:
> - **P&L by CRO/study**: Revenue, costs, margin ratios, fixed vs. visit revenue
> - **Investigator Specification**: Monthly fee breakdown per investigator — basis for payment runs
> - **Coordinator Utilization**: Hours tracked per coordinator across visit types and admin activities, connected via Excel (fragile but functional)
> - **Backlog/Forecast**: Multi-year scheduled visit revenue forecast (2022–2032)
> - **Calendar View**: Coordinator visit calendar built entirely in Power BI without app development
> - **Site-specific dashboards**: Separate PNL views for Solmed vs. Medico RI — Medico management has live real-time access to their financial performance
>
> Power BI should be retained and invested in — it is best-in-class for this use case and deeply integrated into the Microsoft ecosystem Solimed already operates in. The primary gaps are: the Excel time tracking data source (fragile), the absence of a semantic layer causing inconsistent metric definitions (e.g., the multi-arm backlog inflation), and the lack of row-level security for multi-country expansion.

---

### ADDITION 3 — Specific Confirmed Pain Points (Section 1 — missing)

The draft identifies risk categories but not the specific, named pain points that Solimed's team described. These are the items that will appear in the Statement of Work and must be in any credible assessment.

**Add as new Section 1.5 — Confirmed Operational Pain Points (from Jan 23, 2026 demo):**

> The following gaps were explicitly identified by Solimed's team during the demo. They represent the highest-priority work items:
>
> 1. **Working hours flag on visits** *(in progress for Feb 2026)*: Visits performed after regular working hours carry different investigator fee rates. The app currently cannot distinguish in-hours from after-hours visits, meaning investigator fees are potentially calculated incorrectly for a subset of visits.
>
> 2. **Multi-arm study backlog inflation**: For studies with multiple protocol arms, the app currently sums all possible arms per patient rather than the patient's assigned arm. This inflates the backlog and makes the €2.1M forecast unreliable for branching protocols. Ivan: "The app will sum all possibilities. We'll have a study with a huge overall budget because there are several arms."
>
> 3. **Screen fail allotment not tracked**: Contracts specify how many screen fails Solimed can bill for. When screen fails exceed the allotment, the overage cannot be billed. This is not tracked in the app — a revenue recognition risk, especially in gastroenterology studies with high screen fail rates. Drew (CFO background): "You have to be careful about the screening revenue that you're recognizing."
>
> 4. **Automated investigator specification sending**: Finance currently exports monthly payment specifications manually and emails each investigator. Ivan confirmed the numbers have been accurate for 2–3 months and the team is ready to automate. Each investigator's contact details are already in the app.
>
> 5. **Time normatives (normita) missing**: No per-visit time expectation exists in the app. Ivan's proposed approach: derive expected duration from the visit budget (investigator + procedure budget ÷ a configured €/hour rate). Drew validated this matches his bottom-up budgeting methodology.
>
> 6. **Procedure-level visit itemization missing**: Unscheduled visits or partial visits cannot currently be itemized by procedure. Workaround: extract procedures as separate visits. Drew: "If you don't drill down and show exactly the detail, it's hard on the finance side to figure out what we can actually bill for."
>
> 7. **Site filter missing on backlog dashboard**: Ivan noted live during the demo that the backlog dashboard has no site filter and said "I'm going to do that after the call." This is a known, simple gap.
>
> 8. **Patient-level backlog drill-down missing**: The backlog shows study-level revenue only. Drew asked for patient-level view — "Is there a view where you can look at the patient level versus the study level backlog?" Currently not available.

---

### ADDITION 4 — Investigator Fee Model Complexity (Section 1 — missing)

The draft mentions "billing integrity" as a risk but does not describe the actual fee structure. This is critical context for any migration or API design.

**Add to Section 1.2 Risk 5 (Billing Integrity):**

> Solimed's investigator fee model is more complex than typical clinical site billing and must be fully documented before any migration. The current model handles:
> - **PI Fee**: Payment for the principal investigator conducting the visit
> - **PI Cut**: Separate percentage the PI receives simply for being the PI on a study, regardless of who conducts the visit
> - **Sub-investigator split**: When a sub-investigator conducts the visit, the PI cut and PI fee are redistributed between PI and sub-I according to agreed percentages
> - **Referral doctor fee**: When a patient was referred by an external doctor, a referral fee is tracked and paid separately
> - **After-hours rate differential**: Visits conducted outside regular working hours carry a different (higher) investigator rate — currently not captured in the app (in-progress fix)
> - **Payment terms per site study**: Each site study has configurable payment terms that determine when billing events are triggered
>
> Any API layer or data migration must preserve this exact fee logic. It represents years of negotiated contractual terms and investigator relationships. Fee calculation errors would surface immediately — investigators reconcile their monthly specifications and flag discrepancies.

---

### ADDITION 5 — Amendment Handling (existing capability — not mentioned)

**Add to Section 1 (as a strength, not a gap):**

> Solimed has implemented a notable "effective from" date system for budget amendments. When a CRO or sponsor amends a contract mid-study, coordinators can enter the new budget values with a future or retroactive effective date. The system recalculates all visit fees based on the visit date relative to the effective date — not based on when the change was entered. This correctly handles the common scenario of receiving an amendment that applies to past visits. This logic must be preserved exactly in any migration.

---

### ADDITION 6 — The Strategic Question Answer (Section 6 — incomplete)

The draft's key decision point asks: *"Are we building toward a scalable CRO platform or optimizing internal operations?"*

The transcript answers this directly. Add the answer:

**Add after Section 6's question:**

> **The answer from the Jan 23, 2026 demo:** Both — and in that order. The immediate priority is operational optimization (the 8 confirmed pain points above). The medium-term ambition is platform scale — Ivan and Drew explicitly discussed charging CROs for access to the platform. Drew: "Do you guys charge for the app?" Ivan: "Currently no." Drew: "That needs to change."
>
> This confirms the target architecture must support both internal optimization and eventual SaaS productization. The phased approach recommended here is correct — but Phase 4 should explicitly include the commercial model transition from internal tool to licensed platform.

---

### ADDITION 7 — Country Selection Framework (Section 3 Phase 4 — missing)

The draft mentions geographic expansion in Risk 6 but provides no framework for which country to enter first or how to assess readiness.

**Add to Phase 4 (Scale and Differentiate):**

> **Geographic expansion framework:** Not all countries carry equal complexity. Before committing to a target country, assess against four dimensions: regulatory environment, data residency requirements, currency, and language. Recommended first expansion target is a **Tier 1 country** — an EU member state where GDPR compliance is already addressed by the base platform, the Euro is the currency (no FX integration required at launch), and the regulatory environment is familiar. Slovenia or Austria are natural first candidates given Solimed's existing Croatian base. Avoid the US (HIPAA) and China (data sovereignty) until the platform has proven its multi-country model in at least 2–3 Tier 1 markets.

---

### ADDITION 8 — Retain Power BI Explicitly (Section 2 — missing)

The draft's target architecture makes no mention of Power BI. Given that it is Solimed's strongest current asset, this is a significant omission that should be explicitly addressed.

**Add to Section 2.1 as a named component:**

> **Reporting: Power BI (retain and invest)** — Power BI is the strongest component of Solimed's current stack. It should not be replaced. The recommended path is Power BI Embedded, which eliminates per-user licensing costs at scale while preserving all existing dashboard investment. The data model feeding Power BI should be migrated from its current Power Apps/Dataverse connection to the new API layer's Azure SQL database — this improves data freshness, enables real-time dashboard updates, and allows row-level security to be enforced per tenant/country.

---

### ADDITION 9 — Key Person Risk (missing from all risk sections)

**Add to Section 1.2 as Risk 7:**

> **Risk 7: Single Developer Dependency**
> Mladen Geng is the sole developer and architect of the platform, holding all institutional knowledge of the data model, business logic, fee calculation rules, and operational workarounds. There is no documentation, no code repository in the traditional sense (Power Apps is not git-versioned), and no second developer who could maintain or evolve the system if Mladen were unavailable.
>
> For a financial system managing €900K+ in active study budgets, this is a critical operational risk. Immediate mitigation: a structured knowledge transfer and documentation sprint must be the first activity of any engagement — before any new development begins.

---

## SECTION 3 — CONFIRMATIONS (Draft got these right — keep as-is)

| Item | Draft Section | Why It's Correct |
|---|---|---|
| Audit / compliance gap | Risk 1 | Correct — no PII audit logging; real GDPR exposure for expansion |
| No formal API layer | Layer 8 | Correct — Power Automate is not sufficient; structured API needed |
| Azure SQL as database target | Layer 1 | Correct — right choice; already in Microsoft ecosystem |
| Power Apps as transitional UI | Phase 1 + Section 5 | Correct — don't rip it out; keep as UI while API layer is built |
| Geographic expansion risk | Risk 6 | Correct — multi-country requires RBAC, data residency, compliance tooling |
| Event-driven billing triggers | Layer 6 | Correct — visit completion should auto-generate billable events |
| Avoid doubling down on Dataverse | Section 5 | Correct — business logic must move to API, not stay in formulas |
| Azure Functions / Service Bus | Layer 8 | Correct — right tools for async event processing |
| "Progressive hardening" approach | Section 2.2 | Correct — matches Solimed's stated preference for incremental improvement |
| Strategic question framing | Section 6 | Correct question — answer is now known from the transcript (see Addition 6) |

---

## SECTION 4 — SUMMARY OF CHANGES

### High Priority (change before sharing with Solimed)
1. **CORRECTION 1** — Reframe the maturity assessment; remove "lightweight tracking tool" language
2. **CORRECTION 2** — Remove "paper as primary data capture"; clarify source docs are deliberate
3. **CORRECTION 3** — Remove patient engagement / recruitment layer recommendations
4. **CORRECTION 4** — Reframe EDC from "replace paper" to "integrate with CRO-owned EDC"
5. **ADDITION 1** — Add strategic context for why they built their own
6. **ADDITION 3** — Add all 8 confirmed pain points from the demo
7. **ADDITION 9** — Add Mladen key person risk

### Medium Priority (add before final version)
8. **ADDITION 2** — Add Power BI as a strength (Section 1.4)
9. **ADDITION 4** — Add investigator fee model complexity
10. **ADDITION 5** — Add amendment "effective from" system as a strength
11. **ADDITION 6** — Add the answer to the strategic question from the transcript
12. **ADDITION 8** — Explicitly retain Power BI in target architecture
13. **CORRECTION 5** — Soften the "don't extend Power Apps" language

### Lower Priority (add for completeness)
14. **ADDITION 7** — Country selection framework / Tier 1 recommendation
15. Maturity table correction (Correction 1 sub-table)

---

*Gap analysis based on: Solmed_CTO_Assessment_DRAFT.docx (Eric Garrison, WTE Solutions) vs. Pivot analysis of Jan 23, 2026 demo recording (visual + Whisper transcript).*
