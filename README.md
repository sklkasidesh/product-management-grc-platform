# AI Harness — Product Management (GRC Platform)

An AI harness is a set of structured, repeatable workflows baked into Claude Code as local skills. Rather than prompting an AI freeform, a harness constrains it to a proven process — like a harness on a horse, directing the power rather than fighting it.

This repo contains the PM harness for the GRC platform. It gives product managers a consistent, interview-driven pipeline from raw idea to GitHub-ready work items.

---

## Concept

Unstructured AI output is noisy. A harness trades flexibility for reliability: every skill follows a fixed process, asks the right questions, and produces artifacts in a predictable format that the engineering harness can consume downstream.

The output of each PM skill becomes the input of the next:

```
Idea → [to-prd] → PRD issue → [to-plan] → plan file → [to-issue] → GitHub issues
```

The engineering harness then picks up those issues:

```
GitHub issues → [issue-to-code] → working code → [code-review] → merged PR
```

---

## Workflow

### Git flow
Trunk-based development. All work merges to `main` via short-lived branches. PRs require approval from the designated owner before merge.

---

## Skills

Skills are invoked inside Claude Code with `/skill-name` or by describing what you want.

### `/to-prd` — Idea to PRD

Interviews the user, explores the codebase, and produces a PRD submitted as a GitHub issue.

**When to use**: You have a problem or feature idea and need to turn it into a structured requirements document.

**Output**: A GitHub issue with problem statement, user stories, implementation decisions, testing decisions, and out-of-scope sections.

**Process**:
1. User describes the problem in detail
2. Claude explores the codebase for related patterns and integration points
3. Claude interviews the user on users, workflow, edge cases, data model, and compliance requirements
4. Claude sketches major modules and confirms with user
5. PRD is written and submitted as a GitHub issue

---

### `/to-plan` — PRD to Implementation Plan

Breaks a PRD into a phased implementation plan using tracer-bullet vertical slices, saved as a local Markdown file.

**When to use**: You have an approved PRD issue and need a phased delivery plan before creating work items.

**Output**: `./plans/<feature-name>.md` — a multi-phase plan with architectural decisions and acceptance criteria per phase.

**Process**:
1. PRD is confirmed in context (or fetched from GitHub)
2. Durable architectural decisions are identified (routes, schema, key models, auth)
3. PRD is broken into vertical slices — each slice is a thin, demoable path through all layers
4. User reviews and adjusts granularity
5. Plan file is written

**Vertical slice rules**: Each phase must cut end-to-end through schema, API, UI, and tests. No horizontal layer-only slices.

---

### `/to-issue` — PRD to GitHub Issues

Breaks a PRD into independently-grabbable GitHub issues ready for the engineering harness.

**When to use**: You have an approved PRD (and optionally a plan) and need to create the implementation tickets.

**Output**: A set of GitHub issues, created in dependency order, each with acceptance criteria, blocked-by links, and parent PRD reference.

**Process**:
1. PRD is fetched from GitHub
2. PRD is broken into tracer-bullet slices; each is marked HITL (requires human) or AFK (can be implemented autonomously)
3. User reviews dependency graph and granularity
4. Issues are created via `gh issue create` in dependency order

---

## Setup

### Prerequisites

- [Claude Code](https://claude.ai/code) installed
- `gh` CLI authenticated to GitHub
- Skills installed under `.agents/skills/`

### Install skills

Skills are already present in `.agents/skills/`. Claude Code picks them up automatically from this directory.

```
.agents/skills/
├── to-prd/SKILL.md
├── to-plan/SKILL.md
└── to-issue/SKILL.md
```

---

## Downstream: Engineering Harness

The issues produced by this harness are consumed by the engineering harness in `../engineering-grc-platform/`. That harness provides:

- `/issue-to-code` — implements a GitHub issue with atomic commits
- `/code-review` — pre-landing review with auto-fix for obvious issues
- `/e2e-testing`, `/api-testing` — verification skills

---

## PR Approval

All PRs to `main` require approval from the designated repo owner before merge.
