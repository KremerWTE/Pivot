# Solimed — CFO Revenue Intelligence Capabilities
## Predictive Revenue Tracking & Financial Visibility Plan

**Prepared for:** Drew Domescik (CFO Advisor) + Ivan Kruljac
**Prepared by:** Pivot
**Date:** 2026-04-02
**Context:** This document addresses the revenue visibility and predictive forecasting gap that is the most common missing capability in clinical site networks at Solimed's stage of maturity. It defines what "complete" revenue intelligence looks like for an SMO and maps each capability to the build plan.

---

## The Gap — What's Missing Today

Solimed has built solid operational tracking and has a good foundation in Power BI. But there is a clear distinction between **tracking what happened** and **seeing what's coming**. Right now Solimed can answer backward-looking questions reasonably well. The forward-looking picture — the CFO view — is incomplete.

| Question | Can Solimed Answer It Today? | Gap |
|---|---|---|
| How much revenue did we earn last month? | Partially — visit logs exist but billing status is manual | Billing workflow not fully automated |
| How much revenue will we earn next month? | No | No rolling cash flow forecast |
| How much of our backlog is actually billable vs. at risk? | No | Screen fail allotments not tracked; multi-arm inflation |
| Which studies are running ahead of budget vs. behind? | No | No budget variance tracking by study |
| When will we actually receive cash from approved visits? | No | CRO payment terms tracked but not used for cash flow modeling |
| Which CROs are slow payers? | No | No days-outstanding tracking |
| What does revenue look like 6 months from now if enrollment continues at current pace? | No | No enrollment-velocity-based revenue projection |
| Are we leaving revenue on the table (visits not completed within protocol windows)? | No | No missed visit revenue quantification |
| What is our revenue per active patient, per study, per CRO? | Partially — in aggregate only | No unit economics view |

---

## What Complete CFO Revenue Intelligence Looks Like

This is organized as five distinct capabilities, each building on the last. Together they give a CFO the full picture — from what happened, to what's committed, to what's at risk, to what's coming, to what it should look like.

---

### Capability 1 — Revenue Recognition Engine

**The problem:** Not all revenue in the backlog is recognizable. Screen fails, protocol deviations, partial visits, and over-allotment situations mean that "scheduled" revenue is not the same as "billable" revenue. Solimed currently has no system to separate these.

Drew raised this directly in the demo:
> *"Where you have high screen fail studies like gastroenterology, you have to be careful about the screening revenue that you're recognizing."*

**What to build:**

**Revenue status ladder** — every visit dollar is classified at all times:

```
SCHEDULED           — Visit is on the protocol schedule; not yet earned
    ↓
AT RISK             — Visit is within tolerance window but coordinator has not confirmed
    ↓
EARNED (DONE)       — Visit completed; investigator conducted it; coordinator marked Done
    ↓
APPROVED            — Finance or coordinator approved for billing; monitor sign-off received
    ↓
INVOICED            — Invoice sent to CRO/sponsor
    ↓
RECEIVED            — Payment received; cash in hand
    ↓
WRITTEN OFF         — Visit completed but not billable (over-allotment screen fail,
                      protocol deviation, partial visit below billing threshold)
```

**Screen fail revenue split:**
- Each study has a contractual screen fail allotment (number Solimed can bill for)
- System tracks: total screen fails, billable screen fails (within allotment), over-allotment screen fails
- P&L separates these: "Billable Screening Revenue" vs. "At-Risk Screening Revenue"
- Alert when a study is within 2 screen fails of exhausting its allotment

**Partial visit / unscheduled visit recognition:**
- Unscheduled visits where only some procedures were completed → revenue tied to completed procedures only
- System prompts coordinator to itemize what was completed; billing is calculated from itemized procedures

---

### Capability 2 — Cash Flow Forecast (Rolling 13-Week + 12-Month)

**The problem:** Solimed pays investigators 3x/year, receives payments from CROs on varying terms, and has study-specific billing milestones (startup fees, archiving fees). There is no consolidated view of when money comes in vs. when it goes out.

**What to build:**

**Rolling 13-week cash flow view** (operational — for day-to-day finance management):
- Week-by-week: expected cash receipts from CROs based on payment terms + invoice outstanding days
- Week-by-week: expected cash outflows for investigator payments (3x/year schedule, known amounts)
- Running cash position projection
- Flag: weeks where outflows exceed expected inflows

**Rolling 12-month cash flow view** (strategic — for business planning):
- Month-by-month revenue forecast built from the visit schedule
- Each scheduled visit contributes its expected billing value to the month it is scheduled
- Applied conversion rates: what % of scheduled visits historically convert to billed visits (derived from historical data per study type/CRO)
- Adjustment for screen fail expected rate per study (configurable — "this study typically has 30% screen fail")
- Separate line: fixed revenue milestones (startup fees, archiving fees) with their expected billing dates

**Scenario modeling:**
- Base case: current enrollment pace continues
- Upside case: enrollment accelerates 20%
- Downside case: enrollment slows 20% or a major study is put on hold
- Compare all three scenarios in a single chart

---

### Capability 3 — Backlog Quality Score

**The problem:** Solimed's current backlog figure (€2.1M) is a raw number. A CFO needs to know not just the size of the backlog but the *quality* — how much of it is solid vs. soft.

**What to build:**

**Backlog stratification:**

| Tier | Definition | Color |
|---|---|---|
| **Committed** | Visit scheduled, patient confirmed, date set, within tolerance window, study active and open | Green |
| **Probable** | Visit on protocol for enrolled patient, study active, but not yet scheduled with confirmed date | Yellow |
| **Possible** | Visit on protocol for patient who has not yet reached that point; study active | Light yellow |
| **At Risk** | Visit overdue (past tolerance window); study on hold; patient showing attendance issues | Orange |
| **Excluded** | Multi-arm visits for arms patient has not been assigned to; over-allotment screen fails | Red |

**Backlog dashboard shows:**
- Total backlog split by tier with €value per tier
- Trend: how has each tier changed month over month (is Committed growing? Is At Risk growing?)
- Per-study: backlog quality score (% that is Committed + Probable)
- Alert: studies where At Risk + Excluded exceeds 25% of total — flag for review

**Multi-arm correction** (resolves the known inflation bug):
- Arm assignment per patient after randomization
- Backlog only counts the assigned arm
- "Unassigned arm" patients shown separately with a note that their backlog contribution is estimated

---

### Capability 4 — Enrollment Velocity & Revenue Projection

**The problem:** Revenue from a clinical study is driven by two variables: how many patients are enrolled and how fast they move through the protocol. Solimed has the visit data but no tool to project forward from current enrollment pace.

**What to build:**

**Enrollment velocity tracker per study:**
- Actual enrollment rate: patients screened per week, patients randomized per week (rolling 4-week average)
- Projected enrollment completion: based on current pace, when will the study hit its enrollment target?
- Revenue implication: if enrollment is behind pace, show the revenue shortfall vs. original projection

**Protocol completion projection:**
- For enrolled patients: based on where they are in the protocol, project month-by-month visit completions for the next 12 months
- Aggregate across all patients in a study → study-level revenue projection curve
- Aggregate across all studies → site-level revenue projection curve

**Early termination impact:**
- For studies that end early (ET visits): automatically recalculate revenue impact
- Flag: studies where early termination rate is above historical average for that study type

**Study maturity curve:**
- Ivan mentioned in the demo that coordinators get more efficient as a study matures — hours per visit decreases over time
- Show the maturity curve per study: visits per month as the study ramps, plateaus, and winds down
- Overlay: revenue per month following the same curve
- This gives Drew the picture he asked for: when is each study at peak revenue generation?

---

### Capability 5 — Unit Economics & Performance Benchmarking

**The problem:** Solimed manages studies across 15+ CROs with different terms, different visit structures, and different payment patterns. Without unit economics, it is impossible to know which CROs and study types are the most profitable — or where to focus business development.

**What to build:**

**Revenue per unit metrics:**
- Revenue per active patient (by study, by CRO, by site)
- Revenue per visit completed (by visit type, by study)
- Revenue per coordinator hour (efficiency metric — feeds the normita model)
- Revenue per investigator hour (profitability of investigator relationships)

**CRO performance scorecard:**
- Per CRO: total revenue, margin %, payment terms adherence (days to pay vs. contracted terms), screen fail rate, amendment frequency
- Trend: is a CRO's profitability improving or declining over time?
- Benchmark: compare CRO metrics against Solimed's portfolio average
- Flag: CROs whose payment terms compliance is falling (days outstanding increasing)

**Study type profitability:**
- Revenue and margin by therapeutic area (gastroenterology, oncology, etc.)
- Flag: study types with high screen fail rates that consistently erode expected revenue
- Recommendation engine: flag study types where the revenue/effort ratio is below threshold

**Site comparison (for multi-site future):**
- Once multiple countries are live: same metrics across sites
- Answer: which site is most productive per patient, per coordinator, per study?
- Identifies best practices to replicate across the network

---

## Power BI Implementation Plan

These five capabilities map to specific Power BI screens, built in priority order:

| Screen | Capability | Sprint | Audience |
|---|---|---|---|
| **Revenue Status Ladder** | Cap. 1 — Recognition | Sprint 6 | Finance, Ivan, Drew |
| **Screen Fail Revenue Split** | Cap. 1 — Recognition | Sprint 6 | Finance, Drew |
| **Backlog Quality Dashboard v2** | Cap. 3 — Backlog Quality | Sprint 6 | Ivan, Drew |
| **Rolling 13-Week Cash Flow** | Cap. 2 — Cash Flow | Sprint 7–8 | Drew, Finance |
| **12-Month Revenue Forecast** | Cap. 2 — Cash Flow | Sprint 7–8 | Ivan, Drew |
| **Enrollment Velocity Tracker** | Cap. 4 — Enrollment | Sprint 9–10 | Ivan, Drew, Site managers |
| **Protocol Completion Projection** | Cap. 4 — Enrollment | Sprint 9–10 | Ivan, Drew |
| **CRO Performance Scorecard** | Cap. 5 — Unit Economics | Sprint 11–12 | Ivan, Drew |
| **Unit Economics Dashboard** | Cap. 5 — Unit Economics | Sprint 11–12 | Ivan, Drew |
| **Scenario Modeling Tool** | Cap. 2 — Cash Flow | Sprint 13 (Phase 3) | Ivan, Drew |
| **Multi-Site Benchmarking** | Cap. 5 — Unit Economics | Sprint 14+ (Phase 3) | Ivan, Drew |

---

## Data Requirements

Building these capabilities requires some data that does not currently exist in the system and must be added in Phase 1:

| Data Element | Currently Exists? | Where to Add |
|---|---|---|
| Screen fail allotment per study | No | Site study configuration |
| CRO payment terms (days to pay) | Partially (terms configured but not used) | Site study configuration — activate |
| Invoice sent date | No | New "Invoice" status + date field on billing workflow |
| Payment received date | No | New "Payment received" field on billing workflow |
| Arm assignment per patient | No | Patient record — add after randomization |
| Expected enrollment target per study | No | Study configuration |
| Historical screen fail rate per study type | Derivable | Power BI calculated column from history |
| Coordinator available hours per week | No | New capacity configuration per coordinator |

---

## The Case for Drew

Drew has seen this gap before — at every site network that gets to Solimed's stage of maturity, the operational data exists but the CFO-level revenue intelligence does not. The result is that finance teams are always looking backward, never forward, and the board or investors get surprised when studies slow down or screen fail rates spike.

These five capabilities give Drew — and Ivan — the ability to:

1. **Know what revenue is real** — not just what is scheduled
2. **See cash 13 weeks out** — before problems become crises
3. **Grade the backlog** — know the difference between €2.1M of solid committed visits and €2.1M with €400K of at-risk and over-allotment exposure
4. **Project from enrollment** — answer "what does revenue look like if this study recruits on pace?" before it happens
5. **Compare CROs and study types** — know which relationships are most profitable and where to focus growth

This is the difference between a financial tracking system and a financial intelligence platform. Solimed already has the data. These capabilities make it actionable.

---

*This capability set is referenced in the Enhancement Roadmap (Phase 1–2) and the PM Plan (Sprints 6–13). The Power BI screen designs detailed in the PM Plan (Section 4a) align with this framework.*
