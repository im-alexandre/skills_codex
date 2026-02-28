===============================================================================
HARD SCOPE MODE — IMPLEMENTATION DISCIPLINE (MANDATORY)
===============================================================================

This command executes an APPROVED implementation plan.

You are in STRICT EXECUTION MODE.

Allowed inputs:
- The selected plan file (.codex/context/plans/...)
- The single file currently being modified
- Files explicitly listed in the plan MANIFEST (global + current phase)

Forbidden actions:
- Do NOT scan the repository.
- Do NOT run repo-wide searches (NO: `rg ... .`, NO `rg` without explicit path).
- Do NOT re-research.
- Do NOT re-plan.

Absolute prohibitions unless explicitly authorized in MANIFEST:
- .env / secrets
- CI/CD configs
- infra outside project scope
- files outside the target project root

Execution rules:
1. Parse the plan.
2. Extract Global MANIFEST + per-phase MANIFEST.
3. Execute phase by phase.
4. Work on ONE file at a time.
5. Show minimal diff.
6. Ask approval: (y/n/a/f(arquivo inteiro)/b(até o talo))
7. Apply only if approved.

Approval controls:
- y: apply this diff (next diff prompt)
- n: reject
- a: approve the entire current file (keep showing diffs, no prompts for this file)
- f: approve rest of current phase (keep showing diffs, no prompts until phase ends)
- b: approve until the end (keep showing diffs, no prompts until completion)

===============================================================================

# Implement Feature (SDD Bundle) — Fixed Template (v2)

You are executing /implement_feature in the sdd-workflow bundle.

Hard rules:
- Follow the approved plan strictly.
- Do NOT invent new scope.
- Do NOT skip phases.
- Implementation must be incremental and verifiable.
- Never modify existing tests to “make it pass”.
  - Only change tests when it is a legitimate, planned behavior change.

Language policy:
- Logs/artifacts: English
- Interactive communication: pt-BR

=====================================================
0) BOOTSTRAP
=====================================================

Ensure:
- .codex/context/impl/ exists
- .codex/PRD.md exists

If no plan path provided:
Ask (pt-BR):
"Qual plano devemos implementar? (path em .codex/context/plans/)"
→ Wait.

Load the plan fully.

=====================================================
1) VALIDATE PLAN BEFORE EXECUTION
=====================================================

Confirm:
- phases exist and are ordered
- Global MANIFEST exists and is classified (Read/Modify/Create)
- each phase MANIFEST exists and is a subset of the Global MANIFEST
- tests are planned per phase

If inconsistencies exist:
STOP and ask the user (pt-BR) before proceeding.

=====================================================
2) EXECUTE PHASE BY PHASE
=====================================================

For each phase:

1) Announce (pt-BR):
"Executando Fase <N>: <Phase Name>"

2) Implement strictly as described in the plan.
- Keep changes minimal and safe (no overengineering).
- Follow healthy patterns; do not replicate bad patterns blindly.

3) Per-file workflow:
- Show minimal diff
- Prompt: "Aplicar este diff? (y/n/a/f(arquivo inteiro)/b(até o talo))"

4) Tests:
- Run verification commands planned for this phase.
- If this phase is a refactor OR user explicitly required baseline:
  - run baseline BEFORE refactor (as planned)
  - run tests AFTER refactor (as planned)

If tests fail:
- Attempt to fix within scope + within phase MANIFEST.
- If a fix requires scope/manifest/plan change:
  - STOP, present structured diagnosis + necessity
  - propose minimal delta (added Read/Modify/Create entries)
  - show summary + diff of plan/manifest changes
  - wait for explicit approval (y/n)
- If cannot reach PASS within allowed scope:
  - STOP and present structured diagnosis + minimal suggestions.

5) Mark phase items completed in the plan ([ ] → [x]) only after tests PASS.

6) Write an implementation log entry:
- .codex/context/impl/feature_<FEATURE_SLUG>_<DATE_YYYYMMDD>.md

Log frontmatter (if new):
---
date: "<ISO-8601 with timezone>"
command: "/implement_feature"
feature_slug: "<FEATURE_SLUG>"
plan_path: ".codex/context/plans/..."
status: "in-progress"
tags: ["impl","feature"]
---

Log per phase:
## Phase <N>: <Phase Name>
### Summary of Changes
- Modified:
- Created:

### Verification
- Automated:
  - <command> — PASS/FAIL

### Notes
- <observations>

=====================================================
3) FINALIZATION
=====================================================

After all phases are completed:
- Run any final verification commands listed by the plan.
- Implementation is COMPLETE only if all planned tests PASS.
- Provide a pt-BR summary:
  - phases completed
  - files changed count
  - commands executed for verification + PASS/FAIL

If any test is FAIL, do not declare completion.
