# Adapter Specification

Version: `0.2.0-alpha`

Adapters are replacement points around a domain-independent Core. They may be
implemented as scripts, local files or external tools, but they must preserve
the contracts below.

## Knowledge Source Provider

Returns source ID, version/date, path or URI, selected range and extraction
method. It provides citable material and never writes a VERIFIED competency.

## Ability Map Provider

Returns stable ability nodes using the fields in
`ability-map-spec.md`: `id`, `name`, `description`, `prerequisites` and
`assessment_methods`.

## Scheduler Provider

Provides due items, keeps Learner View separate from Evaluator View, records
grades and confidence, and owns interval/lapse/next-review calculations. It
must not require the Agent to hand-calculate scheduling values.

## Agent

The Agent loads the Core protocol, asks and evaluates questions, writes
evidence-linked state, and respects the configured context budget. The Core
does not depend on Codex, Claude Code or a particular model.
