# SDD Workflow Skill Bundle

This repository contains a Codex skill bundle called `sdd-workflow`. It codifies
a Spec-Driven Development (SDD) lifecycle into explicit commands and fixed
templates so work stays traceable, consistent, and auditable from research to
delivery. The workflow prioritizes focused research, a concrete plan/spec, and
then controlled execution with approvals. This structure keeps the context
window lean and reduces failure modes like over-engineering, reinventing the
wheel, duplicating code, or mixing responsibilities in a single file.

Inspiration source:
`https://dfolloni.substack.com/p/como-eu-uso-o-claude-code-workflow`

Prompt sources:

- The prompts are based on the commands from `https://github.com/humanlayer/humanlayer`

## What this provides

- A strict, template-driven workflow: research -> plan -> implement -> close
- A single source of truth for feature status in `.codex/PRD.md`
- A lightweight operational cache in `.codex/PROJECT_MAP.md`
- Per-command templates that enforce scope and interaction patterns
- Explicit approvals for any code changes during implementation

## How to install

1. Ensure this repo is available in your Codex workspace.
2. Register the skill so Codex can discover it, using the provided agent
   configuration in `sdd-workflow/agents/openai.yaml`.
3. Verify the bundle by running `/research_codebase` and checking that
   `.codex/PRD.md` and `.codex/PROJECT_MAP.md` are created or updated.

## Getting Started

1. Run `$sdd-workflow /research_codebase` to capture the current architecture and update
   `.codex/PRD.md` and `.codex/PROJECT_MAP.md`.
2. Run `$sdd-workflow /research_feature` for the feature you want to build. Answer the wizard
   questions and review the generated research doc (PRD-style summary).
3. Clear your context (e.g., start a new chat) to keep the prompt lean.
4. Run `$sdd-workflow /plan_feature` to convert research into an executable plan/spec with
   phases and verification steps.
5. Clear your context again before implementation.
6. Run `$sdd-workflow /implement_feature` to execute the plan/spec with per-file approvals.
7. Run `$sdd-workflow /close_feature` when the feature is done to update the PRD.

Tip: When the context window gets too full (often beyond ~40-50%), reset the
session and continue from the latest artifact (PRD or Spec).

## Commands

- `/research_codebase` - repo-wide read-only research and docs update
- `/research_feature` - feature discovery wizard and research doc
- `/plan_feature` - executable implementation plan
- `/implement_feature` - strict plan execution with per-file approval
- `/update_skill` - update `sdd-workflow` from the upstream `skills_codex` repo (`~/.codex/skills`) into `~/.codex/skills/sdd-workflow`
- `/close_feature` - move a feature to completed in PRD

### Workflow Diagram (Mermaid)

```mermaid
flowchart LR
  A[/research_codebase/] --> B[/research_feature/]
  B --> C[/plan_feature/]
  C --> D[/implement_feature/]
  D --> E[/close_feature/]
```

## Why this workflow (context window focus)

The workflow is designed to avoid "vibe coding" by enforcing a disciplined
sequence: research -> spec -> code. The goal is to keep the context window lean,
make inputs high-signal, and reduce common failure modes:

- Over-engineering (solutions are more complex than needed)
- Reinventing the wheel (rebuilding what already exists)
- Missing knowledge (out-of-date docs or unknown tech)
- Repeated code (duplicate components/logic)
- Mixed responsibilities (too many concerns in one file)

The core idea is that poor or overloaded input leads to poor output. By
concentrating evidence into a PRD (relevant files, documentation excerpts, and
implementation snippets), then translating it into a spec that lists exact file
paths and concrete directives (what to modify or create in each file), you
minimize context bloat before any code is written. This method favors known
patterns, avoids re-implementing existing solutions, and keeps the model focused
on the exact change set.

PRD should contain:

- Relevant codebase files only (avoid noise)
- Key documentation excerpts
- Implementation snippets (internal or external patterns)

Spec should follow this pattern:

- File path
- What to change in that file

## How the workflow works

1. `/research_codebase` scans the repo to document current architecture and
   updates `.codex/PRD.md`, `.codex/PROJECT_MAP.md`, and a research artifact.
2. `/research_feature` runs a short, sequential wizard to capture feature scope
   and creates a feature research document (PRD-style summary with relevant
   files, doc excerpts, and snippets).
3. `/plan_feature` converts research into an executable plan/spec with phases,
   verification steps, and references (including exact file paths and changes).
4. `/implement_feature` executes the plan/spec phase by phase with strict scope
   rules and per-file approval by default.
5. `/close_feature` moves a feature from Open to Completed in the PRD, adding
   the completion date.

## Repo Structure

- `sdd-workflow/` - skill bundle root
- `sdd-workflow/agents/` - agent config and routing
- `sdd-workflow/references/` - fixed templates and bootstrap docs
- `.codex/` - generated artifacts (PRD, PROJECT_MAP, research/plan/spec/impl)

## Notes

- Research and planning are read-only.
- Implementation requires explicit approval per file by default.
- Language policy: artifacts in English; interactive prompts in pt-BR.
