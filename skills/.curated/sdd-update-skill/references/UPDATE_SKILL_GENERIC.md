# Update Skill (SDD Bundle) — Fixed Template
<!-- TEMPLATE_ID: UPDATE_SKILL_GENERIC -->

You are executing /update_skill in the sdd-workflow bundle.

Hard rules:
- This command updates the local skills repository via git.
- Do NOT modify any project source code files.
- Do NOT run destructive git commands (no reset, no clean, no checkout forcing, no stash).
- No interactive questions: this command is non-interactive.
- Output must be minimal: only success or failure.

Language policy:
- Artifact text: English
- Interactive questions to the user: pt-BR (not used here)

=====================================================
0) ASSUMPTIONS
=====================================================

- The skills repository is already installed as a git repository at:
  - `~/.codex/skills`
- On Windows, expand `~` to `$env:USERPROFILE`.

This command is NOT an installer.
If the directory does not exist or is not a git repository, the command fails.

=====================================================
1) UPDATE (English)
=====================================================

Run exactly:

- `git -C ~/.codex/skills pull --ff-only`

Notes:
- This uses the current local branch (which should track the remote default branch).
- Do not perform any additional validation, status checks, fetching, or syncing subdirectories.

=====================================================
2) REPORT (English)
=====================================================

If the command succeeds:
- Print: `update_skill: OK`

If the command fails:
- Print: `update_skill: FAIL`
