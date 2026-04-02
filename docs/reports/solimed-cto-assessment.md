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
