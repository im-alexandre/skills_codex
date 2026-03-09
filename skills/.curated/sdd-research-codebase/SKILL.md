---
name: sdd-research-codebase
display_name: "SDD Research Codebase"
description: "Read-only repository architecture research for SDD"
short_description: "SDD codebase research"
version: "0.1.0"
commands:
  - /research_codebase
language:
  prompts_fixed: "en"
  user_interaction: "pt-BR"
  artifacts: "en"
---

# SDD Research Codebase

You are a disciplined software engineering assistant executing a Spec-Driven Development command skill.

Single-command SDD skill for `/research_codebase`.

## Language Policy

- Internal instructions and written artifacts: English
- Interactive questions and confirmations to the user: Portuguese (Brazil)
- Do not mix languages in the same paragraph.

## Global Rules

- Determinism increases across phases: research (broad) -> plan (frozen) -> implement (strict).
- `/research_feature` outputs Authorized Files (files+dirs) and a Current State Snapshot for planning.
- `/plan_feature` freezes MANIFEST (Global + per-phase) as Read / Modify / Create.
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
- You MAY update only documentation artifacts under `.context/`.
- This command analyzes repository architecture and operational structure.
- This command must not propose implementation plans unless the user explicitly asks.
- This command must not invent business intent when code evidence is insufficient.
- Keep outputs factual, technical, and terse.

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

For `/research_codebase` you must execute this workflow in order:

1. Read `references/RESEARCH_CODEBASE_GENERIC.md` fully before starting repository research.
2. Use it as the detailed operating procedure for this skill.
3. Run Stage 0 before any scan or size estimation.
4. In Stage 0, read when present:
   - `README.md`
   - `AGENTS.md`
   - `.gitignore`
   - context folders such as `.claude/`, `.context/`, `context/`, `docs/`
5. Run Stage 1 repository volume report before deep scan.
6. Estimate at minimum:
   - approximate number of relevant files
   - approximate repository size
   - predominant language
   - monorepo or multi-application signals
   - presence of large generated or vendor folders
7. Apply ignore rules from `.gitignore` plus heuristic exclusions for binary, vendor, cache, dump, and exported data files.
8. If Stage 1 indicates cost, context, or size risk:
   - STOP
   - explain the risk in pt-BR
   - ask authorization before continuing
9. Do not proceed automatically when repository size is uncertain.
10. If authorized, perform structural analysis using adaptive depth:
    - small areas: full read
    - medium areas: selective structural read
    - large areas: sampling plus orchestration files
11. If size was underestimated during deep scan:
    - STOP
    - explain the new risk in pt-BR
    - ask authorization before continuing in reduced or adjusted mode
12. Update `.context/PRD.md` using the template structure without deleting existing sections.
13. Update `.context/PROJECT_MAP.md` incrementally and factually.
14. Write the research artifact under:
    - `.context/research/<TIMESTAMP_YYYYMMDD-HHMM>_codebase.md`
15. Return a concise pt-BR summary.
16. Do not modify application source code or tests.

## Required Output Documents

Create exactly this research artifact path pattern:

- `.context/research/<TIMESTAMP_YYYYMMDD-HHMM>_codebase.md`

The research artifact header must include:

- Timestamp
- Current branch

The research artifact must contain exactly these sections:

1. `## Repository Overview`
2. `## High-Level Architecture`
3. `## Macro Flows`
4. `## Module Structure`
5. `## External Dependencies`
6. `## Test Presence`
7. `## Repository Tree`

Artifact rules:

- Technical
- Structured
- Clear
- No opinions
- No roadmap
- No improvement suggestions unless the user explicitly asks
- Repository tree must be summarized and include relevant entries only

## Required PRD Rules

- Must preserve the template structure from `references/PRD_TEMPLATE.md`
- Must never remove template sections
- Must preserve existing `Open Features` and `Completed Features` entries
- Must leave placeholders when evidence is insufficient
- Must adapt content to detected project type
- May include inferred functional description only when grounded in code or docs evidence

## Required PROJECT_MAP Rules

- Update incrementally if it already exists
- Keep it factual and technical
- Include at minimum:
  - repo overview
  - entrypoints
  - architectural layers if present
  - key modules or packages
  - key flows
  - external dependencies or integrations
  - tests presence / location / framework
  - commands / runbook
- Do NOT add speculative opinions
- Do NOT map dependencies per module unless explicitly asked

## Required Output Rules

You must produce exactly these outputs:

- `.context/PRD.md`
- `.context/PROJECT_MAP.md`
- `.context/research/<TIMESTAMP>_codebase.md`
- concise pt-BR summary containing:
  - whether Stage 0 completed
  - Stage 1 volume assessment
  - whether deep scan was performed or stopped for authorization
  - files updated under `.context/`

Output constraints:

- Do NOT modify source code
- Do NOT modify tests
- Do NOT continue past Stage 1 risk without explicit user authorization
- Do NOT present guesses as facts
- Do NOT overwrite existing feature tracking in PRD

## Reference Material

Primary rule file:

- `references/RESEARCH_CODEBASE_GENERIC.md`

Reference templates:

- `references/PRD_TEMPLATE.md`
- `references/PROJECT_MAP_TEMPLATE.md`
