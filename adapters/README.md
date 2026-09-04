# Adapters

Adapters are the framework's replacement points. They connect the domain-
independent Core to a knowledge source, an Ability Map Provider and a
Scheduler Provider without changing IRLA, evidence rules or Learner State
ownership.

## Current reference configuration

```text
Agent:       Codex
Ability Map: APKM
Scheduler:   mastery-loop + FSRS
State:       user-owned Markdown/JSON
```

The current adapter files are contracts and integration notes. The external
`mastery-loop` Skill supplies the scheduler runtime; it is not vendored into
this repository. APKM is the current example provider, not a Core dependency.

Future adapters may replace any of these components if they preserve the
Learner View / Evaluator View boundary, the evidence gate and the context
budget contract.
