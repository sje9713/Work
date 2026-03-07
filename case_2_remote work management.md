# Case 2 — Remote Work Activity Management with Cross-System Workflow Design

## Context

- Existing production workflow:
  - request → approval → approved / rejected / cancelled
- Requirement:
  - add structured work planning
  - enforce day-based request (no date range)
  - introduce checklist + end-of-day result submission
  - surface plan/result visibility for managers/admins
  - trigger PC-side popup reminders via Agent
- Constraint:
  - preserve legacy approval flow and permissions
  - no destructive impact on existing documents
  - align Web policy logic with Agent runtime behavior

I owned:
- requirement refinement with client
- Web ↔ Agent coordination
- DB design + backend implementation
- test planning and rollout stabilization

---

## Requirement Decomposition

Separated into 3 layers:

1) Request-time layer  
   - Work plan input (1–10 entries)
   - Issue notes
   - Day-only constraint
   - Overtime conflict validation

2) Day-of execution layer  
   - Checklist popup on PC login (Agent-triggered)
   - Full validation required before save
   - Save → 1-minute delayed popup removal
   - Prevent duplicate submissions

3) End-of-day closure layer  
   - Result popup at T-30min before end of workday
   - Allow submission until 3 business days
   - After 3 business days → mark as "unsubmitted"
   - Manager/Admin visibility & filtering

---

## Data Modeling Strategy

New structured entities:

- remotework_plan
- remotework_chkls
- remotework_outcome
- remotework_issue

Design principles:

- Link artifacts to approved telecommuting document
- Preserve original approval document as source of truth
- Maintain status codes (0,1,2,6...) without mutating legacy states
- Ensure idempotency in Agent-triggered saves

---

## Web ↔ Agent Contract Design

Core challenge: policy defined in Web, executed in Agent runtime.

Defined:

- Trigger conditions:
  - Checklist: PC-ON on approved telecommuting day
  - Outcome: T-30min before end-of-day
  - Past-due: immediate popup on PC-ON
- Dismiss behavior:
  - temporary vs completed
  - prevent infinite re-spawn loops
- Re-trigger logic:
  - handle popup removal by external events (e.g., temporary PC unlock)
  - re-execute safely without duplicate data creation
- Logging added for timing inconsistencies

Observed issues during testing:
- Wrong Agent option name → no popup
- Popup triggered outside allowed window
- Business-day mismatch between Web and Agent
- Popup disappearing after temporary-use window
- Re-execution blocked by running process state

Iteratively refined and aligned runtime behavior

---

## Business-Day Logic Unification

Detected mismatch:
- Agent counted business days differently from Web
- Off-by-one-day discrepancy possible

Decision:
- Align both systems to Web-side business-day calculation
- Adjust loop bounds + holiday handling
- Accept slightly more conservative classification
- Document potential rollback risk

---

## Edge Cases Handled

- Approval → cancellation after checklist partially saved
- Workday modification after popup scheduled
- Overtime request conflict with telecommuting
- Same-day registration policy changes (client-driven change mid-project)
- Multiple alerts overlapping (checklist + outcome)
- Submission ordering constraint:
  - cannot submit outcome before checklist (same-day case)
  - past-day exception allowed

---

## Validation & Rollout

Testing split into:

- Web-only workflow tests
- Agent runtime trigger tests
- Integration environment tests (VDI, corporate auth, version mismatch risk)

Provided structured test guide to client for scenario validation. :contentReference[oaicite:4]{index=4}

Stabilization phases included:
- business-day recalculation alignment
- alert re-execution safeguards
- front-end validation tuning
- UX friction reduction (alert fatigue control)

---

## Outcome

- Extended live approval workflow without breaking legacy state transitions
- Enabled structured plan/result capture
- Achieved cross-system runtime alignment
- Reduced manual monitoring via overdue classification
- Closed project with stable production behavior
