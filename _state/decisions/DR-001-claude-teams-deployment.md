# DR-001: Claude Teams Deployment as Knowledge Access Convenience Layer

**Date:** 2026-03-01
**Status:** Approved
**Decision Owner:** Chevy (CTOO)
**Participants:** Aaron Garcia (CEO), Tom (CUO), Chevy (CTOO)

## Decision

Deploy Anthropic Claude Teams as a **knowledge access convenience layer** for the LRMGA team. Claude Teams is explicitly NOT an operational backbone.

## Context

LRMGA is approaching April 2026 go-live targeting $35M GWP Year 1. The team is onboarding new hires (Cali, Angel on 2026-03-02; Daniel on 2026-03-16) who need rapid access to carrier documentation, DA parameters, and BEACON governance guidance. An initial proposal (Perplexity-generated) framed Claude Teams as an "integrated operational backbone." This framing was rejected.

## Source-of-Truth Hierarchy (Non-Negotiable)

1. **BEACON** -- authoritative governance. All business logic documented here first.
2. **Federato** -- execution and enforcement. Deterministic rules, compliance controls, workflow.
3. **Claude Teams Projects** -- reference access only.

A new hire who gets an answer from Claude Teams that conflicts with what Federato enforces **follows Federato**. This hierarchy must be explicit in onboarding.

## Seat Allocation

| Person | Role | Seat Type | Monthly Cost | Rationale |
|--------|------|-----------|-------------|-----------|
| Chevy | CTOO | Premium ($150) | $150 | Claude Code for agentic refactor, multi-dev/multi-agent workflows |
| Cali | UW Tech | Premium ($150) | $150 | Claude Code for development, GitHub integration |
| Daniel | Technical PM | Premium ($150) | $150 | Claude Code for requirements-to-implementation workflow |
| Tom | CUO | Standard ($25-30) | ~$28 | UW knowledge base access, carrier doc reference |
| Angel | Underwriter | Standard ($25-30) | ~$28 | UW knowledge base access, DA parameter reference |

**Total: ~$500-520/month**

Aaron (CEO) stays off-platform unless visibility needs emerge. Seats for UW Director and UW Assistant added when those hires confirm post-launch.

## What Claude Teams Does Well

- Accelerates new hire ramp by making carrier docs, DA terms, and BEACON guidance conversationally accessible
- Provides shared workspace for dev team (Chevy, Cali, Daniel) with Claude Code for the agentic framework refactor
- Custom instructions per Project enable role-appropriate framing
- Cost is a rounding error relative to onboarding cost of a mis-ramped underwriter

## What Claude Teams Does NOT Do

- Replace Federato for compliance enforcement or deterministic rule execution
- Self-document, self-test, or detect drift (0/3 on the SDLC Triad)
- Provide audit trail of guidance given to whom on which decision
- Automatically update when carrier documents change
- Address Loop 3 (FSIS feedback-embedded underwriting), which requires person-to-person training

## Rejected Alternative

The Perplexity-generated proposal assumed a blank-slate MGA without existing governance (BEACON) or PAS (Federato). Specific claims corrected:

| Claim | Correction |
|-------|-----------|
| "Cross-reference every UW decision against DA parameters" | This is Federato's job. Claude Teams cannot prevent a binding decision. |
| "One Project per carrier" | Siloes knowledge. Real UW workflows are cross-cutting. |
| "Analytics on bordereaux" | Ephemeral chat outputs are not auditable reporting pipelines. |
| "Operational backbone" | Backbone requires self-documentation, self-testing, drift detection. Claude Teams has none. |

## Governance Requirements

1. **Content ownership**: Named owner per Project responsible for document currency
2. **Refresh cadence**: Monthly review of uploaded docs against current carrier documentation
3. **Custom instruction governance**: All Project instructions documented in BEACON first, implemented in platform second
4. **Drift detection**: Quarterly review of Project content against BEACON and carrier source docs
5. **Onboarding integration**: Claude Teams is ONE component alongside Federato training, FSIS workflow mentorship, and supervised decision-making during ramp

## Constraints

- Claude Teams Projects cap at 200K tokens (~500 pages). RAG expansion (up to 10x on paid plans) helps but introduces retrieval quality variability.
- Claude Teams has 0/3 on the self-documenting SDLC Triad. Governance processes must compensate manually.
- Custom instructions per Project ARE business logic authored in free-text inside Anthropic's platform. If not mirrored in BEACON, institutional knowledge gets trapped in a vendor platform.
- Minimum 5 seats required for Teams plan. Cannot start smaller.

## Classification

- **Implementation** (setting up Projects, uploading docs): Inference/commodity
- **Governance architecture** (what goes in, who owns currency, drift detection, source-of-truth enforcement): Alpha (CTOO)
