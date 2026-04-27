---
name: init-project
description: Scaffold a new PM harness project — creates the full directory tree (README, .agents/skills, second brain) and seeds it with the to-prd, to-plan, to-issue skills plus all index/template files. Use when the user wants to bootstrap a new product-management harness, init a PM project, or replicate this harness structure in another repo.
---

# Init Project

Bootstrap a new PM-harness project that mirrors this repo's structure: a `README.md` describing the harness, a `.agents/skills/` directory with the three pipeline skills (`to-prd`, `to-plan`, `to-issue`), and a `second brain/` second-brain knowledge base seeded with index files for inbox, ubiquitous language, PRDs, stakeholders, issues, and releases.

## When to activate

Trigger when the user says any of:

- "init a new PM project / harness"
- "scaffold a product-management harness"
- "create the second-brain structure for <repo>"
- "set up to-prd/to-plan/to-issue in another project"

## Process

### 1. Confirm the target

Ask the user:

- **Target directory**: absolute path where the project should be created (e.g. `~/work/acme-pm-harness`). If the directory already exists and is non-empty, ask whether to abort, merge, or pick a new path.
- **Project name**: human-readable name used in the README title (e.g. "Acme PM Harness").
- **Domain**: short descriptor for the README intro (e.g. "GRC Platform", "Mobile Banking").
- **Engineering harness path** (optional): relative path to the downstream engineering harness, used in the README footer. Skip if not applicable.

Echo the resolved values back and wait for confirmation before writing.

### 2. Create the directory tree

Create exactly this layout under the target directory:

```
<target>/
├── README.md
├── .instructions                    # empty placeholder
├── .agents/
│   └── skills/
│       ├── init-project/SKILL.md    # copy of this skill
│       ├── to-prd/SKILL.md
│       ├── to-plan/SKILL.md
│       └── to-issue/SKILL.md
└── second brain/
    ├── 01-inbox/raw.md
    ├── 02-ubiquitous language/index.md
    ├── 03-prd/
    │   ├── 000-index.md
    │   └── legacy.md
    ├── 04-stakeholder/
    │   ├── index.md
    │   ├── bd.md
    │   ├── cs.md
    │   └── csc.md
    ├── 05-issue/
    │   └── index.md
    └── 06-release/
        └── index.md
```

Use `mkdir -p` for directories. Do not create empty `.gitkeep` files — empty placeholder docs (like `.instructions`, `02-ubiquitous language/index.md`, `csc.md`) carry the structural intent.

### 3. Seed the skill files

Write the four `SKILL.md` files with the canonical contents from this repo's `.agents/skills/` directory. Do NOT paraphrase — copy verbatim so the downstream pipeline behaves identically. The four skills are:

- `init-project/SKILL.md` — copy of this skill itself, so the new project can re-bootstrap if needed
- `to-prd/SKILL.md` — Idea → PRD interview
- `to-plan/SKILL.md` — PRD → phased plan in `./plans/`
- `to-issue/SKILL.md` — PRD → GitHub issues

If you do not have the source content in context, read them from the calling repo's `.agents/skills/<name>/SKILL.md` first.

### 4. Seed the second-brain files

Write each file using the templates below. Replace `<placeholders>` with values from step 1.

<readme-template>
# AI Harness — <Project Name> (<Domain>)

An AI harness is a set of structured, repeatable workflows baked into Claude Code as local skills. Rather than prompting an AI freeform, a harness constrains it to a proven process — like a harness on a horse, directing the power rather than fighting it.

This repo contains the PM harness for <Domain>. It gives product managers a consistent, interview-driven pipeline from raw idea to GitHub-ready work items.

---

## Concept

Unstructured AI output is noisy. A harness trades flexibility for reliability: every skill follows a fixed process, asks the right questions, and produces artifacts in a predictable format that the engineering harness can consume downstream.

The output of each PM skill becomes the input of the next:

```
Idea → [to-prd] → PRD issue → [to-plan] → plan file → [to-issue] → GitHub issues
```

---

## Skills

- `/to-prd` — Idea to PRD (GitHub issue)
- `/to-plan` — PRD to phased plan (`./plans/<feature>.md`)
- `/to-issue` — PRD to GitHub issues, in dependency order
- `/init-project` — Re-scaffold this structure in another repo

---

## Setup

- [Claude Code](https://claude.ai/code) installed
- `gh` CLI authenticated to GitHub
- Skills are picked up automatically from `.agents/skills/`

---

## Downstream: Engineering Harness

<If engineering harness path provided, point to it; otherwise omit this section.>
</readme-template>

<inbox-raw-template>
มีอะไรใหม่ ใส่นี้ไว้ก่อน


idea / tasks
[]



completed
</inbox-raw-template>

<ubiquitous-language-template>
# compact
</ubiquitous-language-template>

<prd-index-template>
# PRD Index

## Big Picture

_Define the overarching product vision and how each epic contributes to it._

## Epic List

| #   | Epic                | Jira Ref | Status        | Completed Date |
| --- | ------------------- | -------- | ------------- | -------------- |
| 001 | <first epic>        | —        | ⚪ Not Started | —              |

### Status Legend

- ⚪ Not Started
- 🟡 In Progress
- 🟢 Completed
- 🔴 Blocked
</prd-index-template>

<prd-legacy-template>



release version <area>


versioning

1.0.0 -
1.0.1 -


change log
</prd-legacy-template>

<stakeholder-index-template>
User Story


AC



Non Func (not generic)
</stakeholder-index-template>

<stakeholder-bd-template>
# Business Development

| Name | Contact |
| ---- | ------- |
|      |         |
</stakeholder-bd-template>

<stakeholder-cs-template>
# Customer Support

| Name | Contact |
| ---- | ------- |
|      |         |
</stakeholder-cs-template>

<stakeholder-csc-template>
[HOLD]
</stakeholder-csc-template>

<issue-index-template>
first task

ดึงจาก src code
ทำ glossary
อันไหนซ้ำ ให้เรามาเลือก
</issue-index-template>

<release-index-template>
Release Note




1.0.0 - วันไหน อะไรบ้าง
</release-index-template>

The `.instructions` file is an empty placeholder — `touch` it.

### 5. Verify

Run `find <target> -type f -not -path "*/.git/*"` and show the user the resulting tree. Confirm:

- All 4 `SKILL.md` files exist and are non-empty
- All 7 second-brain seed files exist
- `README.md` substituted the project name and domain correctly

### 6. Suggest next steps

Tell the user:

- Run `cd <target> && git init && git add -A && git commit -m "init PM harness"` to start version control
- Open the directory in Claude Code; the four skills auto-load from `.agents/skills/`
- Start the pipeline with `/to-prd` once they have a problem to articulate

## Notes

- The `second brain/` folder name contains a space — quote it in shell commands.
- Numeric prefixes (`01-`, `02-`, …) are deliberate; they enforce sort order in file explorers. Preserve them.
- Do NOT init git for the user — let them choose remote/visibility themselves.
- Do NOT install `gh` or run `gh auth login` — the README documents these as prerequisites.
