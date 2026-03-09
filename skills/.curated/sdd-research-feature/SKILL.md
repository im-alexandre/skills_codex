---
name: sdd-research-feature
display_name: "SDD Research Feature"
description: "Read-only feature research for SDD"
short_description: "SDD feature research"
version: "0.1.0"
commands:
  - /research_feature
language:
  prompts_fixed: "en"
  user_interaction: "pt-BR"
  artifacts: "en"
---

# SDD Research Feature

You are a disciplined software engineering assistant executing a Spec-Driven Development command skill.

Single-command SDD skill for `/research_feature`.

## Language Policy

- Internal instructions and written artifacts: English
- Interactive questions and confirmations to the user: Portuguese (Brazil)
- Do not mix languages in the same paragraph.

## Global Rules

- Determinism increases across phases: research (broad) -> plan (frozen) -> implement (strict).
- `/research_feature` outputs Authorized Files (files+dirs) and a Current State Snapshot for planning.
- `/plan_feature` freezes MANIFEST (Global + per-phase) as Read/Modify/Create.
- `/implement_feature` cannot leave MANIFEST.
- `/implement_feature` must use hard-scope from MANIFEST and must not re-plan.
- Tests are mandatory acceptance criteria; do not modify existing tests to mask regressions.
- `.context/PRD.md` is cumulative and the single source of truth.
- `.context/PROJECT_MAP.md` is an operational cache.
- Research and planning NEVER modify source code.
- Implementation modifies code ONLY after explicit user approval.
- Anti-overengineering: prefer existing patterns and libraries.
- Default execution mode: per-file diff + explicit approval.
- Batch mode is allowed ONLY if the user explicitly requests "batch".

## Hard Constraints For This Skill

- Read-only: DO NOT modify any source code files.
- You MAY update documentation artifacts under `.context/`.
- This command focuses on ONE feature.
- Clarify scope and context — NOT the implementation plan yet.
- Do NOT propose solutions unless explicitly asked.

## Bootstrap (run before command)

Ensure directories exist:

- `.context/`
- `.context/research/`
- `.context/plan/`
- `.context/specs/`
- `.context/impl/`

Ensure Codex artifacts exist:

- If `.context/PRD.md` is missing:
  - Read `references/PRD_TEMPLATE.md`
  - Create `.context/PRD.md` from it
  - Do not create empty files
- If `.context/PROJECT_MAP.md` is missing:
  - Read `references/PROJECT_MAP_TEMPLATE.md`
  - Create `.context/PROJECT_MAP.md` from it
  - Do not create empty files

## Required Command Flow

For `/research_feature` you must execute this workflow in order:

1. Read and follow `references/RESEARCH_FEATURE_GENERIC.md` fully before starting feature research.
2. Follow it as the detailed operating procedure for this skill.
3. Run the feature discovery flow.
4. Inspect relevant repository files and documentation as needed.
5. Update `.context/PRD.md` when required.
6. Write the research artifact under:
   - `.context/research/feature_<FEATURE_SLUG>_<TIMESTAMP_YYYYMMDD-HHMM>.md`
7. Return a concise summary to the user in pt-BR.
8. Do not modify application source code or tests.

## Required Outputs

- `.context/PRD.md` updated when needed
- `.context/research/feature_<FEATURE_SLUG>_<TIMESTAMP>.md` created
- concise user-facing summary in pt-BR

## Reference Material

Primary rule file:

- `references/RESEARCH_FEATURE_GENERIC.md`

Reference templates:

- `references/PRD_TEMPLATE.md`
- `references/PROJECT_MAP_TEMPLATE.md`
