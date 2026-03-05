---
name: sdd-implement-feature
display_name: "SDD Implement Feature"
description: "Strict MANIFEST-bound implementation for SDD"
short_description: "SDD strict implementation"
version: "0.1.0"
commands:
  - /implement_feature
language:
  prompts_fixed: "en"
  user_interaction: "pt-BR"
  artifacts: "en"
---

# SDD Implement Feature

Single-command SDD skill for `/implement_feature`.

- Executes only inside the frozen MANIFEST scope.
- Requires explicit approval flow by default.
- Enforces tests as acceptance criteria.