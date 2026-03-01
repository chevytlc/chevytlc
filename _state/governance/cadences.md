# Governance Cadences

**Date:** 2026-03-01
**Owner:** Chevy (CTOO)

## Refresh and Review Schedule

### Monthly: Document Currency Review
- **What:** Review all uploaded carrier documents in Claude Teams Projects against current carrier documentation
- **Who:** Content owner per Project (Tom for UW Knowledge Base, Chevy for Governance and Dev)
- **Output:** Update or flag stale documents; update manifests in `_state/projects/manifests/`
- **Trigger:** First business day of each month

### Quarterly: Drift Detection Review
- **What:** Compare Claude Teams Project content (custom instructions, uploaded docs, observed behavior) against BEACON source-of-truth and carrier source docs
- **Who:** Chevy (CTOO) with content owners
- **Output:** Drift report documenting any discrepancies; remediation actions assigned
- **Trigger:** End of Q1, Q2, Q3, Q4
- **Note:** Claude Teams cannot self-detect drift. This is a manual governance process.

### Continuous: Decision Records
- **What:** Any architectural or operational decision captured as a DR-XXX in `_state/decisions/`
- **Who:** Decision maker, with CTOO review
- **Output:** Decision Record file committed to repo
- **Trigger:** When a decision is made

### As-Needed: Custom Instruction Updates
- **What:** Changes to Claude Teams Project custom instructions
- **Process:**
  1. Document new/changed instruction in `_state/projects/instructions/`
  2. Review against BEACON governance principles
  3. Deploy to Claude Teams platform
  4. Confirm deployment matches documented instruction
- **Who:** Content owner proposes, CTOO approves

## SDLC Triad Compensation

Claude Teams scores 0/3 on the self-documenting SDLC Triad:
- No self-documentation
- No self-testing
- No drift detection

Governance cadences above compensate for these gaps through manual processes. This overhead is acceptable at current team scale (5-7 people) but should be reassessed if team grows beyond 10.

## Onboarding Integration

Claude Teams is ONE onboarding component. Complete onboarding includes:

1. **BEACON orientation** -- governance framework, source-of-truth hierarchy, decision records
2. **Federato training** -- PAS workflows, binding authority, compliance controls
3. **FSIS workflow mentorship** -- Loop 3, person-to-person training for feedback-embedded UW (cannot be automated)
4. **Claude Teams access** -- Project-appropriate access, custom instruction awareness
5. **Supervised decision-making** -- Ramp period with CUO oversight before independent authority
