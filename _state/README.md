# BEACON State

BEACON: Behavior, Enrichment, Analytics, Control, Optimization, Network

This directory is the canonical governance state for Lighthouse Risk MGA (LRMGA). All business logic, architectural decisions, and governance artifacts are documented here first, then implemented in downstream systems.

## Source-of-Truth Hierarchy

1. **BEACON** (_state/) -- authoritative governance. All business logic documented here first.
2. **Federato** -- execution and enforcement. Deterministic rules, compliance controls, workflow.
3. **Claude Teams Projects** -- reference access only. Conversational interface to knowledge governed by BEACON and enforced by Federato.

## Directory Structure

```
_state/
  decisions/       Decision Records (DRs) -- architectural and operational decisions
  projects/        Claude Teams Project definitions and boundaries
  governance/      Governance framework, refresh cadences, drift detection
  team/            Team roster, roles, seat allocations
  refactor/        Agentic framework refactor specifications
```

## Governance Principles

- Named owner per artifact, responsible for currency
- Monthly doc review against carrier source documents
- Custom instructions documented in BEACON first, implemented in platform second
- Quarterly drift review (platforms cannot self-detect)
- All decisions require a Decision Record before implementation
