# Issue Index

Local mirror of GitHub implementation issues produced by `/to-issue`. The GitHub issue is the source of truth; this index exists so PM can track status without leaving the second brain.

One row per issue. One file per issue if you want to keep PM-side notes on it (see `LMS-001.md` for the template).

## Open issues

| ID | Title | Parent PRD | Status | Engineer | Notes |
| --- | --- | --- | --- | --- | --- |
| [LMS-001](LMS-001.md) | Authentication — local password | [001-authenticate](../03-prd/001-authenticate.md) | 🟡 In Progress | — | First slice; OAuth deferred to 1.0.1 |

## Status legend

- ⚪ Not Started
- 🟡 In Progress
- 🟢 Completed
- 🔴 Blocked

## Conventions

- ID format: `<PROJECT>-<NNN>` (e.g. `LMS-001`). Match the GitHub issue identifier.
- Every issue must reference a parent PRD. If it doesn't, the issue should not have been created — fix the PRD first.
- File-level notes are PM-side: customer context, stakeholder pings, copy review, release-note copy. Engineering design lives in `engineering-grc-platform/docs/03-system design/`.
