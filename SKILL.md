---
name: sdd-workflow
display_name: "SDD Workflow"
description: "Spec-Driven Development workflow bundle: research → plan → implement → close"
short_description: "SDD workflow with deterministic escalation and strict implementation phase"
version: "0.2.1"
commands:
  - /research_codebase
  - /research_feature
  - /plan_feature
  - /implement_feature
  - /update_skill
  - /close_feature
language:
  prompts_fixed: "en"
  user_interaction: "pt-BR"
  artifacts: "en"
---

# SDD Workflow (Bundle)

This bundle implements a Spec-Driven Development workflow with escalating determinism:

- `/research_codebase`:
  Semi-nuclear intelligent repository analysis.
  Reads README/AGENTS/.gitignore first, performs volume detection, requests authorization,
  then performs structured architectural research.
  Updates `.codex/PRD.md` and `.codex/PROJECT_MAP.md` without modifying source code.

- `/research_feature`:
  Wizard-driven contextual research for a specific feature.
  Produces structured research artifacts without modifying source code.

- `/plan_feature`:
  Generates an executable plan.
  Freezes MANIFEST (Read / Modify / Create).
  No source code changes.

- `/implement_feature`:
  Strict implementation phase.
  Cannot leave MANIFEST.
  Per-file diff approval by default.
  Tests are mandatory acceptance criteria.

- `/update_skill`:
  Performs `git pull` on `~/.codex/skills`.
  Non-interactive.
  Outputs only success or failure.

- `/close_feature`:
  Moves a feature from Open Features to Completed Features in `.codex/PRD.md`.
  Does not modify source code.

## Bootstrap Behavior

If missing, the bundle ensures:

- `.codex/`
- `.codex/context/research/`
- `.codex/context/plans/`
- `.codex/context/specs/`
- `.codex/context/impl/`
- `.codex/PRD.md` (from PRD_TEMPLATE.md)
- `.codex/PROJECT_MAP.md` (from PROJECT_MAP_TEMPLATE.md)

Determinism increases across phases.
Research allows structural flexibility.
Planning freezes scope.
Implementation is mechanical.
