---
name: sdd-plan-feature
display_name: "SDD Plan Feature"
description: "Read-only executable planning for SDD"
short_description: "SDD feature planning"
version: "0.1.0"
commands:
  - /plan_feature
language:
  prompts_fixed: "en"
  user_interaction: "pt-BR"
  artifacts: "en"
---

# SDD Plan Feature

Single-command SDD skill for `/plan_feature`.

- Generates executable implementation plans.
- Freezes MANIFEST scope for implementation.
- Never modifies source code.