# BEACON Agentic Refactor — Rules, Forms, and Schema

**Version:** 2.0.0
**Date:** 2026-03-02
**Owner:** CTOO
**Status:** Phase 1 complete — All 100 rules and 78 forms extracted from Federato source files

---

## What This Is

Three agent-parseable YAML files extracted from the actual xlsx files sent to Federato:

1. **`schema/field-index.yaml`** — Field-level index from Meta Dictionary v5 (172 fields, 14 entities). Shared dependency that rules and forms reference.
2. **`underwriting-rules.yaml`** — All 100 UW rules in tabular format. Each rule includes logic expression, canonical Federato field mappings, enforcement tier, enrichment dependencies, and DA source citations.
3. **`forms-library.yaml`** — All 78 forms in tabular format. Each form includes disposition, condition logic with canonical field mappings, formset assignments, readiness gates, and CUO notes.

## Source Files

| Artifact | Source xlsx | Location |
|----------|-----------|----------|
| underwriting-rules.yaml | Lighthouse_Risk_Underwriting_Rules.xlsx | beacon/domains/ |
| forms-library.yaml | LRMGA Forms Library v1.xlsx | beacon/domains/ |
| field-index.yaml | meta-dictionary-v5.xlsx | beacon/contracts/ |

## Design Principles

### Tabular format preserves the spreadsheet structure

Rules and forms stay as arrays of records — the same layout as the xlsx. Humans can scan, filter, and compare. Agents iterate the array and filter by `category`, `type`, `disposition`, or `condition.canonical`.

### Field references are canonical

Rule and form references to data use `Entity.attribute` format (e.g., `Building.year_built`, `Location.state`). The `canonical_fields` array in each rule maps the original source field names to the Federato schema. Fields resolve against `schema/field-index.yaml`.

### Source priority is embedded in the schema

`field-index.yaml` carries the priority chain (P1 Submission > P2 Nearmap > CPSF > E2Value > Downstream AIR) and multi-source resolution rules for every field. Rules that use multi-source fields inherit these resolution rules.

### Rules reference rules

Rules with `depends_on` declare which rules must evaluate first, creating the evaluation DAG.

### Multi-dev multi-agent ready

Three files, clear boundaries. Schema owner, rules owner, and forms owner can work independently. Agents working on rules don't need to load forms, and vice versa. Merge conflicts are limited to the specific file being edited.

## Directory Structure

```
_state/refactor/
├── README.md                    # This file
├── schema/
│   └── field-index.yaml         # Agentic field index (from meta dictionary v5)
├── underwriting-rules.yaml      # 100 rules — tabular (from Federato xlsx)
└── forms-library.yaml           # 78 forms — tabular (from Federato xlsx)
```

## Rule Summary

**100 rules** across 8 categories:

| Category | Count | Types |
|----------|-------|-------|
| Geography (ELIG) | 2 | Hard Decline |
| Occupancy (ELIG) | 10 | Hard Decline |
| Building (ELIG/BLDG) | 19 | Hard Decline, Soft Decline, Conditional |
| Protection Class | 3 | Hard Decline, Soft Decline |
| Coverage Limits (LIMIT/COV) | 26 | Threshold, Soft Decline |
| Deductibles (DED) | 8 | Threshold |
| Aggregation (AGG) | 7 | Threshold |
| Operational (DATA/INSP/OPER/RATE) | 25 | Informational, Conditional |

**Enforcement tiers:**
- **Hard Decline (23):** Auto-decline, no override. Geography, prohibited occupancies, building conditions.
- **Soft Decline (8):** Referral with CUO/Leadership approval. Building age, protection class, coverage limits.
- **Threshold (24):** Limit/deductible boundaries. No HITL unless exceeded.
- **Conditional (15):** Context-dependent rules. Inspection requirements, coverage allocation.
- **Informational (14):** Advisory. Required fields, operational procedures.

## Form Summary

**78 forms** across 6 disposition types:

| Disposition | Count | Description |
|-------------|-------|-------------|
| Mandatory | 28 | Always attached. No UW discretion. |
| Conditional-Mandatory | 9 | Attached when condition met. No discretion once triggered. |
| Default-On/Removable | 12 | Attached by default. UW may remove with reason. |
| Default-Off/Addable | 12 | Not attached by default. UW may add. |
| TBD | 9 | Disposition not yet determined by CUO. |
| Excluded/Hold/Blocked | 8 | Not available for current phase. |

**Readiness (go-live):**
- Form Content Ready: 40 of 78
- Rule Logic Defined: 57 of 78
- Ready for Federato: 33 of 78
- Federato Configured: 0 of 78
- Tested/Validated: 0 of 78

## How Agents Use This

### Evaluating a submission against rules
```
1. Load underwriting-rules.yaml
2. Filter rules by lifecycle phase (Submission Ingestion first, then Risk Assessment)
3. For each rule, resolve canonical_fields against field-index.yaml
4. Evaluate rule.logic against submission data
5. Respect depends_on ordering (evaluation DAG)
6. Return: [decline | referral | advisory | pass] per rule
```

### Determining which forms to attach
```
1. Load forms-library.yaml
2. Filter forms by disposition = Mandatory (always attach)
3. For Conditional-Mandatory forms, evaluate condition against submission
4. For Default-On, attach unless UW explicitly removes
5. Resolve condition.canonical against field-index.yaml
```

### Adding a new rule
```
1. Open underwriting-rules.yaml
2. Append new record to rules[] array
3. Assign ID per prefix convention (ELIG, BLDG, LIMIT, DED, COV, VAL, AGG, OPS, DATA)
4. Map source_fields to canonical_fields using field-index.yaml
5. Set depends_on if evaluation order matters
```

## Relationship to BEACON

- **field-index.yaml** is a compact projection of `contracts/schemas/canonical-reference.json`
- **underwriting-rules.yaml** is the machine-readable version of the xlsx sent to Federato
- **forms-library.yaml** is the machine-readable version of the forms xlsx sent to Federato
- This directory lives in `chevytlc` until validated, then promotes to `lrmga-knowledge`

## Next Steps

1. **Map remaining [UNMAPPED] fields** — Some rule fields don't have canonical Federato mappings yet (e.g., `barrier_island_flag`, `systems_updated`, `soil_type`). These need IaC-DICT work or HazardHub field mapping.
2. **Connect formsets to rules** — Cross-reference which rules apply to which formset combinations.
3. **Populate Federato rule IDs** — Once FED-02/FED-03 form builder config completes.
4. **Validate readiness gates** — 0/78 forms are Federato-configured or tested.
5. **Promote to lrmga-knowledge** once CUO validates.
