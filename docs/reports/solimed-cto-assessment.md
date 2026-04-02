# Solimed Study Tracking — CTO Technology Assessment

**Client:** Solimed
**Prepared by:** Pivot
**Date:** 2026-04-02
**Source:** Jan 23, 2026 demo recording (visual analysis + full transcript)
**Audience:** Internal Pivot / Solimed leadership

---

## Executive Summary

Solimed has built a functionally sound, domain-specific SMO platform using Microsoft Power Apps and Power BI. The technology choices were pragmatic and appropriate for the stage of the business — the team moved fast, validated their processes, and built real operational value without over-engineering. That said, the platform is now at a well-known inflection point: the low-code foundation that enabled fast early delivery is becoming the ceiling for the next phase of growth.

**Bottom line:** The current stack is not broken — it is simply approaching the boundary of what it can support. The right response is not a rewrite; it is a deliberate, phased modernization that preserves what works while building the layer that will carry the product to international scale.

**Overall technical maturity rating: 5.5 / 10**
Appropriate for current scale. Requires structured investment to reach 8/10 before international expansion.

---

## 1. Current Technology Stack

### What They Have
| Layer | Technology | Notes |
|---|---|---|
| Frontend / App | Microsoft Power Apps | Canvas app; browser-based; no mobile |
| Reporting / BI | Microsoft Power BI | Embedded dashboards; data model in Power BI Desktop |
| Data Store | Microsoft Dataverse (implied) or SharePoint Lists | Standard Power Apps backend |
| Authentication | Microsoft 365 / Azure AD | Single-tenant, corporate accounts |
| Time Tracking | Excel spreadsheet | Connected to Power BI via Power Query |
| Hosting | Microsoft Cloud (Power Platform) | Managed; no infrastructure to maintain |
| Integration | None (deliberate) | No EDC, no accounting, no external APIs |

### What They Do Not Have
- REST API layer
- Mobile application
- Automated email/notification system (being added manually)
- Document management
- CI/CD pipeline
- Test suite
- Staging environment (implied — single environment)
- Multi-tenant architecture
- Localization / i18n framework

---

## 2. Architectural Assessment

### 2.1 Data Architecture

**Current state:**
Power Apps uses Microsoft Dataverse (or SharePoint Lists) as its backing store. The data model is well-conceived for the domain — the Study → Site Study → Patient → Visit hierarchy is logically sound and reflects real clinical trial structure. The "effective from" date system for budget amendments is a particularly clever piece of design that handles a genuinely hard problem (retroactive budget changes) correctly.

**Concerns:**
- **No separation of concerns:** Business logic, data access, and presentation are all embedded in Power Apps formulas. There is no API layer — the app reads/writes directly to the data store. This means there is no way to access the data programmatically, build integrations, or support a mobile app without going through the Power Apps layer.
- **Dataverse scalability:** Dataverse is capable at moderate volumes, but Power Apps' formula-driven data access patterns (especially gallery controls loading large datasets) can create performance problems as patient/visit volume grows. With 321 active patients today and plans to scale to multiple countries, this will surface within 12–18 months if not addressed.
- **No read/write separation:** Reports and operational writes go through the same data layer. At scale, heavy Power BI report refreshes can contend with coordinator write operations.
- **Multi-tenancy:** The current data model has no `tenant_id`, `country_id`, or `site_id` isolation pattern. All sites share the same data namespace. Adding a second country today would require either a separate Power Apps environment (operational nightmare) or retrofitting the schema (risky on a live system).

**Rating: 5/10** — Correct domain model, but architecture will not support international scale without structural changes.

---

### 2.2 Application Architecture

**Current state:**
Power Apps Canvas apps are event-driven, formula-based applications. They are excellent for rapid CRUD interfaces but have well-documented constraints:

**Strengths of the current approach:**
- Zero infrastructure management — Microsoft handles hosting, availability, and patching
- Deep Power BI integration — same identity provider, same data connectors, native embedding
- Fast iteration — Mladen can ship a new field or form change in hours, not days
- Familiar to Microsoft-shop enterprises (relevant for CRO/hospital clients)

**Hard constraints they will hit:**
- **Formula complexity ceiling:** Power Apps formulas cannot be unit tested, version controlled meaningfully, or refactored safely at scale. As business logic grows more complex (multi-arm arms, procedure-level budgeting, normita calculations), it becomes increasingly fragile.
- **No background processing:** Power Apps cannot run scheduled jobs, background processes, or event-driven triggers natively. The automated investigator specification sending they want requires either Power Automate (which has its own limitations and licensing costs) or an external API.
- **Performance with large datasets:** Power Apps loads data client-side. A coordinator view showing 321 patients × their visits is already slow in some scenarios (implied by delegation warnings they likely encounter). At 1,000+ patients across multiple countries, this becomes unusable without careful delegation handling.
- **No true offline capability:** Power Apps has limited offline support. For field investigators needing to log visits without connectivity, this is a hard blocker.
- **Customization limits:** Complex UI patterns (e.g., the matrix visit/patient toggle they built) require creative workarounds in Power Apps. Drew asked about procedure-level drill-down; this is extremely difficult to build well in Canvas apps.

**Rating: 5/10** — Excellent for the current scale and team size. Will require augmentation within 12 months and migration within 24 months for international scale.

---

### 2.3 Reporting Architecture

**Current state:**
Power BI is the strongest part of the technical stack. The data model is well-structured, the dashboards show genuine analytical sophistication (margin tracking, multi-year backlog, investigator fee breakdowns, utilization), and the Microsoft-to-Microsoft integration with Power Apps is seamless.

**Strengths:**
- Power BI is genuinely best-in-class for this use case — no recommendation to change this
- The separate PNL dashboards per site (Solimed vs. Medico) show good multi-tenant thinking at the reporting layer even if not yet at the data layer
- Calendar view built in Power BI (not in the app) is a clever pragmatic choice — added a complex feature without app development
- The data model in Power BI is flexible — new columns, measures, and relationships can be added without touching the app

**Concerns:**
- **Excel time tracking as a Power BI data source:** The time tracking spreadsheet being connected via Power Query is a fragile point. If the Excel file format changes, the connection breaks. If multiple people edit it simultaneously, data conflicts occur. This needs to move into a proper data store.
- **No semantic layer / single source of truth:** Business metrics (e.g., "backlog") are calculated differently depending on who's viewing — the multi-arm inflation problem is a symptom of this. A proper semantic layer (either in Power BI or an API) would ensure consistent calculation everywhere.
- **Refresh frequency:** Power BI datasets on shared capacity refresh on a schedule (not real-time). For the Medico RI shared dashboard they mentioned, there may be a lag between a coordinator marking a visit done and the Medico finance team seeing it. At scale, this matters.
- **Licensing costs at scale:** Power BI Pro licenses are ~€10/user/month. As they expand to multiple countries with many site staff, this compounds. Power BI Embedded (for sharing without per-user licenses) should be evaluated for external-facing dashboards.

**Rating: 7.5/10** — Best component in the stack. Retain and invest in it. Address the Excel dependency and semantic layer gaps.

---

### 2.4 Integration Architecture

**Current state:** There is no integration architecture. This is explicitly intentional — Solimed does not replicate clinical data from CRO EDC systems, and the only external data flow is the Excel time tracking sheet into Power BI.

**Assessment:**
This was the right call at the current stage. Building EDC integrations early would have been over-engineering for a 2-site operation. However, as they scale:

- **Investigator specification sending** (their next automation step) requires either Power Automate or an external API — this is the first integration they need
- **EDC integration** (Medidata, Veeva, REDCap) will eventually be a competitive requirement — CROs will prefer SMO partners whose systems can receive data without manual re-entry
- **Accounting/ERP integration** becomes necessary when automated invoicing is added and finance teams in multiple countries need books to reconcile automatically

The absence of an API layer is the single biggest architectural gap. Without it, none of the integrations above are possible without coupling tightly to Power Platform, which limits vendor flexibility and imposes Microsoft licensing on all integration partners.

**Rating: 3/10** — Appropriate for today; a blocker for Phase 3+.

---

### 2.5 Security & Compliance Architecture

**Current state:**
Authentication is handled by Microsoft 365 / Azure AD — this is solid. Power Platform inherits Azure AD's security model, including MFA support.

**Critical gaps:**

- **RBAC is binary:** The app distinguishes between admin and non-admin users, but has no fine-grained role model. Coordinators can see data they shouldn't (other coordinators' studies), and there is no concept of country-level or site-level isolation. For international expansion, this is a compliance blocker.
- **No PII audit logging:** There is no record of who accessed which patient record and when. Under GDPR (EU), this is legally required. Under HIPAA (if they expand to the US), it is required. Under most country-level data protection laws they'll encounter during expansion, it will be required.
- **Data residency:** Power Platform environments can be provisioned in specific Azure regions, but there is no per-tenant/per-country residency configuration. All data currently lives in a single region. Country-specific data residency (required in some markets, e.g., Germany, some Middle East countries) is not architected.
- **No penetration testing:** Implied from the demo — no mention of security testing, vulnerability scanning, or formal security review.
- **Source documentation is paper:** While deliberate and CRO-approved, this means Solimed has no digital audit trail for clinical data. If CRO or regulatory requirements change, there is no path to digital source docs without a significant new system.

**Rating: 4/10** — Azure AD foundation is good. Everything above it needs structured investment before country expansion.

---

### 2.6 Development Operations (DevOps)

**Current state:**
Power Apps has version history built-in (solution versioning), but it is not git-based, not branch-based, and does not support pull request workflows or automated testing. Mladen appears to be the sole developer, deploying directly to production.

**Critical gaps:**
- **No automated testing:** Power Apps formulas cannot be unit tested. There is no regression test suite. A change to the visit budget calculation could silently break fee calculations with no automated detection.
- **No staging environment:** Changes appear to go directly to production. For a financial system handling €900K+ in real budgets, this is high risk.
- **Single developer dependency:** The entire technical knowledge base is in one person (Mladen). No documentation of the data model, business logic, or architectural decisions was mentioned. This is a key-person risk.
- **No CI/CD pipeline:** No automated deployment, no build validation, no linting or code review process (because there is no "code" in the traditional sense with Power Apps).
- **No incident response process:** No monitoring, alerting, or on-call rotation mentioned.

**Rating: 3/10** — Acceptable for a one-developer internal tool at early stage. Not acceptable for a multi-country platform handling clinical trial financial data.

---

## 3. Technical Debt Inventory

| Item | Severity | Description |
|---|---|---|
| Excel time tracking as data source | High | Single point of failure; not concurrent-safe; will break at scale |
| No API layer | High | Blocks all integrations, mobile, and third-party access |
| Binary RBAC model | High | Compliance blocker for international expansion |
| No PII audit log | High | GDPR/legal requirement in most expansion markets |
| Multi-arm backlog calculation | High | Produces materially incorrect financial forecasts |
| Single developer, no documentation | High | Key-person risk; knowledge not transferable |
| No staging environment | Medium | Financial system changes go direct to production |
| No automated tests | Medium | No safety net for changes to fee calculations |
| Data residency not configurable | Medium | Required for some country markets |
| Power BI Excel connector | Medium | Fragile; Excel format changes break reports |
| No semantic layer for business metrics | Medium | Same metric calculated differently in different places |
| No mobile support | Medium | Field investigators cannot log visits offline |
| Working hours flag missing | Medium | Investigator fees calculated incorrectly for after-hours visits (in progress) |
| No document management | Low | Protocol docs, consent forms on paper/email |
| No screen fail allotment tracking | Low | Revenue recognition risk, especially high-fail-rate studies |

---

## 4. Technology Risk Assessment

### Risk 1: Platform Lock-in (HIGH)
**Issue:** The entire application is built on Microsoft Power Platform. Migrating away would require rebuilding from scratch.
**Mitigation:** This is manageable as long as the migration is planned before the constraints become acute. The recommended approach (build an API layer alongside Power Apps, then migrate the front-end incrementally) avoids a big-bang rewrite.
**Timeline:** Begin API layer in Phase 2 (Months 3–5). Power Apps front-end can remain through Phase 3. Full front-end migration in Phase 4–5.

### Risk 2: Key Person Dependency (HIGH)
**Issue:** Mladen Geng holds all institutional knowledge of the platform — data model, business logic, edge cases, workarounds. If he is unavailable, no one can maintain or evolve the application.
**Mitigation:** Sprint 1 must include a full knowledge transfer and documentation sprint. All business logic must be documented before Pivot writes a single line of code.
**Timeline:** Resolve in Sprint 1 (Month 1).

### Risk 3: Data Integrity at Scale (MEDIUM-HIGH)
**Issue:** The multi-arm backlog inflation bug, the Excel time tracking connector, and the lack of a semantic layer all represent points where data can be materially wrong without detection. For a financial system, this is serious.
**Mitigation:** Data validation layer in the API (Phase 2); canonical business metric definitions in a semantic layer; automated reconciliation reports.
**Timeline:** Phase 2 (Months 3–5).

### Risk 4: Compliance Gap for International Markets (HIGH)
**Issue:** GDPR (EU), local data protection laws, data residency requirements, and clinical trial regulatory frameworks vary by country. The current platform has none of the tooling to support compliance in a new market.
**Mitigation:** Regulatory review must precede every country launch. RBAC expansion and PII audit logging must be complete before Phase 3 begins.
**Timeline:** Phase 2 completion is a prerequisite for Phase 3.

### Risk 5: Financial Data Accuracy (MEDIUM)
**Issue:** Visit fee calculations (PI cut, PI fee, sub-investigator splits, after-hours rates) are implemented in Power Apps formulas. These formulas are not tested, not version-controlled in any meaningful way, and a change by Mladen could silently break fee calculations. Given that investigators reconcile their specs monthly and flag discrepancies, errors would eventually surface — but potentially after payment.
**Mitigation:** Fee calculation logic must be migrated to the API layer (tested, versioned, auditable) in Phase 2. Until then, monthly reconciliation is the only safety net.
**Timeline:** Phase 2 (Months 3–5).

---

## 5. Technology Roadmap Recommendation

### Phase 1 (Months 1–3): Stabilize on Current Stack
**Goal:** Deliver confirmed quick wins without introducing new technology risk.
- All work in Power Apps + Power BI
- Parallel: complete knowledge transfer and document the full data model
- Parallel: set up a staging Power Apps environment so changes are not deployed directly to production
- Deliver: working hours flag, multi-arm fix, screen fail tracking, investigator spec automation

**No new technology introduced in Phase 1.**

---

### Phase 2 (Months 3–5): Introduce the API Layer
**Goal:** Build the technical foundation that makes everything else possible.

**Recommended stack for the API layer:**

| Component | Choice | Rationale |
|---|---|---|
| API runtime | **Node.js (Fastify)** or **Python (FastAPI)** | Fast development; strong ecosystem; both have excellent Azure deployment support |
| Database | **Azure SQL** (primary) | Relational — fits the structured clinical trial data model; already in the Microsoft ecosystem |
| ORM | **Prisma** (Node) or **SQLAlchemy** (Python) | Type-safe database access; migration management |
| Auth | **Azure AD B2C** | Already in the Microsoft ecosystem; supports multi-tenant, external identities, MFA |
| Hosting | **Azure App Service** (start) → **Azure Container Apps** (scale) | Managed; cost-effective at start; scales with containers |
| CI/CD | **GitHub Actions** | Code lives in GitHub; Actions is first-class and free for standard workflows |
| Testing | **Jest** (Node) or **pytest** (Python) | Industry standard; enforce >80% coverage on business logic |
| API docs | **OpenAPI / Swagger** | Required for future integrations |

**What the API layer does in Phase 2:**
- Exposes all core entities: Studies, Sites, Patients, Visits, Budgets, Investigators
- Centralizes all fee calculation logic (replaces Power Apps formulas)
- Implements RBAC (6-tier role model)
- Provides PII audit logging
- Enables Power Apps to become a thin UI over the API (decoupling presentation from logic)
- Enables Power Automate or direct SMTP for automated investigator spec sending

**Power Apps remains the UI.** Coordinators see no change. The API runs behind Power Apps initially — Power Apps calls the API instead of Dataverse directly.

---

### Phase 3 (Months 6–9): Localization and Data Isolation
**Goal:** Make the platform deployable in a new country without code changes.

**Additional technology introduced:**
- **i18n library** (react-i18next or equivalent when front-end migrates; Power Apps has limited i18n support — manage translations at API/data level in the interim)
- **FX rate service:** ECB or OpenExchangeRates API for daily currency updates
- **Azure region per country:** New Solimed tenant in a new country gets its own Azure resource group in the appropriate region (e.g., Germany for Germany data residency)
- **Tenant configuration service:** A config store (Azure App Configuration or database table) that holds per-country settings: currency, locale, tax rules, regulatory fields

---

### Phase 4 (Months 10–14): Front-End Migration + Mobile
**Goal:** Migrate from Power Apps to a modern web front-end; add mobile.

**Recommended front-end stack:**

| Component | Choice | Rationale |
|---|---|---|
| Framework | **Next.js (React)** | SSR for performance; strong ecosystem; same language as API if Node.js chosen |
| UI component library | **shadcn/ui** or **Ant Design** | Pre-built components; accessible; customizable |
| State management | **React Query** (TanStack Query) | API-first state management; caching; optimistic updates |
| Forms | **React Hook Form + Zod** | Type-safe validation; matches the form-heavy nature of the app |
| Mobile | **React Native** | Code sharing with web (components, business logic, types) |
| Offline sync | **WatermelonDB** or **MMKV + custom sync** | Offline-first for field investigators |

**Why migrate from Power Apps to Next.js:**
- Power Apps costs ~€15–25/user/month for premium connectors — at 50+ users across multiple countries, this is €750–1,250/month just in licensing
- Next.js is free; hosting on Azure Static Web Apps or Vercel is ~€20–50/month total
- Complex UI patterns (procedure drill-down, matrix views, multi-arm arm assignment) are straightforward in React; they are painful workarounds in Power Apps
- The mobile app cannot share code with Power Apps; it can share code with Next.js

**Power BI is retained** and embedded in the Next.js app via Power BI Embedded SDK. No change to reporting.

---

### Phase 5 (Months 15–20): SaaS Platform
**Goal:** Multi-tenant, self-serve, subscription-based.

**Additional technology:**
- **Stripe:** Subscription billing and usage metering
- **Azure Service Bus:** Async event processing (visit status changes → invoice triggers → notification events)
- **Azure Cognitive Search:** Full-text search across studies, patients, investigators at scale
- **ML platform:** Azure Machine Learning or lightweight Python inference API for visit no-show prediction and budget overrun early warning

---

## 6. Build vs. Buy Analysis

Several components should be evaluated as buy vs. build:

| Component | Build or Buy | Recommendation |
|---|---|---|
| Core CTMS features | Build | Domain is too specific; existing vendors are over-featured |
| Authentication / SSO | Buy | Azure AD B2C — do not build auth |
| Reporting / BI | Buy (retain) | Power BI Embedded — best in class for this use case |
| E-signature | Buy | DocuSign or Adobe Sign API — commodity feature |
| Email delivery | Buy | SendGrid or Azure Communication Services — do not build SMTP |
| FX rates | Buy | ECB or OpenExchangeRates API — commodity data |
| EDC integration | Build adapter layer | Each EDC has its own API; build thin adapters, not a full integration platform |
| Invoicing / PDF | Buy | Docmosis or similar PDF generation API — do not build from scratch |
| Subscription billing | Buy | Stripe — industry standard; do not build billing |
| Mobile offline sync | Build | No off-the-shelf solution fits the clinical visit data model |
| Regulatory intelligence | Buy (initially) | Use a regulatory database service; build alerting layer on top |

---

## 7. CTO Recommendations — Priority Order

1. **Immediately:** Complete knowledge transfer from Mladen and document the full data model, business logic, and all known edge cases. This is the single highest-risk item and costs nothing except time.

2. **Month 1:** Establish a staging Power Apps environment. No change goes to production without being tested in staging first.

3. **Month 2:** Fix the multi-arm backlog calculation. The €2.1M backlog figure that leadership is using for business planning is materially inaccurate for studies with branching protocols. This is a financial reporting integrity issue.

4. **Months 3–5:** Build the API layer. This is the most impactful technical investment — it unblocks integrations, mobile, proper RBAC, PII audit logging, and automated processes simultaneously.

5. **Before Phase 3:** Complete security audit and penetration test. Do not deploy a clinical trial financial platform to a new country without a formal security review.

6. **Long term:** Plan the Power Apps → Next.js migration as a deliberate program, not a crisis. If it is planned and executed well, coordinators experience it as a UI refresh, not a disruption.

---

## 8. Summary Scorecard

| Dimension | Current Score | Target (Phase 2 Complete) | Target (Phase 5 Complete) |
|---|---|---|---|
| Data Architecture | 5/10 | 8/10 | 9/10 |
| Application Architecture | 5/10 | 7/10 | 9/10 |
| Reporting / BI | 7.5/10 | 8.5/10 | 9/10 |
| Integration Architecture | 3/10 | 7/10 | 9/10 |
| Security & Compliance | 4/10 | 8/10 | 9/10 |
| DevOps & Operations | 3/10 | 7/10 | 9/10 |
| **Overall** | **5.5/10** | **7.5/10** | **9/10** |

---

*This assessment is based on the January 23, 2026 demo recording. A technical discovery session with Mladen Geng to review the actual Power Apps configuration, data model, and Power BI data model is required to validate these findings before Phase 2 architecture begins.*

---

## 9. Scaling Capability Recommendations

Scaling Solimed's platform operates across four dimensions simultaneously: **functional capabilities** (what the app can do), **technical capacity** (how many users/sites/studies it can handle), **operational capabilities** (how the team runs the platform), and **geographic capabilities** (how it expands to new countries). Each dimension has its own scaling path and its own failure modes if neglected.

---

### 9.1 Functional Capability Scaling

These are the features that must grow with the business. They are ordered by the dependency chain — later items require earlier items to be in place.

#### Tier 1 — Immediate (Months 1–3): Fix What's Broken at Current Scale
The platform already has functional gaps that create operational risk at its current 2-site, 321-patient scale. These must be resolved before adding any new capability.

| Capability | Current Gap | Scaling Recommendation |
|---|---|---|
| **Fee calculation accuracy** | After-hours visits pay wrong investigator rate | Add working hours flag; drive fee engine from API (Phase 2) so calculation is tested and auditable |
| **Financial forecast integrity** | Multi-arm studies inflate backlog by summing all arms | Arm-assignment per patient at randomization; backlog recalculates from assigned arm only |
| **Revenue recognition controls** | Screen fail allotments not tracked; gastroenterology risk unmanaged | Screen fail counter per study per contract; billable vs. over-allotment split in PNL |
| **Investigator transparency** | Manual monthly spec export and email | Automate: generate spec from approved visits → email each investigator on schedule |
| **Time normatives** | No visit duration baseline exists | Derive normita from visit budget (investigator + procedure budget ÷ configured €/hr rate); expose in coordinator view and Power BI |

#### Tier 2 — Foundation (Months 3–5): Enable the Next Order of Magnitude
These capabilities do not exist today and are required before the platform can handle 10x the current volume or a second country.

| Capability | Why It's a Scaling Blocker | Recommendation |
|---|---|---|
| **API layer** | Without it, no integration, no mobile, no external access, no proper business logic versioning | Build REST API (Node.js/FastAPI) exposing all core entities; migrate fee calculation logic out of Power Apps formulas into tested API functions |
| **Role-based access control (RBAC)** | Current binary admin/non-admin model cannot support multi-site, multi-country data isolation | Implement 6-tier role model: Platform Admin → Country Admin → Site Admin → Coordinator → Investigator → Read-Only; enforce at API layer |
| **Multi-tenant data model** | All sites share a single data namespace; a second country requires either a full schema redesign or separate environments | Introduce `tenant_id` / `country_id` / `site_id` columns throughout; enforce row-level security at API and database layer |
| **PII audit logging** | No record of who accessed patient data — GDPR violation in most expansion markets | Every patient data read/write logged: user, timestamp, action, record; retained per regulatory requirements of each country |
| **Procedure-level visit itemization** | Unscheduled/partial visits cannot be itemized; CROs want to pay only for what happened | Add procedure sub-items on visits; each procedure has its own budget, completion flag, and billing status; resolves Drew's concern about unscheduled visit billing |

#### Tier 3 — Growth (Months 6–14): Enable Multi-Country and Ecosystem Integration
Once the foundation is solid, these capabilities unlock the next revenue tier.

| Capability | Scaling Value | Recommendation |
|---|---|---|
| **Multi-currency engine** | Required for every non-Croatia site | Per-site currency config; live FX rates (ECB API); historical rate preservation for accurate historical reporting; all Power BI dashboards show local + base currency |
| **EDC bidirectional sync** | Eliminates double-entry burden that grows linearly with study count | Build thin adapter per EDC system (Medidata, Veeva, REDCap); sync visit status bidirectionally; do not replicate clinical data — only operational/financial status |
| **Automated invoicing** | Billing is currently manual; at 5 countries it is unmanageable | Invoice auto-generated when visit reaches Approved; PDF created; delivered to CRO with line-item breakdown; status tracked through to payment receipt |
| **Mobile app (investigator-facing)** | Field investigators cannot log visits offline; a blocker for rural/hospital sites in new markets | React Native app: view schedule, check in/out of visit, log visit offline, sync when connected; push notifications for upcoming visits |
| **Document management** | Protocol amendments, consent forms, ethics approvals managed via email/paper | Version-controlled document store per study; expiry tracking for ethics/insurance; e-signature for coordinator/investigator acknowledgment of amendments |
| **Amendment workflow** | Currently handled by "effective from" date — no formal workflow | Structured amendment process: CRO submits amendment → Solimed reviews → approves → system applies effective-from date changes across all affected visits/budgets; full audit trail |

#### Tier 4 — Scale (Months 15–20): Platform-Level Capabilities
These are capabilities that transform Solimed from an operational tool into a platform.

| Capability | Strategic Value | Recommendation |
|---|---|---|
| **Self-service site onboarding** | Manual onboarding cannot scale past 10 sites | Guided wizard: site details → CRO mapping → protocol setup → user provisioning → first study — no Solimed staff intervention required |
| **CRO portal** | CROs currently receive data via email/spec exports | CRO-facing read-only view of their studies across all Solimed sites; live visit status, backlog, payment specs; replaces email reporting entirely |
| **AI-driven visit scheduling optimization** | Coordinator time is the most constrained resource | ML model trained on historical visit data: recommend optimal visit scheduling to minimize coordinator hours while respecting tolerance windows; flag high-risk patients likely to miss visits |
| **Predictive budget overrun detection** | Studies go over budget silently | Early warning model: flag studies where actual spend trajectory will exceed contracted budget before it happens; give coordinator and finance 30+ days to intervene |
| **Regulatory intelligence feed** | Country-specific rules change; currently no monitoring | Subscribe to regulatory update service per country; surface alerts when a change affects active studies; map change to affected visit types, budget items, or required fields |

---

### 9.2 Technical Capacity Scaling

These are the infrastructure and architecture decisions that determine how many users, sites, studies, and countries the platform can support before it breaks.

#### Current Capacity Ceiling (estimated)
| Dimension | Current | Estimated Power Apps Ceiling | Required for 10-Country Target |
|---|---|---|---|
| Active patients | 321 | ~2,000–3,000 (before gallery performance degrades) | 50,000+ |
| Concurrent users | ~10–20 | ~50 (Power Apps shared capacity) | 500+ |
| Sites | 2 | ~10 (before multi-env management becomes unmanageable) | 100+ |
| Countries | 1 | 1 (no localization, no data residency) | 10+ |
| Studies | Hundreds | ~1,000 (manageable) | 10,000+ |
| Report refresh latency | Minutes (scheduled) | Minutes (acceptable now) | Near real-time for operational dashboards |

#### Scaling Path by Phase

**Phase 1 (current Power Apps):**
- Optimize gallery delegation to push filtering server-side (reduces data loaded client-side)
- Add Power Apps staging environment to reduce risk of production incidents
- No architectural changes — capacity ceiling stays the same

**Phase 2 (API layer introduced):**
- API layer (Azure App Service) scales horizontally — add instances behind a load balancer as load increases
- Azure SQL scales vertically (upgrade tier) and horizontally (read replicas for Power BI data model)
- Estimated new ceiling: **10,000+ active patients, 100+ concurrent users, 20+ sites**
- Power Apps still the UI but reads/writes through the API — offloads business logic from client

**Phase 3 (multi-tenant, multi-region):**
- Per-country Azure resource groups — each country's data in the correct Azure region
- Azure Traffic Manager routes users to nearest region — reduces latency for international coordinators
- Database: single schema, row-level security per tenant; optionally shard by country at high volume
- Estimated new ceiling: **unlimited sites, 50+ countries, 1,000+ concurrent users**

**Phase 4 (Next.js front-end):**
- Next.js deployed to Azure Static Web Apps (global CDN) — sub-second page loads worldwide
- React Native mobile adds offline capacity — coordinators and investigators work without connectivity
- Power BI Embedded replaces per-user Power BI Pro licenses — significant cost reduction at scale
- Estimated cost reduction: from ~€25/user/month (Power Apps premium) to ~€3–5/user/month equivalent

**Phase 5 (platform scale):**
- Azure Kubernetes Service (AKS) for API layer — auto-scales to demand; zero-downtime deployments
- Azure Service Bus for async processing — visit approval events → invoice generation → notifications all handled asynchronously, no UI latency
- Azure Cognitive Search for cross-study, cross-country search at full dataset scale
- Target: **99.9% uptime SLA, <500ms API response p95, <2s page load globally**

---

### 9.3 Operational Capability Scaling

The technology scaling is only half the picture. The team and processes running the platform must scale in parallel or the technical investment is wasted.

#### Development Team Scaling

| Phase | Team Size | Key Additions |
|---|---|---|
| Phase 1 | 3–4 people | Power Apps dev, Power BI dev, QA, PM |
| Phase 2 | 5–6 people | Add backend API developer; retain Power Apps dev through transition |
| Phase 3 | 6–8 people | Add second full-stack dev; add DevOps/infrastructure engineer |
| Phase 4 | 8–10 people | Add mobile developer; UX designer full-time; data engineer |
| Phase 5 | 10–14 people | Add ML engineer; second mobile dev; security engineer; support engineer |

#### Process Maturity Scaling

| Process | Current State | Phase 2 Target | Phase 5 Target |
|---|---|---|---|
| Testing | None | Unit tests on all API business logic (>80% coverage); regression suite | Full test pyramid: unit, integration, E2E; automated performance tests |
| Deployment | Direct to production | Staging → UAT → Production pipeline with approvals | Blue/green deployments; feature flags; automated rollback |
| Monitoring | None | Azure Monitor: API uptime, error rates, response times | Full observability: distributed tracing, custom business metrics, SLO dashboards |
| Incident response | None | On-call rotation; runbook per alert type | SLA-driven incident management; post-mortem culture |
| Security | Azure AD only | Pen test before Phase 3; RBAC enforced; PII audit logs | Annual pen test; SOC 2 Type II target for enterprise CRO clients |
| Documentation | None (in Mladen's head) | API docs (OpenAPI); data model ERD; runbooks | Full developer portal; architecture decision records (ADRs) |

#### Support Model Scaling

| Scale | Support Model |
|---|---|
| 1–2 sites (today) | Mladen handles everything; informal |
| 3–10 sites (Phase 3) | Dedicated support inbox; documented escalation path; SLA: respond in 4 business hours |
| 10–50 sites (Phase 4) | Tiered support: L1 (self-service knowledge base), L2 (support engineer), L3 (dev escalation); SLA: P1 = 1hr, P2 = 4hr, P3 = 24hr |
| 50+ sites (Phase 5) | In-app support chat; customer success manager per country; automated monitoring catches issues before users report them |

---

### 9.4 Geographic Capability Scaling

This is the dimension most specific to Solimed's expansion ambition — scaling from 1 country to many.

#### Country Readiness Framework

Before launching in any new country, the following must be true:

| Gate | Check | Owner |
|---|---|---|
| **Regulatory** | Local clinical trial site regulations reviewed; required data fields identified; data residency requirement confirmed | Solimed Legal + Pivot |
| **Technical** | Azure region available in/near country; data residency config deployed; per-country tax/billing rules configured | Pivot DevOps |
| **Compliance** | RBAC enforced; PII audit logging active; GDPR or local equivalent data processing agreement in place | Pivot Security + Solimed Legal |
| **Localization** | UI language available; date/number/currency formats correct; required field labels in local language | Pivot Dev |
| **Financial** | Currency configured; FX rate integration active; invoicing format meets local requirements | Pivot Dev + Solimed Finance |
| **Operational** | Site admin user provisioned; coordinators trained; support escalation path defined for local time zone | Solimed Ops + Pivot PM |
| **Go-live** | Pilot study identified; test run complete on staging; hypercare plan in place for first 30 days | Pivot PM + Solimed |

#### Country Complexity Tiers

Not all countries are equally complex to enter. Classify target countries before committing:

| Tier | Description | Examples | Estimated Onboarding Time |
|---|---|---|---|
| **Tier 1 — Low complexity** | EU member state; GDPR already handled by base platform; Euro currency; English secondary language | Slovenia, Austria, Czech Republic | 2–4 weeks |
| **Tier 2 — Medium complexity** | EU-adjacent or similar regulatory framework; own currency (FX required); local language required | Serbia, Bosnia, Poland, Hungary | 4–8 weeks |
| **Tier 3 — High complexity** | Non-EU; distinct regulatory framework; non-Latin script or major language difference; data residency requirements | Turkey, UAE, Saudi Arabia | 8–16 weeks |
| **Tier 4 — Very high complexity** | US (HIPAA); China (data sovereignty); heavily regulated markets | USA, China, India | 16–26 weeks + legal counsel |

**Recommendation for first expansion:** Select a Tier 1 country. The fastest path to proving the international model is a country where GDPR is already handled (EU), Euro is the currency (no FX integration needed), and regulatory requirements are familiar. Slovenia or Austria are natural first candidates given Solimed's existing Croatian base.

#### Country Scaling Architecture

```
Global (Azure Traffic Manager)
├── Croatia (West Europe region) — existing
│   ├── Solimed Clinic
│   └── Medico RI
├── Country 2 (appropriate Azure region)
│   └── Site(s)
├── Country 3 (appropriate Azure region)
│   └── Site(s)
└── ...

Shared services (single region, non-PII):
├── Authentication (Azure AD B2C — global)
├── Power BI Embedded (global)
└── Platform admin console
```

Each country is an isolated tenant:
- Separate database schema (row-level security by `country_id`) or separate database instance (for strict data residency)
- Separate Azure resource group in the correct region
- Country-specific configuration: language, currency, tax rules, required fields, regulatory flags
- Shared codebase — configuration drives behavior, not code branches

#### Revenue Model for Country Scaling

As each country goes live, the platform should generate incremental revenue from that country's sites without incremental development cost. The target model:

| Model | Description | When to Use |
|---|---|---|
| **Managed service** (current) | Solimed uses the platform for their own sites; no external licensing | Phases 1–3 |
| **White-label to partner sites** | Other SMOs in new countries license the platform under their brand | Phase 3–4 |
| **SaaS subscription** | Sites and CROs pay per-study or per-site subscription | Phase 4–5 |
| **CRO portal licensing** | CROs pay for read-only access to their data across all Solimed sites | Phase 5 |

Ivan and Drew's comment during the demo — *"Do you guys charge for the app?" "Currently no." "That needs to change."* — validates that the SaaS licensing model is already on the roadmap in Solimed's thinking. The technical architecture recommended here is explicitly designed to support it.

---

### 9.5 Scaling Capability Summary

| Capability Dimension | Biggest Bottleneck Today | Phase 2 Unlock | Phase 5 Target State |
|---|---|---|---|
| **Functional** | Multi-arm backlog; fee accuracy; no procedure drill-down | API layer centralizes and tests all business logic | Full EDC sync, automated invoicing, AI scheduling optimization |
| **Technical capacity** | Power Apps gallery performance; no multi-tenancy | API layer + Azure SQL handle 10x current volume | AKS auto-scale; 99.9% uptime; <500ms API globally |
| **Operational** | Single developer; no tests; no staging; no monitoring | CI/CD pipeline; test suite; staging env; Azure Monitor | Full observability; tiered support; SOC 2 Type II |
| **Geographic** | 1 country; no localization; no data residency | Multi-tenant schema; country config layer | 10+ countries; self-serve onboarding; <2 weeks per new country |
