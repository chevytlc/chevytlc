# Claude Teams Project Architecture

**Date:** 2026-03-01
**Owner:** Chevy (CTOO)
**Status:** Draft -- pending team review
**Governs:** DR-001 (Claude Teams as convenience layer)

## Design Principles

1. **Cross-cutting over carrier-siloed**: Real UW workflows span carriers. Projects organized by function, not by carrier.
2. **Role-appropriate framing**: Custom instructions shape context per audience (dev team vs. UW team).
3. **Token budget awareness**: 200K token cap per Project (~500 pages). RAG extends reach but introduces retrieval variability.
4. **BEACON-first**: Every custom instruction documented in `_state/projects/instructions/` before deployed to platform.
5. **Named ownership**: Each Project has a named content owner responsible for currency.

## Project Definitions

### Project 1: UW Knowledge Base

**Purpose:** Cross-cutting underwriting reference for all carriers and lines.
**Audience:** Tom (CUO), Angel (Underwriter), future UW hires
**Content Owner:** Tom (CUO)
**Refresh Cadence:** Monthly against carrier source documents

**Content includes:**
- Carrier appetite guides (all carriers, cross-referenced)
- DA parameter summaries (deductibles, limits, excluded perils by carrier)
- Coverage form comparisons across carriers
- Cat modeling interpretation guidance
- Binding authority terms and conditions summaries
- E&S surplus lines compliance reference by state

**Custom instructions (draft):**
> You are a reference assistant for LRMGA underwriters. You help locate carrier appetite parameters, DA terms, coverage comparisons, and surplus lines requirements. You do NOT make binding decisions. You do NOT replace Federato. If asked about a specific risk decision, remind the user that Federato is the system of record for binding authority and compliance enforcement. Always cite which carrier document or DA section your answer references.

**Token budget estimate:** ~120-150K tokens (largest Project; may need selective loading)

---

### Project 2: BEACON Governance Reference

**Purpose:** Governance framework, decision records, operational standards.
**Audience:** All team members (especially Cali, Daniel during onboarding)
**Content Owner:** Chevy (CTOO)
**Refresh Cadence:** Continuous (mirrors _state/ directory)

**Content includes:**
- BEACON framework documentation
- Decision Records (DR-series)
- Source-of-truth hierarchy
- Governance cadences and drift detection procedures
- Team roles and responsibilities
- Onboarding checklists and workflows

**Custom instructions (draft):**
> You are a reference assistant for LRMGA governance and operational standards. You answer questions about BEACON framework decisions, team structure, governance cadences, and source-of-truth hierarchy. The canonical source is always the _state/ directory in the beacon-pin repo. If you are unsure about currency, say so and recommend checking the repo directly.

**Token budget estimate:** ~30-50K tokens (compact, mostly structured docs)

---

### Project 3: Dev & Agentic Framework

**Purpose:** Technical reference for the development team working on the agentic framework refactor.
**Audience:** Chevy (CTOO), Cali (UW Tech), Daniel (Technical PM)
**Content Owner:** Chevy (CTOO), transitioning to Cali for implementation artifacts
**Refresh Cadence:** As development progresses

**Content includes:**
- Agentic framework architecture and specifications
- UW requirements in multi-dev/multi-agent format
- Federato API integration patterns
- beacon-pin repo conventions and contribution guidelines
- Technical decision records specific to implementation

**Custom instructions (draft):**
> You are a technical reference assistant for the LRMGA development team. You help with agentic framework architecture, Federato integration patterns, and UW requirement specifications. You understand the multi-dev/multi-agent workflow model. Code-level questions should reference the beacon-pin repo. Architecture-level questions should reference BEACON Decision Records.

**Token budget estimate:** ~60-80K tokens (grows with implementation artifacts)

---

### Project 4: Onboarding (Temporary/Rotating)

**Purpose:** Consolidated onboarding resource, role-specific, used during ramp periods.
**Audience:** New hires (currently Cali, Angel; later Daniel, UW Director, UW Assistant)
**Content Owner:** Chevy (CTOO) initially, delegated per role
**Refresh Cadence:** Rebuilt per onboarding cohort

**Content includes:**
- Role-specific onboarding checklist
- "First 30/60/90 days" guidance
- Key contacts, tools, and access provisioning
- Federato training prerequisites and workflow
- FSIS context (what it is, why it matters, how it informs UW decisions)
- Pointers into Projects 1-3 for deeper reference

**Custom instructions (draft):**
> You are an onboarding assistant for new LRMGA team members. You help navigate the first weeks: understanding BEACON governance, getting oriented with Federato, and learning the team structure. You are a starting point, not a substitute for training. For UW-specific questions, direct users to the UW Knowledge Base project. For technical questions, direct to the Dev & Agentic Framework project. Always remind new hires that Federato is the system of record and BEACON is the governance authority.

**Token budget estimate:** ~20-40K tokens (lean, high-signal)

## Token Budget Summary

| Project | Est. Tokens | % of 200K Cap | Risk |
|---------|------------|---------------|------|
| UW Knowledge Base | 120-150K | 60-75% | May hit cap; prioritize most-referenced docs |
| BEACON Governance | 30-50K | 15-25% | Low risk |
| Dev & Agentic Framework | 60-80K | 30-40% | Grows; monitor as implementation progresses |
| Onboarding | 20-40K | 10-20% | Low risk; rebuilt per cohort |

**Note:** RAG expansion on paid plans extends effective capacity up to 10x, but retrieval quality becomes variable for less-structured content. Structured documents (tables, clear headings) retrieve better.

## Rejected: One Project Per Carrier

Carrier-siloed Projects would:
- Fragment cross-cutting UW workflows (e.g., comparing DA parameters across carriers)
- Create redundant governance overhead (N content owners instead of 1 for UW knowledge)
- Waste token budget on repeated boilerplate per Project
- Not reflect how underwriters actually work (across multiple carriers simultaneously)

## Governance Cross-Reference

- Custom instructions: Documented in `_state/projects/instructions/` before platform deployment
- Content manifests: Each Project maintains a list of uploaded documents in `_state/projects/manifests/`
- Drift detection: Quarterly comparison of Project content against `_state/` and carrier source docs
