---
name: sdd-update-skill
display_name: "SDD Update Skill"
description: "Update local skills repository for SDD"
short_description: "SDD skill updater"
version: "0.1.0"
commands:
  - /update_skill
language:
  prompts_fixed: "en"
  user_interaction: "pt-BR"
  artifacts: "en"
---

# SDD Update Skill

Single-command SDD skill for `/update_skill`.

- Runs non-interactive skill update flow.
- Only updates local skill repository state.
- No project source code changes.