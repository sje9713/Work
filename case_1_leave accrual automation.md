# Case 1 — Policy-Constrained Leave Accrual Automation

## Context
- Fragmented and partially conflicting requirement documents across multiple “cases”
- Stakeholder replacement mid-project → policy intent continuity was weak
- Policy details not fully closed even during implementation
- Legacy manual allocation (spreadsheet-driven) already used in production
- Need for production-safe automation that would not break existing operations

## What I optimized for
- Policy alignment and operational safety over raw feature velocity
- Explicit assumption logging (do not hide uncertainty inside code)
- Coexistence of automation + manual override

## Requirement structuring
- Consolidated multiple documents into 2 core tenure buckets:
  - `< 1 year`
  - `>= 1 year`
- Modeled extensions by employment type (e.g., regular / contract / executive)
- Separated rule dimensions:
  - accrual rule
  - carry-over rule
  - employment-type transition rule
  - termination/adjustment rule
- Maintained “manual allocation” pathway during transition instead of forcing full replacement

## System design decisions
- Rule-based accrual model keyed by:
  - employment type
  - tenure bucket
  - trigger event (hire / conversion / extension)
- Guard conditions for safe generation:
  - exclude users with existing allocations (avoid destructive overwrite)
  - exclude users with manual modification history by default
- Designed automation to be idempotent and safe to re-run (with controlled deletion only when applicable)
- Allowed manual adjustments to coexist with automation (operations must remain flexible)

## Operational tooling & safety rails
- Provided an operator-facing guide describing:
  - switching between manual (spreadsheet) vs auto-generation modes
  - manual adjustments after auto-generation
  - safe deletion only when no usage history exists
  - exceptions requiring operator intervention (e.g., differing hire-date references, employment-type changes)
- Added validation checks to block unsafe deletion scenarios related to downstream processes

## Edge cases explicitly considered
- Employment-type conversions and identifier/attribute transitions
- Overlap between pre-generated records and auto-generation
- Carry-over interacting with first-year accrual logic
- Policy reference changes mid-implementation
- Operational exceptions where automation should not decide (requires operator override)

## Data reconciliation & iterative refinement
- Ran dataset-level comparison:
  - automated accrual results vs historical manual allocations
- Detected significant mismatch in initial runs, then:
  - analyzed root causes (policy reference mismatch, carry-over, manual adjustments)
  - refined the rule engine and assumptions
  - re-validated through another reconciliation cycle
- The system converged through “data-informed policy alignment”, not blind implementation

## Outcome
- Standardized accrual logic across employment types
- Reduced dependency on manual interpretation for routine cases
- Preserved override capability for exceptions
- Stabilized the workflow without post-release operational complaints
