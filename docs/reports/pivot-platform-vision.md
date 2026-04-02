# Pivot Platform — Product Vision
## Unified Clinical Trial Intelligence Platform

**Document Type:** Product Vision
**Prepared by:** Pivot
**Date:** 2026-04-02
**Status:** Internal — Strategic Planning
**Codename:** TBD *(suggested names at end of document)*

---

## The Vision in One Sentence

A single platform that ingests patient data from anywhere, communicates with patients through AI, predicts which candidates will qualify and complete a study, and tracks every dollar from visit to payment — replacing the fragmented combination of Crio, Dialpad, spreadsheets, and EDC workarounds that research sites cobble together today.

---

## The Problem We Are Solving

Clinical research sites today operate with 4–6 disconnected tools:

| Tool | What it does | What it misses |
|---|---|---|
| **CTMS (e.g., Crio)** | Study and visit management | No AI communication layer; no predictive analytics; no payment intelligence |
| **Communications (e.g., Dialpad)** | Phone/SMS/video with patients | No clinical context; conversations not linked to patient records or protocols |
| **EDC (Medidata, Veeva, REDCap)** | Clinical data capture | Sponsor-owned; sites don't control it; no financial layer |
| **Excel** | Scheduling, time tracking, budget modeling | Not a platform — it breaks at scale |
| **EHR (Epic, Cerner)** | Patient health records | Not designed for trial matching; no protocol awareness |
| **Accounting system** | Invoicing and payments | No connection to visit events; manual reconciliation |

The result: coordinators spend 32–40% of their time on administrative work that should be automated. Revenue leaks because billing is manual. Recruitment is reactive — waiting for referrals rather than proactively identifying candidates. Dropout rates are high because nobody predicted which patients were at risk. Payments are late because nobody connected the visit log to the invoice.

**This is the gap. We fill it with one platform.**

---

## Target Customer

**Primary:** Site Management Organizations (SMOs) and independent research sites managing 5–500 active studies across 1–50 sites.

**Secondary:** Large academic medical centers (AMCs) with active research programs looking to commercialize their site operations.

**Beachhead customer:** Solimed — our first development partner. They have the domain knowledge, the existing data, and the right scale to validate the platform before we bring it to market.

---

## Platform Architecture — The Five Layers

```
┌─────────────────────────────────────────────────────────────────┐
│                    LAYER 5: AI INTELLIGENCE                      │
│   Predictive scoring · Recruitment engine · Dropout risk        │
│   Revenue forecasting · Protocol deviation detection            │
├─────────────────────────────────────────────────────────────────┤
│                 LAYER 4: COMMUNICATION HUB                       │
│   AI voice · SMS · Video · Email · Real-time transcription      │
│   Sentiment analysis · Coordinator coaching · Call summaries     │
├─────────────────────────────────────────────────────────────────┤
│                  LAYER 3: CLINICAL OPERATIONS                    │
│   Study/site management · Visit workflow · eSource              │
│   eConsent · Protocol scheduling · Regulatory binder            │
├─────────────────────────────────────────────────────────────────┤
│                  LAYER 2: PAYMENT ENGINE                         │
│   Investigator fees · Patient reimbursements · CRO invoicing    │
│   Cash flow forecasting · Automated payment runs                 │
├─────────────────────────────────────────────────────────────────┤
│                  LAYER 1: DATA INGESTION HUB                     │
│   EHR (Epic/Cerner) · EDC sync · Lab systems · Wearables        │
│   Claims data · Registry data · Social determinants             │
└─────────────────────────────────────────────────────────────────┘
```

---

## Layer 1 — Data Ingestion Hub

**What it does:** Pulls patient and study data from any source and normalizes it into a single patient profile.

**Problem it solves:** Today, sites have patient data scattered across EHRs, EDC systems, paper forms, lab portals, and spreadsheets. No single view exists. Finding candidates for a study means manual chart review — slow, expensive, and incomplete.

### Data Sources Supported

| Source Type | Examples | Integration Method |
|---|---|---|
| **EHR Systems** | Epic, Cerner, Allscripts, athenahealth | HL7 FHIR R4 API |
| **EDC Systems** | Medidata Rave, Veeva Vault, REDCap, Castor | REST API adapters |
| **Lab Systems** | Quest, LabCorp, hospital labs | HL7 v2, FHIR |
| **Wearables / Devices** | Apple Watch, Fitbit, Dexcom CGM, spirometers | FHIR-compatible APIs |
| **Claims / Insurance** | Payer data, insurance records | FHIR Claims resources |
| **Clinical Registries** | Disease registries, national health databases | REST / flat file import |
| **Social Determinants** | Address-based risk scores, transportation access | Third-party enrichment APIs |
| **Manual / CSV Import** | Existing patient lists, referral sheets | Structured CSV import with field mapping |
| **EDC Bidirectional Sync** | Visit status, protocol deviations | Write-back to CRO-owned EDC |

### What Gets Created

Every patient who enters the system from any source gets a **Unified Patient Profile**:
- Demographics (age, sex, ethnicity — from EHR)
- Relevant diagnoses and ICD-10 codes
- Lab values and trends (for eligibility matching)
- Medication history (for inclusion/exclusion)
- Prior study participation (from platform history)
- Communication preferences (phone, SMS, email, language)
- Social determinants score (transportation, literacy, socioeconomic)
- AI-predicted study eligibility scores (Layer 5 output)
- All contact history (Layer 4 output)

---

## Layer 2 — Payment Engine

**What it does:** Connects every completed visit to a payment event — automatically. Tracks money from visit completion to cash in the bank.

**Problem it solves:** At Solimed and every site we've studied, billing is manual. Finance exports reports, creates invoices, emails CROs, and manually marks things paid. Revenue leaks because visits are missed in billing runs. Cash flow is invisible. Investigators get paid months after earning fees because nobody automated the reconciliation.

### Payment Engine Capabilities

**Investigator Fee Management:**
- Per-study fee configuration: PI fee, PI cut %, sub-investigator splits, referral doctor fees
- After-hours / in-hours rate differentiation (confirmed gap at Solimed)
- Monthly automated specification generation and delivery to each investigator
- Investigator self-service portal: view specs, flag discrepancies, see payment history
- Payment run management: batch payments 3x/year or on configurable schedule
- Discrepancy resolution workflow: investigator flags error → coordinator reviews → corrects in system → revised spec auto-resent

**Site Revenue Management:**
- Revenue status ladder: Scheduled → Earned → Approved → Invoiced → Received → Written Off
- Automated invoice generation when visit reaches Approved status
- Invoice template per CRO with correct line items and procedure detail
- CRO payment terms tracking: days outstanding per invoice per CRO
- Cash flow forecast: 13-week rolling + 12-month strategic
- Revenue recognition controls: screen fail allotment tracking, partial visit itemization, over-allotment flagging

**Patient Reimbursement:**
- Per-protocol travel reimbursement rules configured at study setup
- Patient submits travel claim via mobile (photo of receipt + mileage)
- Coordinator reviews and approves in platform
- Payment issued via ACH, check, or prepaid card integration
- Full reimbursement audit trail

**CRO / Sponsor Invoicing:**
- Auto-invoice on visit approval or on configurable billing events
- Procedure-level line items on invoices (unscheduled visit itemization)
- One-time fee invoicing: startup, archiving, pharmacy, administrative
- Invoice status dashboard: sent, viewed, overdue, paid
- Aging report by CRO

---

## Layer 3 — Clinical Operations

**Best of Crio + what Crio is missing.**

**What it does:** Manages the full clinical trial workflow for coordinators and investigators — from study setup through visit completion and regulatory closeout.

### Study & Protocol Management
- Study hierarchy: Study → Site Study → Patient → Visit → Procedure
- Protocol import: upload protocol PDF → AI extracts visit schedule, eligibility criteria, and procedures (with human review)
- Multi-arm studies: patients assigned to arms at randomization; backlog calculated only from assigned arm
- Amendment handling: "effective from" date system; multi-site amendment propagation; notification to all coordinators
- Site replication: create a new site study from an existing one with configurable overrides per site

### Visit Workflow
- Auto-scheduling at randomization: all future visits auto-populated from randomization date + protocol intervals
- Tolerance window enforcement: visits flagged as approaching or outside their tolerance window
- Visit types: Regular, Virtual, SCR, EOT, Colonoscopy, Custom (unscheduled), Early Termination
- Visit status lifecycle: Scheduled → Planned → Done → Approved → Billed
- Working hours flag: in-hours vs. after-hours toggle on every visit; drives fee calculation
- Procedure-level itemization on visits: each procedure has its own completion flag, budget, and billing status
- Bulk status operations: select multiple visits → bulk update status
- Stop Report: when a study is stopped, auto-generates list of all open visits at time of halt

### eSource (Electronic Source Documentation)
**Taking from Crio — the gap Solimed currently has paper for:**
- Structured visit checklists: coordinator and investigator complete digital checklists per visit
- Consultation report generation: auto-generated from checklist completions; replaces paper consultation report
- Timestamped, signed entries: every field entry has user, timestamp, and e-signature
- 21 CFR Part 11 / ICH-GCP compliant audit trail
- Query management: CRA raises a query → coordinator responds → resolved in system
- Remote monitoring: CRA accesses source data without site visit

*Note: Solimed currently uses paper source docs by CRO advisory. eSource is offered as an optional module — sites that want to stay on paper can. Sites ready to go digital have a path.*

### eConsent
- Digital informed consent workflow
- Patient reads, asks questions (via chat or video), and signs electronically
- Re-consent triggered automatically when protocol amendments affect the patient
- Consent version tracking per patient
- 21 CFR Part 11 / GDPR compliant

### Regulatory Binder
- Document storage per study: protocol, IB, ICF versions, ethics approvals, insurance certificates
- Expiry tracking: ethics approval expiry, insurance certificate expiry — alert 30/60/90 days before
- Version control on all documents
- FDA inspection readiness checklist per study

---

## Layer 4 — Communication Hub

**Best of Dialpad, built for clinical trials.**

**What it does:** Handles all communication between coordinators and patients (and between sites and CROs) with AI transcription, sentiment analysis, and real-time coaching — all linked to the patient's clinical record.

**The key difference from Dialpad:** Every call, text, and email is linked to a specific patient record, visit, and study. The AI knows the context. It knows the protocol. It knows whether the patient has missed visits. It knows their sentiment trend over the last 3 calls.

### AI-Powered Patient Communications

**Voice (AI-transcribed calls):**
- Outbound and inbound calls from within the platform
- Real-time transcription with speaker labeling (Coordinator / Patient)
- Post-call AI summary: key topics discussed, action items, next steps
- Call automatically logged against patient's record and upcoming visit
- Escalation detection: if patient expresses distress, withdrawal intent, or adverse event, flag for coordinator supervisor and investigator

**Real-Time Coordinator Coaching (during calls):**
- AI surfaces relevant protocol information during the call ("Patient is due for Visit W144 in 5 days — confirm appointment")
- Adverse event detection: AI flags if patient describes a symptom that matches the study's AE monitoring list — prompts coordinator to capture it
- Eligibility reminders: during screening calls, AI surfaces the inclusion/exclusion criteria and prompts coordinator to ask each question
- Compliance coaching: if coordinator deviates from script, AI provides a gentle prompt

**SMS / MMS:**
- Two-way texting from within the platform
- Automated visit reminders: configurable templates per visit type (e.g., "Your visit is tomorrow at 10am. Reply CONFIRM or RESCHEDULE.")
- Pre-visit instructions: sent automatically 48 hours before visit
- Post-visit follow-up: automated check-in SMS 24–48 hours after visit
- Patient opt-out management: TCPA-compliant

**Video:**
- Native video visits for virtual visit types
- Recorded with consent; linked to visit record
- Screen share for e-consent walkthrough

**Email:**
- Outbound email from coordinator with templates per study/visit
- Automated: monthly investigator specifications, visit reminders, study updates
- Inbound email threaded per patient

### Sentiment Intelligence
- Per-patient sentiment trend: is this patient's engagement getting better or worse over time?
- Study-level sentiment: which studies have the most stressed/disengaged patient populations?
- Early withdrawal prediction: sentiment + missed visits + call avoidance → dropout risk score (feeds Layer 5)
- Coordinator performance: which coordinators have the highest patient satisfaction scores?

### CRO / Sponsor Communications
- Dedicated channel per study for CRO communication
- Site visit scheduling (monitor visits) tracked and linked to study record
- Meeting intelligence: AI summarizes sponsor calls; action items extracted and assigned
- Query responses tracked within the platform

---

## Layer 5 — AI Intelligence

**The layer that makes the platform predictive.**

**What it does:** Applies machine learning across all data in the platform — patient profiles, visit histories, communication sentiment, payment patterns — to surface predictions that help sites make better decisions before things go wrong.

### Patient Qualification Engine

**Purpose:** Find the best candidates for a study before the study starts — from existing patient populations, EHR data, and external registries.

**How it works:**
1. Coordinator uploads or links a study protocol (inclusion/exclusion criteria, patient demographics, lab value ranges, medication requirements)
2. Platform runs the eligibility criteria against the entire patient database (across all data sources in Layer 1)
3. Returns a ranked list of candidates with:
   - **Match score** (0–100): how closely the patient's profile matches the protocol criteria
   - **Qualification probability**: likelihood of passing formal screening based on the patient's lab trends and demographics
   - **Contact score**: how likely is this patient to respond and engage (based on communication history)
   - **Completion probability**: how likely is this patient to complete the full protocol (based on prior study history and social determinants)

**Output:** Coordinator sees a prioritized list — start with the patients most likely to qualify AND complete, not just those who appear eligible on paper.

### Screening & Dropout Risk Prediction

**Pre-randomization (screening) prediction:**
- Before formal screening, AI scores each candidate:
  - Screen fail probability: based on demographics, lab trends, similar patients in historical data
  - Early withdrawal risk: based on social determinants, communication engagement, distance from site
- Coordinator focuses recruitment effort on high-probability patients; avoids over-investing in likely screen fails

**Post-enrollment (retention) prediction:**
- Per-patient dropout risk score, updated after every visit and every communication
- Inputs: missed visits, late visits, call sentiment trends, adverse events, protocol complexity, time since last contact
- Alert: patient crosses a risk threshold → coordinator receives prompt to intervene (call the patient, offer additional support, escalate to PI)
- Predicted dropout date: "at current trend, Patient 403001 is likely to withdraw within 3 visits" — gives coordinator time to act

### Study Revenue Prediction

Built on Drew's CFO revenue intelligence framework:
- **Backlog quality score**: per study, per patient — stratifies backlog into Committed / Probable / At Risk / Excluded
- **Enrollment velocity model**: projects monthly enrollment completion based on current pace + historical patterns for this study type
- **Revenue curve per study**: based on enrollment pace, protocol length, and historical visit completion rates — projects month-by-month revenue for the next 12 months
- **Scenario modeling**: base / upside / downside scenarios with configurable assumptions
- **Payment timing prediction**: based on CRO payment terms and historical payment behavior, predicts when invoiced revenue will convert to cash

### Protocol Deviation Detection

- AI monitors all visit entries against the protocol schedule in real-time
- Flags: visit conducted outside tolerance window, incorrect procedure recorded, missing required assessment
- Generates a Protocol Deviation report per study per audit period
- Distinguishes: minor deviations (no patient safety impact) vs. major deviations (may require CRO notification)

### Coordinator Performance Intelligence

- Visit-per-hour efficiency by coordinator (from time tracking)
- Protocol adherence rate by coordinator (deviation frequency)
- Patient retention rate by coordinator (dropout rate per coordinator's patient list)
- Communication quality score (from AI sentiment analysis of their calls)
- Coaching recommendations: "Coordinator X has a high screen fail rate on Study 42 — suggest reviewing the screening call script"

---

## Patient Discovery & Recruitment Engine

This is a standalone capability that operates before a patient is enrolled — the top-of-funnel that most sites completely lack.

**The problem today:** Sites wait for referrals from physicians, word-of-mouth, and ClinicalTrials.gov listings. Recruitment is passive. Enrollment timelines extend. Studies fail because sites can't find enough eligible patients fast enough.

**What the platform provides:**

### Internal Discovery (Existing Patient Population)
- Run eligibility screening against the full patient database (EHR + prior study participants + referral history)
- Identify patients who have never been approached for a study but match a protocol
- Priority queue: ranked by match score, qualification probability, and contact score

### External Discovery (New Patient Acquisition)
- **Physician referral network:** Manage a network of referring physicians; track referrals per physician; auto-send new study opportunities to relevant physicians in the network
- **Digital intake forms:** Publish study-specific pre-screening questionnaires on a patient-facing landing page; candidates self-screen; qualified candidates enter the recruitment pipeline
- **Registry partnerships:** Connect to disease registries and patient advocacy organizations; candidates opt in to be contacted about studies
- **Social/digital campaigns (optional):** Track campaign leads from Facebook/Google health ads; integrate with CRM for follow-through

### Recruitment Pipeline Management
- Each candidate moves through stages: Identified → Contacted → Pre-screened → Scheduled for Screening → Screened → Enrolled
- Stage-level conversion rates: where does the pipeline lose candidates?
- Time-in-stage tracking: candidates sitting in "Contacted" for >7 days get an automatic follow-up prompt
- Diversity monitoring: real-time visibility into the demographic composition of the pipeline; alert if pipeline is under-representing target diversity for the study
- Cost-per-enrolled-patient tracking: total recruitment spend ÷ enrolled patients per study

---

## Competitive Positioning

| Feature | Our Platform | Crio | Dialpad | Medidata | Florence |
|---|---|---|---|---|---|
| Clinical visit workflow | ✅ Full | ✅ Full | ❌ | ✅ Full | ✅ Partial |
| eSource / eConsent | ✅ | ✅ | ❌ | ✅ | ✅ |
| AI communication (call/SMS/video) | ✅ Full | ❌ Basic | ✅ Full | ❌ | ❌ |
| Real-time call coaching | ✅ | ❌ | ✅ | ❌ | ❌ |
| Sentiment analysis | ✅ | ❌ | ✅ | ❌ | ❌ |
| Multi-source data ingestion | ✅ Full | ❌ | ❌ | ✅ Partial | ❌ |
| Patient qualification AI | ✅ Full | ❌ Basic | ❌ | ❌ Partial | ❌ |
| Dropout risk prediction | ✅ | ❌ | ❌ | ❌ | ❌ |
| Investigator payment engine | ✅ Full | ❌ | ❌ | ❌ | ❌ |
| CRO invoicing automation | ✅ Full | ❌ | ❌ | ❌ | ❌ |
| Cash flow forecasting | ✅ | ❌ | ❌ | ❌ | ❌ |
| Patient recruitment pipeline | ✅ Full | ✅ Partial | ❌ | ❌ | ✅ Partial |
| Revenue prediction | ✅ | ❌ | ❌ | ❌ | ❌ |
| Multi-country / multi-tenant | ✅ | ❌ | ✅ | ✅ | ❌ |
| Open API / EHR integration | ✅ FHIR | ✅ | ❌ | ✅ | ❌ |

**The gap in the market:** No single platform combines clinical operations + AI communications + predictive analytics + payment intelligence. Sites buy 3–4 tools and stitch them together. We are the first to do all of it in one place.

---

## Technology Architecture

### Core Stack

| Layer | Technology | Rationale |
|---|---|---|
| Frontend | Next.js (React) | SSR performance; component-driven; same codebase as mobile shell |
| Mobile | React Native | Coordinators and investigators on iOS/Android; offline-first for field use |
| API | Node.js (Fastify) + GraphQL | Fast; flexible querying for complex patient/study relationships |
| Database — primary | PostgreSQL (Azure) | Relational; ACID compliant; strong for financial transactions |
| Database — patient profiles | MongoDB Atlas | Document store for flexible, multi-source patient profiles |
| Database — time-series | InfluxDB or TimescaleDB | Wearable data, lab trends, sentiment over time |
| AI / ML | Python (FastAPI) | Separate ML service; models trained in PyTorch / scikit-learn |
| LLM integration | Claude API (Anthropic) | Call summaries, protocol extraction, AI coaching prompts |
| Data ingestion | Apache Kafka + Azure Data Factory | Real-time streaming + batch ETL from EHR/EDC sources |
| FHIR layer | Azure Health Data Services | FHIR R4 compliance; EHR interoperability standard |
| Communications | Twilio (voice/SMS) + Daily.co (video) | Programmable; HIPAA-eligible |
| Auth | Auth0 or Azure AD B2C | Multi-tenant; RBAC; MFA |
| Search | Elasticsearch | Patient eligibility matching at scale; protocol criteria search |
| Reporting | Power BI Embedded | Retain for Solimed + standard for all SMO clients |
| Infrastructure | Azure (Kubernetes) | Microsoft ecosystem alignment; global regions; HIPAA BAA available |
| CI/CD | GitHub Actions | Code in GitHub; automated test and deploy pipeline |

### Compliance Architecture

| Requirement | How Met |
|---|---|
| HIPAA (US) | Azure HIPAA BAA; Twilio HIPAA-eligible; encrypted at rest + transit; PII audit log |
| GDPR (EU) | Data residency per country; consent management; right-to-erasure workflow; DPA with all processors |
| ICH-GCP | Audit trail on all clinical data; timestamped, attributed, immutable |
| 21 CFR Part 11 | Electronic signatures on eSource and eConsent; audit trail; access controls |
| SOC 2 Type II | Planned for Year 2 post-launch |

---

## Development Roadmap

### Phase 1 — Solimed (Months 1–9)
Build the core platform using Solimed as the design partner. All 5 layers built but scoped to Solimed's specific needs. Solimed gets a better product; we get a validated platform with real clinical trial data.

**Deliverables:**
- Layers 1–3 (Data ingestion from Power Apps migration, clinical operations, payment engine)
- Layer 4 (Communication hub — AI voice, SMS, call coaching)
- Layer 5 foundations (Backlog quality, cash flow prediction, basic dropout risk)

### Phase 2 — Second Site (Months 10–15)
Onboard a second SMO (target: existing relationship in new country). Validate multi-tenancy. Prove the country expansion model.

**Deliverables:**
- Full multi-tenant architecture live
- First country beyond Croatia on the platform
- Patient qualification engine (Layer 5) trained on Solimed's historical data

### Phase 3 — Market Launch (Months 15–24)
Open the platform to other SMOs. Self-service onboarding. Subscription pricing. CRO portal.

**Deliverables:**
- Self-serve onboarding wizard
- Subscription billing (Stripe)
- CRO network portal
- Full recruitment engine live
- Patient discovery from EHR integrations (Epic, Cerner)

---

## Business Model

| Revenue Stream | Model | Target |
|---|---|---|
| **Platform subscription** | Per-site per-month (€500–€2,000/site/month based on tier) | SMOs, research sites |
| **CRO portal access** | Per-CRO per-month (€200–€500/CRO/month) | CROs managing 10+ sites |
| **AI Intelligence add-on** | Per-study per-month (€50–€200/study) | Sites using predictive features |
| **Communication usage** | Per-minute (voice) + per-SMS (pass-through Twilio cost + margin) | All sites |
| **Implementation / setup fee** | One-time per site (€2,000–€10,000) | New site onboarding |
| **Data services** | Custom pricing for EHR integration setup | Large AMCs and hospital networks |

**Year 3 target:** 50 sites × €1,000/month average = €600K ARR base + AI add-ons + usage

---

## Platform Name — Options

| Name | Meaning | Domain likely available? |
|---|---|---|
| **Meridian** | Navigation reference point — finding the exact position of a patient in their clinical journey | Likely |
| **Helix** | DNA double helix — clinical/life sciences reference | Unlikely (taken) |
| **Nexus** | Connection point — where all data sources connect | Unlikely |
| **Canopy** | Protection and coverage — covering the full trial journey | Possible |
| **Cadence** | Rhythm — the rhythm of clinical visits, payments, and protocols | Possible |
| **Lumen** | Unit of light — illuminating the patient journey | Possible |
| **Stratum** | Layers — the layered platform architecture | Possible |

---

## Relationship to Solimed Engagement

Solimed is the **first customer and development partner** for this platform. The Pivot-Solimed engagement is not just contractor work — it is the Phase 1 build of this product with Solimed as the design partner and first reference customer.

**What Solimed gets:**
- A platform tailored to their specific needs, built with their domain experts
- First-mover advantage: when the platform launches, they are on the most mature version
- Potential equity or revenue share if they want to invest in the platform as a product

**What Pivot gets:**
- A real clinical trial environment to build and validate the platform
- Solimed's 15+ CRO relationships as potential future customers
- Drew Domescik's CFO network as an introduction channel to other SMOs

**The transition moment:** When the second SMO is onboarded (Phase 2), Solimed's engagement transitions from "custom development" to "platform customer." Their monthly fee becomes a platform subscription. Their feedback becomes the product roadmap.

---

*This is a living vision document. It will be updated as Solimed's Sprint 1 knowledge transfer and market research refines the feature priorities.*
