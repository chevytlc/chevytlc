# Project 1: UW Knowledge Base — Content Manifest

**Version:** 1.0
**Date:** 2026-03-02
**Content Owner:** Tom (CUO)
**Refresh Cadence:** Monthly (first business day)
**Token Budget:** ~120-150K of 200K cap

---

## Uploaded Documents

| # | Document | Source | Status | Token Est. | Notes |
|---|----------|--------|--------|-----------|-------|
| 1 | Carrier appetite guides (all carriers) | Carrier portals | Pending upload | TBD | Cross-reference across carriers; Tom to compile |
| 2 | DA parameter summaries | Carrier DAs | Pending upload | TBD | Deductibles, limits, excluded perils, territory restrictions |
| 3 | Coverage form comparisons | Carrier forms library | Pending upload | TBD | Cross-carrier comparison tables preferred |
| 4 | Cat modeling interpretation guide | Internal / AIR / RMS docs | Pending upload | TBD | Guidance for interpreting model outputs in UW context |
| 5 | Binding authority terms & conditions | Carrier DAs | Pending upload | TBD | Summarized per carrier, thresholds highlighted |
| 6 | E&S surplus lines compliance reference | State DOI sources | Pending upload | TBD | By-state requirements for surplus lines placement |
| 7 | Underwriting rules reference (from BEACON) | _state/refactor/underwriting-rules.yaml | Pending upload | ~15K | 100 rules, all categories and enforcement tiers |
| 8 | Forms library reference (from BEACON) | _state/refactor/forms-library.yaml | Pending upload | ~12K | 78 forms, dispositions, readiness status |

## Token Budget Risk

This is the largest Project at 60-75% of the 200K cap. Mitigation:

- Prioritize most-referenced documents first
- Use structured formats (tables, clear headings) for better RAG retrieval
- Consider splitting large carrier docs into per-topic summaries rather than uploading raw PDFs
- Monitor retrieval quality after initial upload; adjust if answers degrade

## Upload Priority

1. **Immediate (Week 1):** Items 7-8 (already exist in BEACON; direct upload)
2. **Week 2:** Items 1-2 (carrier appetite + DA parameters — highest UW value)
3. **Week 3:** Items 3, 5 (coverage forms + binding authority)
4. **Week 4:** Items 4, 6 (cat modeling + surplus lines)

## Review Log

| Date | Reviewer | Action | Notes |
|------|----------|--------|-------|
| 2026-03-02 | Chevy | Created manifest | Initial setup; no docs uploaded yet |
