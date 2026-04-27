# Plans

Output target for `/to-plan`. One file per feature:

```
plans/<feature-name>.md
```

A plan breaks an approved PRD into phased vertical slices. Each phase cuts end-to-end through schema, API, UI, and tests — no horizontal layer-only slices.

## Lifecycle

```
PRD issue (approved)
  → /to-plan
  → plans/<feature>.md  (this folder)
  → /to-issue            (creates GitHub work items, optionally referencing the plan)
  → engineering harness  (issues consumed by /issue-to-design and /issue-to-code)
```

## File contents

Each plan file should include:

- **Architectural decisions** — durable choices (routes, schema shape, key models, auth approach)
- **Phases** — numbered, with user stories and acceptance criteria per phase
- **Dependencies** — between phases, and on external systems
- **Open questions** — must be empty before the plan is used to generate issues

## Naming

Use kebab-case. Match the PRD slug where possible.

Examples: `authenticate.md`, `course-enrollment.md`, `instructor-payouts.md`.

## Index

| Feature | PRD | Status |
|---|---|---|
| _none yet_ | — | — |
