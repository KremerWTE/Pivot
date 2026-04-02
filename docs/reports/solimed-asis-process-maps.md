# Solimed — As-Is Process Maps
## Current State Workflow Documentation

**Prepared by:** Pivot
**Date:** 2026-04-02
**Source:** January 23, 2026 demo recording + transcript analysis
**Status:** DRAFT — to be validated with Mladen Geng and site coordinators in Sprint 1
**Note:** These maps reflect what was observed and described in the demo. They are approximations until Sprint 1 knowledge transfer confirms or corrects them.

---

## Overview of Processes Mapped

1. **Study & Site Study Setup** — How a new study gets configured in the system
2. **Patient Enrollment & Protocol Scheduling** — How a patient enters the system and gets a visit schedule
3. **Visit Execution & Status Tracking** — How coordinators log and manage visits day-to-day
4. **Budget Amendment Handling** — How contract changes are processed
5. **Billing & Revenue Workflow** — How a completed visit becomes billed revenue
6. **Investigator Payment Workflow** — How investigators get paid
7. **Coordinator Time Tracking** — How coordinator hours are currently tracked
8. **Reporting & Dashboard Access** — How management and finance access reporting

---

## Process 1 — Study & Site Study Setup

**Who does it:** Site Manager (occasionally any coordinator)
**Trigger:** Solimed signs a contract with a CRO for a new clinical study
**System used:** Power Apps (Study Tracking app)

```
START: CRO contract signed
    │
    ▼
[Site Manager creates Study record]
    - Universal code
    - Study code
    - Study name
    - CRO assignment
    - Start date / Duration
    - Status = Open
    │
    ▼
[Site Manager configures Study Protocol]
    - List of visits (visit name, type, interval in days)
    - Visit type: Regular / Virtual / SCR / EOT / Colonoscopy / Custom
    - Complex flag (Y/N) — if Y, has additional procedures to budget
    - Tolerance in days (e.g., ±7 days)
    - Status: Open
    │
    ▼
[Site Manager creates Site Study]
    (Site Study = Study running at a specific site)
    - Assigns study to: Solimed Clinic OR Medico RI
    - Assigns lead coordinator(s)
    - Assigns investigators (list)
    │
    ▼
[Site Manager configures Investigator Fees per Site Study]
    For each investigator:
    - PI Fee (payment for conducting the visit)
    - PI Cut % (percentage earned simply for being PI)
    - Sub-investigator split %
    - Referral doctor fee (if applicable)
    │
    ▼
[Site Manager configures Visit Budgets per Site Study]
    For each visit in the protocol:
    - Site Budget (internal cost of the visit)
    - Investigator Budget (maximum for investigator procedures)
    - Procedure Budget (external procedure costs, e.g., colonoscopy)
    - Procedure Fee
    - Date Effective (for amendment tracking)
    │
    ▼
[Site Manager configures One-Time Budgets]
    - Archiving Budget + Fee + Start Date + Status
    - Pharmacy Budget + Fee + Start Date + Status
    - Startup Budget + Fee + Start Date + Status
    - Administrative Budget + Fee + Start Date + Status
    │
    ▼
[Site Manager configures Payment Terms]
    - Billing trigger conditions per study
    (currently configured but not fully utilized)
    │
    ▼
END: Site Study is ready. Status = Open.
    Coordinator can now begin adding patients.
```

**Known gaps / pain points:**
- ❌ No working hours flag configured at this stage — fee calculation does not differentiate in-hours vs. after-hours
- ❌ If a study has multiple arms, all arms are configured but there is no patient-level arm assignment mechanism
- ❌ No screen fail allotment field — contracts specify how many screen fails are billable but this is not captured
- ❌ Replicating a study for a second site requires full manual re-entry; modifying site-specific visits on a copy is architecturally limited
- ⚠️ Payment terms are configured but not yet driving automated billing events

---

## Process 2 — Patient Enrollment & Protocol Scheduling

**Who does it:** Lead Coordinator
**Trigger:** A patient is identified as a potential study participant and passes pre-screening
**System used:** Power Apps (Study Tracking app)

```
START: Patient identified for study
    │
    ▼
[Coordinator adds Patient to Site Study]
    - Patient assigned anonymized ID (e.g., 403001 - Patient 667)
    - Patient linked to Site Study
    │
    ▼
[Screening Visit scheduled]
    - Coordinator sets Screening visit date
    - Status = Scheduled
    - Investigator assigned
    │
    ▼
[Screening Visit conducted]
    (See Process 3 — Visit Execution)
    │
    ├── SCREEN FAIL ──────────────────────────────────┐
    │                                                  ▼
    │                                    [Coordinator marks visit = Screen Fail]
    │                                    [All future visits auto-set to Skipped]
    │                                    [Patient record closed]
    │                                    END: Patient exits study
    │
    └── PASS SCREENING ────────────────────────────────┐
                                                        ▼
                                        [Patient proceeds to Randomization Visit]
                                        (Scheduled and conducted per Process 3)
                                                        │
                                         ┌──────────────┘
                                         ▼
                         [RANDOMIZATION VISIT MARKED DONE]
                         ← THIS IS THE KEY EVENT →
                                         │
                                         ▼
                         [System auto-populates ALL future visit dates]
                             Based on: randomization date +
                             interval in days per protocol visit
                             All future visits set to: Scheduled
                                         │
                                         ▼
                         [For multi-arm studies: ⚠️ GAP]
                             All arms' visits are scheduled
                             No mechanism to assign patient to a specific arm
                             Backlog incorrectly includes ALL arms
                                         │
                                         ▼
                    END: Patient has full protocol schedule in the app
                    Coordinator begins managing visits week-by-week
```

**Known gaps / pain points:**
- ❌ Multi-arm studies: all arms scheduled at randomization; no arm assignment; backlog inflated
- ❌ Screen fail allotment: system records the screen fail but does not track it against a billable limit
- ❌ Pre-screening patients: patients in pre-screening are NOT in the app — only post-randomization patients appear in coordinator dashboard (confirmed in demo)
- ⚠️ Enrollment velocity: the system has the data to calculate enrollment rate but no dashboard shows it

---

## Process 3 — Visit Execution & Status Tracking

**Who does it:** Lead Coordinator (primary), Investigator
**Trigger:** A patient visit is approaching (coordinator contacts patient)
**System used:** Power Apps (visit logs view) + Paper (source documentation)

```
START: Visit appearing in coordinator's upcoming visits view
    │
    ▼
[Coordinator contacts patient]
    - Confirms attendance
    - Notes any issues
    │
    ▼
[Coordinator sets Visit Date + Status = Planned]
    - Specific confirmed date entered
    - Status changes from Scheduled → Planned
    - Investigator assigned (if known at this point)
    │
    ├── PATIENT DOES NOT ATTEND ─────────────────────┐
    │                                                  ▼
    │                               [Coordinator assesses: reschedule or skip]
    │                               If within tolerance: reschedule → new date
    │                               If outside tolerance: Status = Skipped
    │                               Return to START or END
    │
    └── VISIT PROCEEDS ────────────────────────────────┐
                                                        ▼
                                    [Doctor (Investigator) conducts visit]
                                        - Works through checklist
                                        - Each checklist item → elaborated on
                                          consultation report (printed x2)
                                        - One copy to patient
                                        - One copy filed in paper patient records
                                                        │
                                                        ▼
                                    [Coordinator updates app after visit]
                                        - Sets Investigator (if not set)
                                        - Sets actual visit date
                                        - Status: Scheduled/Planned → DONE
                                                        │
                                                        ▼
                                    [⚠️ GAP: Working Hours NOT recorded]
                                        App does not capture whether visit
                                        was in-hours or after-hours
                                        Fee calculation cannot differentiate
                                                        │
                                                        ▼
                                    [Optional: Coordinator adds visit note]
                                        Free text note field
                                        No structured data capture
                                                        │
                                                        ▼
                                    [Optional: Complex procedure handling]
                                        If colonoscopy or external procedure:
                                        Extracted as separate visit with own budget
                                        (workaround for no procedure-level tracking)
```

**Known gaps / pain points:**
- ❌ Working hours not captured — critical for investigator fee accuracy
- ❌ No procedure-level itemization — partial visits or unscheduled procedure visits cannot be itemized
- ❌ Custom visit workaround: protocol deviations or unscheduled visits require coordinator to manually create a custom visit with hand-entered budgets
- ⚠️ Visit note is free text only — no structured reason codes for skipped/missed visits
- ⚠️ Monitor visit view (visit-first matrix) — exists but is a separate toggle from patient-first view; both exist but the toggle requires coordinator training

---

## Process 4 — Budget Amendment Handling

**Who does it:** Site Manager or experienced Coordinator
**Trigger:** CRO/sponsor sends a protocol amendment that affects visit budgets or fees
**System used:** Power Apps (Admin Area — Site Study Configuration)

```
START: Amendment received from CRO/sponsor
    │
    ▼
[Site Manager reviews amendment document]
    - Identifies which visits/budgets are affected
    - Identifies the effective date of the amendment
    │
    ▼
[Site Manager opens affected budget(s) in Site Study Config]
    - Navigates to: Admin Area → Site study config → Visit Budgets
    - Opens "Edit visit budget" modal for each affected visit
    │
    ▼
[Site Manager updates budget values]
    - Changes: Site Budget / Investigator Budget / Procedure Budget / Procedure Fee
    - Sets "Date Effective" = the amendment's effective date
      (can be a past date, present date, or future date)
    │
    ▼
[System recalculates affected visits]
    - All visits with a visit date AFTER the effective date use new budget
    - All visits with a visit date BEFORE the effective date use old budget
    - Historical visits are NOT changed retroactively in value
    ← THIS IS THE KEY STRENGTH OF THE CURRENT SYSTEM →
    │
    ▼
[Site Manager saves + verifies]
    - Spot-checks a before and after visit in Power BI to confirm
    │
    ▼
END: Amendment applied. Reporting automatically reflects new rates
     for applicable visits going forward.
```

**Known gaps / pain points:**
- ⚠️ No amendment audit trail — who made the change, when, and why is not logged
- ⚠️ No workflow: there is no approval step before a budget change takes effect in production
- ⚠️ No notification: coordinators and finance are not automatically notified when a budget changes
- ❌ Two-site amendments: if the same protocol runs at both Solimed Clinic and Medico RI, the amendment must be applied separately to each site study — there is no "apply to all sites" option

---

## Process 5 — Billing & Revenue Workflow

**Who does it:** Lead Coordinator (Done → Approved), Finance Role (Approved → Billed)
**Trigger:** Visit is marked Done by coordinator
**System used:** Power Apps + manual finance review + external invoicing

```
START: Visit status = DONE
    │
    ▼
[Monitor/CRA reviews the visit]
    (External — CRO's Clinical Research Associate visits the site
     and reviews source documentation for the visit)
    This happens periodically, not in real-time
    │
    ├── MONITOR DOES NOT APPROVE ─────────────────────┐
    │                                                   ▼
    │                              [Coordinator holds visit at DONE status]
    │                              [Issue documented outside the app (email)]
    │                              [Revisit when resolved]
    │
    └── MONITOR APPROVES ──────────────────────────────┐
                                                        ▼
                                [Coordinator updates visit status: DONE → APPROVED]
                                Meaning: this visit is cleared for billing
                                                        │
                                                        ▼
                                [⚠️ GAP: No automated alert to finance]
                                Finance must manually check for new APPROVED visits
                                No notification triggers currently
                                                        │
                                                        ▼
                                [Finance Role reviews approved visits in Power BI]
                                Checks: visit details, investigator, patient,
                                        budget, any special circumstances
                                                        │
                                                        ▼
                                [Finance generates invoice to CRO/sponsor]
                                Currently: manual invoice creation outside the app
                                                        │
                                                        ▼
                                [Finance updates visit status: APPROVED → BILLED]
                                                        │
                                                        ▼
                                [⚠️ GAP: Payment receipt not tracked in app]
                                No field for "payment received date" or amount
                                Cash flow cannot be tracked in the system
                                                        │
                                                        ▼
END: Visit is Billed. Appears in P&L as revenue.
```

**Known gaps / pain points:**
- ❌ No automated notification when visit moves to Approved — finance must manually check
- ❌ Invoice generated outside the app — no invoice record in the system
- ❌ Payment receipt not tracked — no cash flow data in the system
- ❌ Screen fail allotment not checked — over-allotment screen fails may be invoiced incorrectly
- ❌ After-hours fee not differentiated — all approved visits use the same fee rate regardless of hours worked
- ⚠️ One-time budgets (startup, archiving) tracked manually with status flags — no automated billing trigger
- ⚠️ Monitor approval is an external process — no system record of CRA review/approval

---

## Process 6 — Investigator Payment Workflow

**Who does it:** Finance Role
**Trigger:** End of month
**System used:** Power BI (Investigator Specification dashboard) + Email (manual)
**Frequency:** Monthly specifications; actual payment 3× per year (every 4 months)

```
START: Month end
    │
    ▼
[Finance opens Investigator Specification dashboard in Power BI]
    - Selects: Site + Year + Month
    - Dashboard shows: each investigator, their visits, patients,
      fees earned (Inv Fee, PI Cut, Ref Fee, Total)
    │
    ▼
[Finance exports dashboard to Excel/PDF]
    - One export per investigator (or filtered by investigator)
    │
    ▼
[Finance manually emails specification to each investigator]
    - Attaches their monthly breakdown
    - Currently: fully manual process
    - Ivan confirmed: ready to automate after 2–3 months of validated numbers
    │
    ▼
[Investigator receives and reviews specification]
    - Cross-checks against their own records
    - If correct: no action required
    - If discrepancy: investigator emails back with the error
    │
    ├── DISCREPANCY FOUND ─────────────────────────────┐
    │                                                   ▼
    │                              [Finance investigates in app]
    │                              [Coordinator missed entering a visit, OR]
    │                              [Wrong investigator assigned to visit, OR]
    │                              [Budget configuration error]
    │                              [Correction made in app]
    │                              [Revised spec resent]
    │
    └── NO DISCREPANCY ────────────────────────────────┐
                                                        ▼
                            [Specification filed for payment run]
                                                        │
                                                        ▼
                            [Every 4 months: payment run executed]
                            Finance tallies 4 months of approved specs
                            Payment made to each investigator
                                                        │
                                                        ▼
END: Investigator paid. 4-month cycle resets.
```

**Known gaps / pain points:**
- ❌ Export + email is fully manual — ready to automate (Ivan confirmed)
- ❌ After-hours visits use the wrong rate — fee in specification may be incorrect until working hours flag is implemented
- ❌ PI cut vs. PI fee is calculated correctly but not clearly labeled in current specification — investigators may be confused about the two components
- ⚠️ Discrepancy resolution happens outside the app (by email) — no system record of disputes and corrections
- ⚠️ Payment runs happen 3× per year but investigators receive monthly specs — cash flow timing mismatch can cause confusion

---

## Process 7 — Coordinator Time Tracking

**Who does it:** Each coordinator (self-reporting)
**Trigger:** End of day or end of week (coordinator's choice)
**System used:** Excel spreadsheet (shared) → Power BI (via Power Query connection)

```
START: Coordinator completes work activities
    │
    ▼
[Coordinator opens Excel time tracking spreadsheet]
    (At end of day OR accumulated at end of week)
    │
    ▼
[Coordinator logs time entries]
    Each entry includes:
    - Coordinator name
    - Study / Project
    - Task category (from list):
        • Vizita (patient visit)
        • Unos podataka u EDC (data entry to EDC/systems)
        • Komunikacija s CRA (communication with CRAs)
        • Administracija (administration)
        • Komunikacija s pacijentima (patient communication)
        • Priprema dokumentacije (document preparation)
        • Kontrola/organizacija (coordination activities)
        [other categories as defined]
    - Hours
    │
    ▼
[Excel file saved]
    - No validation on the data entry
    - No concurrent access protection (if two coordinators edit simultaneously: conflict)
    - No audit trail on the Excel file
    │
    ▼
[Power BI refreshes on schedule]
    - Power Query reads the Excel file
    - Utilization dashboard updates
    - Shows: hours by coordinator, by study, by task category, by month
    │
    ▼
[Management reviews utilization dashboard in Power BI]
    - Coordinator hours per visit
    - Visit vs. admin time split
    - Study-level utilization
    - Hours per visit trend over time (maturity curve)

END: Time tracked in dashboard. No normative comparison currently possible.
```

**Known gaps / pain points:**
- ❌ Excel is fragile as a data source — format changes break Power BI connection
- ❌ No concurrent access protection — data conflicts if multiple users edit simultaneously
- ❌ Self-reported and retrospective — not real-time; accuracy depends on coordinator diligence
- ❌ No per-visit time normative — can't compare actual hours to expected hours
- ❌ Not integrated into the app — time entries are not linked to specific visit records
- ⚠️ Ivan said moving this into the app would be "too much" for coordinators right now — transition must be gradual
- ⚠️ Task categories appear to be in Croatian — will need to be maintained in local language(s) at expansion

---

## Process 8 — Reporting & Dashboard Access

**Who does it:** Finance (P&L, Investigator Specs), Site Managers (operational), Ivan/Drew (strategic), Medico RI management (their P&L)
**Trigger:** On-demand or scheduled review
**System used:** Power BI (browser or Power BI app)

```
Current Dashboards Available:

[P&L Dashboard]
    Access: Solimed finance + leadership
    Shows: Revenue by CRO/study, fixed vs. visit revenue, costs,
           margin ratios, patient travel, SCR/RAN visit counts
    Filter: Date range, study/CRO
    Gap: No site filter, no screen fail revenue split, no cash flow

[Investigator Specification Dashboard]
    Access: Finance role
    Shows: Monthly fee breakdown per investigator per visit/patient
    Gap: Manual export/send, no after-hours rate differentiation

[Coordinator Utilization Dashboard]
    Access: Site managers, Ivan
    Shows: Hours tracked per coordinator, visit vs. admin split,
           hours per visit, study-level breakdown
    Data source: Excel (fragile)
    Gap: No normative comparison, no demand forecast

[Backlog / Budget Forecast]
    Access: Ivan, Drew, leadership
    Shows: Cumulative visit budget by year (2022–2032)
    Gap: No site filter, multi-arm inflation, no patient drill-down

[Calendar View]
    Access: Coordinators
    Shows: Scheduled visits in calendar format per coordinator
    Gap: No site filter, no tolerance color coding
    Note: Built in Power BI, not in Power Apps app — a pragmatic workaround

[Medico RI Dedicated P&L]
    Access: Medico RI management team (shared live)
    Shows: Medico-specific revenue, costs, margins
    Note: Hospital management uses this for day-to-day study performance monitoring
    Gap: Real-time data but end-of-month controlling review recommended before showing
```

**Known gaps / pain points:**
- ❌ No revenue recognition split (billable vs. at-risk vs. written-off)
- ❌ No cash flow forecast (13-week or 12-month)
- ❌ No enrollment velocity tracking
- ❌ No backlog quality stratification
- ❌ No CRO performance / payment terms tracking
- ⚠️ Power BI Pro licenses required per user — cost will scale with expansion
- ⚠️ Dashboard refresh is scheduled, not real-time — Medico RI sees data with a lag

---

## Process Gap Summary

The following table consolidates all identified gaps across all processes:

| Gap | Process | Priority | Phase to Fix |
|---|---|---|---|
| Working hours flag missing | Visit Execution (P3) | Critical | Phase 1 Sprint 2 |
| Multi-arm arm assignment missing | Enrollment (P2) | Critical | Phase 1 Sprint 2 |
| Screen fail allotment not tracked | Enrollment (P2), Billing (P5) | High | Phase 1 Sprint 3 |
| Automated investigator spec sending | Investigator Payment (P6) | High | Phase 1 Sprint 4 |
| No invoice record in system | Billing (P5) | High | Phase 1 / Phase 4 |
| No payment receipt tracking | Billing (P5) | High | Phase 1 / Phase 4 |
| No audit trail for budget changes | Amendment (P4) | High | Phase 1 Sprint 5 |
| No finance notification when visit Approved | Billing (P5) | Medium | Phase 1 Sprint 4 |
| Time tracking in Excel (fragile) | Time Tracking (P7) | Medium | Phase 2 |
| No site filter on backlog dashboard | Reporting (P8) | Medium | Phase 1 Sprint 6 |
| No patient-level backlog drill-down | Reporting (P8) | Medium | Phase 1 Sprint 6 |
| No normita / time baseline per visit | Time Tracking (P7) | Medium | Phase 1 Sprint 6 |
| No cash flow forecast | Reporting (P8) | Medium | Phase 1 Sprint 7–8 |
| No procedure-level visit itemization | Visit Execution (P3) | Medium | Phase 2 |
| Amendment notification missing | Amendment (P4) | Medium | Phase 2 |
| Single-site amendment application | Amendment (P4) | Medium | Phase 2 |
| No enrollment velocity tracking | Reporting (P8) | Medium | Phase 1 Sprint 9–10 |
| Excel concurrent access conflict | Time Tracking (P7) | Medium | Phase 2 |
| No monitor approval record in app | Billing (P5) | Low | Phase 2 |
| Discrepancy resolution outside app | Investigator Payment (P6) | Low | Phase 2 |
| Power BI Pro cost at scale | Reporting (P8) | Low | Phase 4 (Embedded) |

---

## Validation Required (Sprint 1)

These process maps are based on the January 23, 2026 demo. The following must be validated with Mladen Geng and at least one coordinator before development begins in Sprint 2:

- [ ] Confirm study setup process — are there configuration steps not shown in the demo?
- [ ] Confirm investigator fee calculation logic — all edge cases (PI only, sub-I only, PI + sub-I, with referral)
- [ ] Confirm amendment handling — are there cases where the "effective from" system breaks down?
- [ ] Confirm billing workflow — who specifically is the "finance role"? What system do they use for invoicing?
- [ ] Confirm time tracking — how many coordinators are tracking time? What are all the task categories?
- [ ] Confirm reporting access — who has Power BI Pro licenses? How is access provisioned for Medico RI?
- [ ] Confirm the ClickUp reference — the WTE Solutions draft mentioned ClickUp for recruitment; is this in use anywhere?

---

*These As-Is process maps are living documents. They will be updated during Sprint 1 knowledge transfer and should be considered baseline documentation for the API design in Phase 2.*
