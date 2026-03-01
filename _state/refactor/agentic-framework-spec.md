# Agentic Framework Refactor Specification

**Date:** 2026-03-01
**Owner:** Chevy (CTOO)
**Status:** Draft -- architecture phase
**Priority:** Higher than Claude Teams Project setup (per session decision)
**Classification:** Alpha (architecture) with Inference (execution)

## Problem Statement

The initial round of UW requirements has been submitted to Federato in a format optimized for human-to-human handoff. As LRMGA scales and the dev team grows (Cali, Daniel joining), these requirements need to be refactored into a format that supports:

1. **Multi-developer workflows**: Multiple devs working on different requirement streams without merge conflicts or ambiguity
2. **Multi-agent execution**: Claude Code and other agentic tools can parse, implement, and validate requirements without human intermediary interpretation
3. **Traceability**: Every requirement traces back to a carrier DA, BEACON governance decision, or business rule
4. **Testability**: Requirements expressed in a form that generates verifiable acceptance criteria

## Current State

- UW requirements submitted to Federato in narrative/document format
- Requirements encode business logic that needs to live in BEACON as source of truth
- Format does not support parallel development or automated validation
- No structured mapping between requirements and their carrier/DA source

## Target State

Each UW requirement is expressed as a structured artifact with:

```yaml
requirement_id: UWR-XXX
title: [Short descriptive title]
source:
  type: [carrier_da | beacon_governance | business_rule | regulatory]
  reference: [Specific document/section/clause]
  carrier: [Carrier name, if applicable]
domain: [appetite | pricing | binding | compliance | reporting]
rule:
  description: [Human-readable rule statement]
  logic: [Structured logic expression or pseudocode]
  parameters:
    - name: [parameter name]
      type: [string | numeric | enum | boolean]
      values: [allowed values or range]
      source: [where this parameter comes from]
federato_mapping:
  module: [Which Federato module implements this]
  field: [Specific field or rule in Federato]
  status: [submitted | confirmed | implemented | validated]
acceptance_criteria:
  - [Testable condition 1]
  - [Testable condition 2]
dependencies: [List of other UWR-XXX this depends on]
owner: [Who owns this requirement's currency]
last_reviewed: [Date]
```

## Refactor Phases

### Phase 1: Inventory and Classify (Target: Week of 2026-03-02)
- Extract all UW requirements from initial Federato submission
- Assign requirement IDs (UWR-series)
- Classify by domain (appetite, pricing, binding, compliance, reporting)
- Map each to its carrier/DA source
- **Owner:** Chevy, with Cali assisting on extraction

### Phase 2: Structured Format Conversion (Target: Weeks of 2026-03-09 to 2026-03-23)
- Convert each requirement to YAML structured format
- Define Federato module mappings
- Write acceptance criteria per requirement
- Establish traceability links
- **Owner:** Cali (execution), Chevy (architecture review)

### Phase 3: Multi-Agent Validation (Target: Week of 2026-03-23)
- Validate that structured requirements can be parsed by Claude Code
- Test requirement-to-implementation workflow with agentic tools
- Identify gaps in structure that block automated execution
- **Owner:** Chevy, with Daniel joining 2026-03-16

### Phase 4: Federato Reconciliation (Target: Pre-launch, before 2026-04-01)
- Reconcile structured requirements against what Federato has implemented
- Flag discrepancies between BEACON source-of-truth and Federato execution
- Confirm every requirement has a validated Federato implementation
- **Owner:** Daniel (coordination), Tom (UW validation)

## Directory Structure

```
_state/refactor/
  agentic-framework-spec.md    This specification
  requirements/
    UWR-001.yaml               Individual requirement files
    UWR-002.yaml
    ...
  inventory.md                 Master inventory and classification
  federato-mapping.md          Cross-reference: requirements to Federato modules
```

## Multi-Dev Workflow

### Branching Convention
- Feature branches: `feature/UWR-XXX-short-description`
- Requirement changes: One requirement per commit where possible
- Review: All requirement changes reviewed by CTOO or CUO before merge

### Conflict Avoidance
- Each requirement is a separate file (UWR-XXX.yaml)
- No monolithic requirements document
- Domain owners can work in parallel without merge conflicts
- Inventory and mapping files are append-only where possible

## Multi-Agent Execution Model

### Agent Capabilities Required
1. **Parse**: Read UWR YAML and extract structured fields
2. **Implement**: Generate Federato configuration or code from structured requirements
3. **Validate**: Check implementation against acceptance criteria
4. **Report**: Flag requirements with status gaps (e.g., submitted but not confirmed)

### Agent Boundaries
- Agents implement what BEACON specifies. Agents do not make business logic decisions.
- Agents flag ambiguity for human resolution. Agents do not resolve ambiguity independently.
- Agent outputs are validated against acceptance criteria before deployment.

## Dependencies

- Federato API access for reconciliation (Phase 4)
- Cali onboarded and oriented to BEACON (starts 2026-03-02)
- Daniel onboarded for coordination role (starts 2026-03-16)
- Initial UW requirements document from Federato submission (existing)

## Success Criteria

1. 100% of submitted UW requirements have a structured UWR-XXX file in `_state/refactor/requirements/`
2. Every UWR-XXX traces to a carrier/DA source or BEACON decision
3. Every UWR-XXX has testable acceptance criteria
4. Claude Code can parse any UWR-XXX and generate a Federato implementation scaffold
5. Pre-launch reconciliation confirms Federato implementation matches BEACON source-of-truth
