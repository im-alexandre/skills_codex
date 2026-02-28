# Plan Feature (SDD Bundle) — Fixed Template (v2)

You are executing /plan_feature in the sdd-workflow bundle.

Hard rules:

- This command produces an actionable implementation plan.
- Read-only: DO NOT modify any source code files.
- You MAY update documentation artifacts:
  - .codex/PRD.md
  - .codex/context/plans/*.md
  - .codex/context/specs/*.md (optional)
- No risk/complexity estimates. Focus only on an executable plan.
- The plan must be complete and executable.
- Prefer reuse: libraries/dependencies over reimplementing functionality.
- Avoid overengineering (KISS). Follow existing patterns only when they are healthy.

Determinism policy:
- /plan_feature freezes scope via MANIFEST (global + per-phase).
- /implement_feature will be restricted to this frozen MANIFEST.

Language policy:
- Artifacts produced/updated by this command: English
- Interactive questions to the user: pt-BR

# ===================================================== 0) BOOTSTRAP

Ensure:
- .codex/context/plans/ exists
- .codex/context/specs/ exists
- .codex/PRD.md exists

Research baseline rules:
- If feature research exists: use the latest one automatically.
- Else: use the latest /research_codebase artifact as baseline.

=====================================================

1) SELECT FEATURE (pt-BR)

Before asking, read section ## Open Features in PRD.md and list them (even if there's only one).

Q1. Qual feature vamos planejar? (slug)
1. <feature_slug_1>
2. <feature_slug_2>
→ Wait.

Load the latest research doc for that feature if available; otherwise load the latest codebase research.

# ===================================================== 2) ANALYZE CURRENT STATE (English)

- Reuse the "Current State Snapshot (for planning)" bullets from the research doc verbatim.
- If you must read additional files beyond research Authorized Files, you MAY do so (no confirmation), and you MUST:
  - include them in the plan's MANIFEST (Read/Modify/Create)
  - inform the user in the pt-BR summary that scope expanded during planning.

If there are contradictions between research and code:
- Note the discrepancy and ask the user (pt-BR) BEFORE freezing the plan.

# ===================================================== 3) DEFINE PHASES (pt-BR)

Propose phases tailored to this feature.

If phases >= 2:
- Ask for confirmation that the phasing/order/granularity is ok.
- Wait.

If phases == 1:
- Proceed without asking.

# ===================================================== 4) CREATE IMPLEMENTATION PLAN DOCUMENT (English)

Create:

Path:
- .codex/context/plans/feature_<FEATURE_SLUG>_<TIMESTAMP_YYYYMMDD-HHMM>.md

Frontmatter:
---
date: "<ISO-8601 with timezone>"
command: "/plan_feature"
feature_slug: "<FEATURE_SLUG>"
research_path: ".codex/context/research/..."
status: "plan-complete"
tags: ["plan","feature"]
---

Plan structure:

# <Feature Name> — Implementation Plan

## Overview
2–6 bullets.

## Current State Snapshot (from research)
Paste the bullets verbatim.

## Desired End State
Verifiable behavior.

## Non-Goals (Out of Scope) (ONLY if scope creep risk exists)
Explicit list.

## MANIFEST (Frozen Scope) — Global
Classify as Read / Modify / Create. Include a short justification per entry (no evidence refs required).

### Read
- `path` — short why

### Modify
- `path` — short why

### Create
- `path` — short why

## Phases

### Phase 1: <Phase Name>

#### Objective
What this phase achieves.

#### MANIFEST (Phase 1)
Subset of Global MANIFEST. Same classification (Read/Modify/Create). Short justification per entry.

#### Changes
- File/Area: `path/to/file.ext`
  - Specific change

#### Tests (planned in this phase)
- Unit tests to add/adjust (sufficient coverage; no universe)
- Verification commands to run after this phase

#### Baseline tests (ONLY if refactor, or user explicitly required)
- Commands to run BEFORE applying refactor
- Commands to run AFTER applying refactor

---

(Repeat per phase)

## Dependencies (ONLY if introducing anything new)
If introducing a new dependency (pip/npm/composer/mason/etc), include:

- Name
- Official documentation link
- Why it is needed
- How it will be used (primary API / integration point)
- Version pin strategy
- Where it will be declared (pyproject/requirements/package.json/etc.)
- Install command

## Migration / Rollback Notes (ONLY if structural change)
Keep concise.

## References
- Research: `<research_path>`
- Key files: `...`

# ===================================================== 5) UPDATE `.codex/PRD.md`

For the feature entry under Open Features:
- Add a short note linking the plan path (do not mark completed).

# ===================================================== 6) PRESENT SUMMARY (pt-BR)

Return:
- Plan file path
- Number of phases
- Any scope expansions done during planning (added files/directories)
- Ask if user wants to proceed to /implement_feature
