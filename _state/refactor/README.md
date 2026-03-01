# BEACON Agentic Refactor — Requirements, Forms, and Schema

**Version:** 1.0.0
**Date:** 2026-03-01
**Owner:** CTOO
**Status:** Phase 1 — Initial extraction from meta dictionary and rule artifact specification

---

## What This Is

Structured, agent-parseable YAML artifacts for:

1. **Schema** — Field-level index extracted from Meta Dictionary v5 (172 fields across 14 Federato entities). This is the shared dependency that rules and forms import.
2. **Requirements** — Underwriting rules as `UWR-XXX.yaml` files. Each rule is one file, references fields by `Entity.attribute`, includes source traceability, enforcement tier, and Federato mapping.
3. **Forms** — Form specifications as `FORM-XXX.yaml` files. Each form is one file, lists which fields it captures, which rules it triggers, and which enrichment it initiates.

## Design Principles

### One file = one artifact = no merge conflicts

Every rule, form, and schema entity is a separate YAML file. Multi-dev teams can work on different rules/forms simultaneously without merge conflicts. Agents can read/write individual artifacts without loading the full knowledge base.

### Field references are canonical

All references to data fields use the format `Entity.attribute` (e.g., `Building.year_built`, `Location.state`). These resolve against `schema/field-index.yaml`, which is extracted from the canonical-reference.json in the beacon contracts layer.

### Source priority is embedded

Every field reference includes its source priority (P1 Submission > P2 Nearmap > CPSF > Conditional E2Value > Downstream AIR). Rules that use multi-source fields include resolution logic that matches the meta dictionary's conflict rules.

### Rules reference rules

Rules declare `sequential_dependencies` listing which rules must evaluate first. This creates an implicit DAG that agents can traverse to determine evaluation order.

### Forms reference rules

Forms declare `triggers_rules` listing which UWR rules fire on the form's data. This creates the form → rule → field traceability chain.

## Directory Structure

```
_state/refactor/
├── README.md                         # This file
├── schema/
│   └── field-index.yaml              # Agentic field index (from meta dictionary)
├── requirements/
│   ├── UWR-ELIG-001.yaml             # Alaska/Hawaii exclusion (Hard Block)
│   ├── UWR-ELIG-002.yaml             # Minimum TIV threshold (Hard Block)
│   ├── UWR-BLDG-001.yaml             # Year built eligibility (Gated Referral)
│   ├── UWR-BLDG-002.yaml             # Construction type eligibility (Gated Referral)
│   ├── UWR-LIMIT-001.yaml            # Per-location/occurrence limits (Hard Block)
│   ├── UWR-CAT-001.yaml              # CAT exposure scoring (Gated Referral)
│   └── UWR-DATA-001.yaml             # Submission completeness (Gated Referral)
└── forms/
    ├── FORM-CP-001.yaml              # Commercial property application
    └── FORM-DW-001.yaml              # Dwelling property application
```

## Rule Inventory

| Rule ID | Title | Tier | Domain | Key Fields | Status |
|---------|-------|------|--------|------------|--------|
| ELIG-001 | Alaska/Hawaii Exclusion | Hard Block | appetite | Location.state | Active |
| ELIG-002 | Minimum TIV | Hard Block | appetite | Policy.tiv | Draft |
| BLDG-001 | Year Built Eligibility | Gated Referral | appetite | Building.year_built | Draft |
| BLDG-002 | Construction Type | Gated Referral | appetite | Building.construction_type | Draft |
| LIMIT-001 | Per-Location/Occurrence Limits | Hard Block | pricing | Building.building_limit, Policy.tiv | Draft |
| CAT-001 | CAT Exposure Scoring | Gated Referral | pricing | Building.earthquake_risk_score, Building.flood_risk_score | Draft |
| DATA-001 | Submission Completeness | Gated Referral | compliance | 13 critical fields | Draft |

## Form Inventory

| Form ID | Title | Type | Selection Field | Status |
|---------|-------|------|-----------------|--------|
| CP-001 | Commercial Property Application | application | Policy.policy_type = "Commercial Property" | Draft |
| DW-001 | Dwelling Property Application | application | Policy.policy_type = "Dwelling Property" | Draft |

## Rule Evaluation DAG

```
Submission Ingestion:
  UWR-DATA-001 (completeness check)
  └─→ UWR-ELIG-001 (geography)
      └─→ UWR-ELIG-002 (minimum TIV)

Risk Assessment:
  UWR-BLDG-001 (year built)
  UWR-BLDG-002 (construction type)
  UWR-LIMIT-001 (limit boundaries + ITV)
  └─→ UWR-CAT-001 (CAT exposure scoring)
```

## How Agents Use This

### Reading a rule
```
Agent reads: requirements/UWR-ELIG-001.yaml
Agent extracts: rule.logic, data_requirements.fields
Agent resolves field types via: schema/field-index.yaml
Agent knows enforcement tier: beacon_alignment.enforcement_tier
```

### Evaluating a submission
```
1. Load form spec (e.g., FORM-CP-001.yaml)
2. Identify triggers_rules list
3. Load each rule's YAML
4. Evaluate rules in DAG order (sequential_dependencies)
5. For each field, resolve source priority from field-index.yaml
6. Apply rule logic
7. Return: [decline | referral | advisory | pass]
```

### Creating a new rule
```
1. Copy any existing UWR-XXX.yaml as template
2. Assign new ID per prefix convention (ELIG, BLDG, LIMIT, DED, COV, VAL, AGG, OPS, DATA, BI, PC, CAT)
3. Reference fields from schema/field-index.yaml
4. Declare sequential_dependencies
5. Add to triggers_rules in relevant form YAML
```

## Relationship to BEACON

- **field-index.yaml** is a compact projection of `contracts/schemas/canonical-reference.json`
- **UWR-XXX.yaml** follows the structure from `framework/rule-artifact-specification.md`
- **FORM-XXX.yaml** is the new artifact type designed for this refactor (no prior equivalent in BEACON)
- This directory lives in `chevytlc` (CTOO personal corpus) until validated, then promotes to `lrmga-knowledge`

## Next Steps

1. **Populate CARRIER_PARAM values** from binding authority agreements (requires DA access)
2. **Create remaining rule artifacts** for the 100+ rules identified in the rule taxonomy
3. **Add endorsement/binder/policy forms** (FORM-CP-002 through FORM-XX-NNN)
4. **Connect to Federato rule IDs** once FED-02/FED-03 form builder config completes
5. **Validate against Federato field names** with IaC-DICT-001/002 data dictionary outputs
6. **Promote to lrmga-knowledge** once CUO validates rule content
