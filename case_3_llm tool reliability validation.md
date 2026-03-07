# Case 3 — LLM Tool Reliability Validation in Structured HR Systems

## Context

- Goal: evaluate feasibility of an LLM-powered HR assistant using tool-based function calling
- Flow:
  - Natural language → Tool selection → Parameter extraction → SQL mapping → DB → Response
- Risk:
  - Correct-looking tool call with incorrect or incomplete parameters
  - Silent logical errors in policy-sensitive environments

This work is currently ~1 month in progress and focused on mechanism validation rather than feature completion.

---

## Focus Area

Rather than building the chatbot interface, I focused on:

- How tool selection behaves under multi-tool environments
- How parameter extraction depends on schema design
- Whether selection correctness guarantees extraction completeness
- How schema design (name / description / enum) affects reliability

---

## Hypotheses

- H1: Parameter **name** influences extraction more strongly than expected.
- H2: Tool selection correctness does not guarantee parameter completeness.
- H3: Description and schema constraints improve reliability, but effects are not purely additive.

---

## Experimental Structure (Early Stage)

### 1) Variable Isolation

Designed controlled variants to separate effects:

- Obscure vs intuitive parameter names
- No vs detailed parameter descriptions
- Bare vs enum-constrained schemas
- Single-tool vs multi-tool environments

### 2) Logging & Observation

Tracked:
- Selected tool
- Extracted parameters
- Missing fields
- Enum mismatches
- Implicit category resolution failures

---

## Early Findings (Limited Test Set)

- Parameter **name** impact is stronger than expected.
  - Intuitive naming improved extraction consistency in initial tests.
- “Correct tool, wrong params” failure mode exists.
  - Model selects intended tool but omits or partially extracts required parameters.
- Enum constraints reduce ambiguity, but do not eliminate extraction gaps.

---

## Observed Failure Modes

- Missing or ambiguous dates
- Implicit category not mapped correctly to enum
- Partial target extraction (e.g., missing type/value structure)
- Valid tool selection with incomplete required fields

---

## Next Steps

- Expand categorized test buckets:
  - happy path / missing field / ambiguity / irrelevant / adversarial phrasing
- Build automated validation layer:
  - schema validation
  - semantic normalization (date resolution, enum mapping)
- Add clarification strategy when required parameters missing
- Refine parameter naming and descriptions based on extraction sensitivity

---

## Scope

- Current validation based on limited internal test queries
- Primary goal: establish evaluation framework before scaling
- Reliability metrics and larger test coverage are the next milestone
