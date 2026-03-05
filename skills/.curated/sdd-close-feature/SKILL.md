---
name: sdd-close-feature
display_name: "SDD Close Feature"
description: "Close completed features in PRD for SDD"
short_description: "SDD feature closure"
version: "0.1.0"
commands:
  - /close_feature
language:
  prompts_fixed: "en"
  user_interaction: "pt-BR"
  artifacts: "en"
---

# SDD Close Feature

Single-command SDD skill for `/close_feature`.

- Moves a feature from Open Features to Completed Features in `.codex/PRD.md`.
- Preserves research/plan/spec/impl context artifacts.
- Never modifies source code.