# Evidence Specification

Version: `0.2.0-alpha`

Evidence must be attributable, time-stamped and specific enough for another
Agent to inspect. The following levels must remain distinct:

| Level | Meaning | Examples |
|---|---|---|
| Observation | What happened in a concrete interaction | Could not trace a pointer update |
| Evidence | An inspectable record of that observation | Answer, code, test output, review ID |
| Verification | A deliberate task designed to test a hypothesis | Explain the same idea in a new framing |
| Transfer Evidence | Success or failure on an unseen but related problem | Apply the concept to a new API or design |

A single incorrect answer is evidence of one observed performance event; it is
not proof of a durable inability. A single correct answer is also not proof of
mastery. Long-term status must pass the evidence gate defined by the Core
policy and retain links to the underlying records.
