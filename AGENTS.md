# FreshKitchen Docs AI Working Rules

This repository manages the current shared documents for the FreshKitchen team.

AI agents must treat this repository as a documentation workspace, not an implementation repository.

## Repository Purpose

The goal of this repository is to make the current project state easy to read from GitHub.

The root documents are the source of truth for the team:

- `README.md`: repository purpose and navigation
- `STATUS.md`: current progress
- `SCOPE.md`: included and excluded project scope
- `ROADMAP.md`: upcoming document and project milestones
- `PRESENTATION.md`: presentation scope and demo flow
- `api/frontend-backend.md`: frontend-backend API contract
- `api/backend-ai.md`: backend-AI API contract

## Writing Rules

- Write for teammates who open Markdown directly on GitHub.
- Prefer short paragraphs, headings, bullets, and checklists.
- Avoid tables unless comparison or field mapping is genuinely clearer as a table.
- Do not create archive-style folders such as `specs/` or `decisions/`.
- Update the existing root documents instead of creating new status documents.
- Separate long JSON or YAML examples into `api/examples/`.
- Keep confirmed content separate from unresolved questions.
- Remove or update stale content instead of preserving old explanations.
- Do not leave placeholder text that looks final.

## API Documentation Rules

- Use `api/frontend-backend.md` for the frontend-backend boundary.
- Use `api/backend-ai.md` for the backend-AI boundary.
- Keep examples in `api/examples/`.
- Use readable explanation first, then link to example files.
- Keep request and response examples small enough to understand.

## Commit Workflow

Use `.codex/skills/smart-commit/SKILL.md` for commit analysis and commit creation.

The skill owns:

- diff inspection
- document-oriented commit grouping
- selective staging
- Korean commit message generation
- commit execution

Do not push, create pull requests, or merge unless the user explicitly asks.
