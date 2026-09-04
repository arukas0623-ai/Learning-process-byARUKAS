# Learner State Specification

Version: `0.2.0-alpha`

Learner State is user-owned, portable state that records the learner's current
position against an Ability Map. It must remain separate from source material
and from the Agent implementation.

## Required concepts

Each competency record should identify:

- `competency`: stable node ID and human-readable name;
- `status`: one of `UNKNOWN`, `HYPOTHESIS`, or `VERIFIED`;
- `evidence`: locatable observations, answers, code, tests or review records;
- `confidence`: a number from 0 to 1;
- `verification`: a planned or completed validation task;
- `timestamp`: when the observation, verification or state change occurred.

The machine-readable baseline is
`core/learner-state/schema/learner-state.schema.json`.

The public template includes `name`, `verifications` and `timestamp` so a new
instance follows this specification. The current baseline JSON Schema enforces
the core status/evidence/confidence gate; stricter field-level enforcement is a
known follow-up and is not changed in this release.

## State rules

`UNKNOWN` is the default when evidence is absent. `HYPOTHESIS` records a
testable interpretation of observed evidence. `VERIFIED` requires at least one
evidence record, one verification record and confidence at or above 0.6.

An Agent must not convert a single response, a reading event or a self-report
directly into a stable ability claim. State changes follow:

```text
Observation → Evidence → Hypothesis → Verification → Stable State
```

Every update should preserve the source reference and timestamp so another
Agent can audit or migrate it.
