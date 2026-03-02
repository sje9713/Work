# Production System Case Studies (Policy, Workflow, AI Tooling)

I build and refine production systems where policy logic, workflow state, and structured data must remain consistent under real-world constraints.

This repository documents selected production-oriented case studies from my current engineering role.

---

## Case Studies

### 1) Leave Accrual Automation (Policy-Constrained Rule Engine)

- Reconstructed fragmented client policy documents into a production-safe accrual model
- Translated complex labor rules into configurable backend logic
- Designed rule-based automation while preserving manual override pathways
- Performed dataset-level reconciliation against historical manual allocations to validate integrity
- Stabilized logic through iterative comparison with legacy operational data

→ See: `case_1_leave accrual automation.md`


### 2) Remote Work Activity Management with Cross-System Workflow Design

- Extended a live approval workflow without breaking legacy state transitions
- Introduced structured plan, checklist, and outcome tracking (day-based constraint model)
- Designed Web ↔ Runtime Agent contract for time-sensitive popup triggers
- Unified business-day logic across systems to prevent off-by-one inconsistencies
- Iteratively stabilized cross-system runtime behavior through logging and validation

→ See: `case_2_remote work management.md`


### 3) LLM Tool Reliability Validation in Structured HR Systems

- Designed hypothesis-driven experiments to isolate schema effects (name vs description vs enum)
- Identified “correct tool, incomplete parameters” failure mode
- Validated tool selection and parameter extraction behavior under multi-tool conditions
- Building guardrails for reliable SQL mapping in policy-sensitive environments

→ See: `case_3_llm tool reliability validation.md`

---

## Scope

All cases focus on:

- Production environments (not toy prototypes)
- Structured data systems
- Policy- or workflow-sensitive logic
- Reliability under ambiguity

Each document describes problem context, system constraints, design decisions, and observed failure modes.

No proprietary code is included.
