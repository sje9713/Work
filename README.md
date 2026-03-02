# Workflow Systems Case Studies (No Code)

This repository contains short, anonymized case studies describing how I structure requirements, design workflow logic, and validate production behavior in policy-constrained environments.

- No proprietary code
- Focus on decision-making, constraints, edge cases, and operational outcomes

## Why this exists
I tend to build systems in ambiguous, constraint-heavy contexts (policy, multi-role workflows, structured data).  
I document real-world engineering judgment: what was unclear, what assumptions were made explicit, how safety rails were designed, and how validation refined the final behavior.

## Case Studies
1) **Leave Accrual Automation (Policy-Constrained Rule Engine + Data Reconciliation)**  
- Reconstructed fragmented policy documents into a production-safe accrual model  
- Designed rule-based automation with manual override coexistence  
- Performed dataset-level reconciliation against historical manual allocations and iterative refinement

2) **Remote Work Activity Management with Cross-System Workflow Design**
- Extended a live approval workflow without breaking legacy state transitions  
- Introduced structured plan, checklist, and outcome tracking (day-based model)  
- Designed Web ↔ Runtime Agent contract for time-sensitive popup triggers  
- Unified business-day logic across systems to prevent off-by-one inconsistencies  
- Stabilized cross-system runtime behavior through iterative debugging and logging
