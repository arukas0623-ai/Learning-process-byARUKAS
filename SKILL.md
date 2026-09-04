---
name: agent-learning
description: Run a durable, evidence-gated active-learning loop for any subject using IRLA, a pluggable ability-map provider, a pluggable scheduler, and user-owned Markdown/JSON state. Use for ongoing study and reassessment; do not use for one-off explanations or full-course note dumps.
metadata:
  short-description: Evidence-gated, context-bounded agent learning
---

# Agent Learning Skill

This Skill turns an Agent into a tutor and evaluator while keeping durable state
outside chat. It is domain-neutral: the knowledge source, ability-map provider,
scheduler, and learner data are replaceable.

## Non-negotiable boundaries

- Keep `Knowledge Source`, `Ability Map`, and `Learner State` separate.
- Use IRLA: Input → Recall → Learning Validation → Application.
- During Recall, expose only a Learner View; load the Evaluator View only after an
  answer is recorded.
- Update long-term state only through Observation → Evidence → Hypothesis →
  Verification → Stable State. A single mistake is not a stable ability judgement.
- Check `user-space/context-budget.json` (or the configured equivalent) before
  loading state. Never recursively load all logs, history, items, or map nodes.
- The Agent owns questions, explanations, and judgement; the scheduler owns
  timing, intervals, and numeric review state.
- Do not create UI, a database, a multi-agent system, or a whole-course dump.

## Session routing

1. Read [agents/AGENT_PROTOCOL.md](agents/AGENT_PROTOCOL.md) and the active
   `user-space/context-budget.json` (or an equivalent budget inside the selected
   learner-state).
2. Import or identify the source and map it through the configured adapters.
3. Load only the compact learner profile, constraints, relevant competency
   summary, and the scheduler's Learner View queue.
4. Run [Workflow.md](Workflow.md), then write evidence-linked state and schedule
   the next review.

Detailed contracts live under `core/`; stable cross-provider interfaces live
under `specification/`; provider-specific behavior lives under `adapters/`.
The public starting point is `examples/template-learning/`. Real learner state
belongs in a private User Space and must not be committed to this repository.
