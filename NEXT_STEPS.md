# NEXT STEPS — Pivot

**Last Updated:** 2026-04-02
**Current Branch:** Kremer-dev

---

## Immediate Priorities

### 1. Solimed Engagement — Commercial Close ⏳ Urgent
- [ ] Send NDA to Ivan Kruljac for signature (BLOCKER — should have been done before demo)
- [ ] Get verbal Phase 1 budget confirmation from Ivan (~€38K–€50K)
- [ ] Draft and send Phase 1 SOW for legal review
- [ ] Clarify WTE Solutions / Eric Garrison engagement status with Ivan
- [ ] Confirm Mladen Geng 2 days/week availability for Sprints 1–2

### 2. Solimed Engagement — Pre-Sprint 1 Access ⏳ Before Sprint 1
- [ ] Provision Pivot with Power Apps read/admin access
- [ ] Provision Pivot with Power BI workspace access
- [ ] Provision Pivot with Dataverse read access
- [ ] Confirm Azure subscription status
- [ ] Identify 2–3 coordinator names for Phase 1 UAT

### 3. Meridian Platform — Internal ⏳ Ongoing
- [ ] Confirm second Full-Stack Developer for Sprint 1 (critical to 18-month plan)
- [ ] Begin second site outreach by Month 4 (Drew's network / Solimed CRO referrals)
- [ ] Architecture decisions (Phase 0 Week 2): FastAPI vs Fastify, Auth0 vs Azure AD B2C
- [ ] GitHub repo created: `pivot-meridian`
- [ ] Decide on project management tool: Linear vs Jira

---

## Key Decisions Made This Session

| Decision | Outcome |
|---|---|
| Platform name | **Meridian** |
| Build timeline | **18 months** (compressed from 24) |
| Compression strategy | Phase 0 parallel to Sprint 1; Phase 1 = Layers 1–3 MVP; 2nd dev from Sprint 1; Layers 4–5 in Phase 2 |
| Claude Code | Use throughout — saves ~4–6 sprint-weeks total; does not change 18-month timeline |
| Timeline bottlenecks | Mladen knowledge transfer, coordinator UAT cycles, second site BD, multi-tenancy correctness |

---

## Recently Completed

| Date | Item |
|---|---|
| 2026-04-02 | ✅ Meridian PM Plan v2.0 — compressed 18-month plan (MD + DOCX) |
| 2026-04-02 | ✅ Claude Code feasibility assessment for Meridian build |
| 2026-04-02 | ✅ Solimed Enhancement Roadmap (MD + DOCX) |
| 2026-04-02 | ✅ Solimed PM Plan — Phase 1 full sprint plan (MD + DOCX) |
| 2026-04-02 | ✅ Solimed Proposal — €476K–€595K across 5 phases (MD + DOCX) |
| 2026-04-02 | ✅ Solimed CTO Technology Assessment (MD + DOCX) |
| 2026-04-02 | ✅ Solimed CTO Draft Gap Analysis vs Eric Garrison/WTE Solutions (MD + DOCX) |
| 2026-04-02 | ✅ Solimed CFO Revenue Intelligence — tailored to Drew Domescik (MD + DOCX) |
| 2026-04-02 | ✅ Solimed Project Charter (MD + DOCX) |
| 2026-04-02 | ✅ Solimed Readiness Assessment — CONDITIONALLY READY (MD + DOCX) |
| 2026-04-02 | ✅ Solimed As-Is Process Maps — 8 flowcharts, 20 gaps (MD + DOCX) |
| 2026-04-02 | ✅ Meridian Platform Vision (MD + DOCX) |

---

## Blocked / On Hold

- **Solimed Sprint 1 start** — blocked on NDA, budget confirmation, and access provisioning
- **Meridian Phase 0** — blocked on second developer hire and internal team formation

---

## Notes

- All Solimed deliverables: `docs/reports/solimed-*.md`
- Meridian deliverables: `docs/reports/meridian-*.md`, `docs/reports/pivot-platform-vision.md`
- All work on `Kremer-dev` — never commit directly to `main`
