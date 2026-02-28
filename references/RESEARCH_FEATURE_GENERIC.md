# Research Feature (SDD Bundle) — Fixed Template (v2)

You are executing /research_feature in the sdd-workflow bundle.

Hard rules:

- Read-only: DO NOT modify any source code files.
- You MAY update documentation artifacts:
  - .codex/PRD.md
  - .codex/context/research/*.md
- This command focuses on ONE feature.
- Clarify scope and context — NOT the implementation plan yet.
- Do NOT propose solutions unless explicitly asked.

Determinism policy:
- Research may be broad to avoid missing context.
- Determinism will increase in /plan_feature and be maximal in /implement_feature.

Language policy:
- Artifacts produced/updated by this command: English
- Interactive questions to the user: pt-BR

# ===================================================== 0) BOOTSTRAP

Ensure:
- .codex/context/research/ exists
- .codex/PRD.md exists (create from references/PRD_TEMPLATE.md if missing)

=====================================================

1) FEATURE DISCOVERY WIZARD (pt-BR — SEQUENTIAL MODE)

Ask ONE question at a time.
Wait for the user's answer before asking the next question.
Do NOT show future questions in advance.

Q1. Qual é o nome curto da feature? (slug em kebab-case)
→ Wait.

Q2. Qual problema real ela resolve? (comportamento observável)
→ Wait.

Q3. Quais diretórios/arquivos você já sabe que estão envolvidos? (opcional, mas recomendado)
- Isso é um ponto de partida forte (não exclusivo). Priorize começar por aqui.
→ Wait.

Q4. Documentação relevante? (URLs ou paths locais)
→ Wait.

Q5. Repositórios externos / repos locais / diretórios com funcionalidades parecidas? (links ou paths)
→ Wait.

Q6. Observações técnicas gerais? (pode colar snippets aqui)
→ Wait.

Q7. Quais constraints não-negociáveis? (segurança, compliance, performance, compatibilidade)
→ Wait.

Q8. Além de testes unitários (obrigatório), existe algum critério adicional de aceitação?
→ Wait.

Q9. Observações adicionais? (qualquer nuance que possa impactar)
→ Wait.

If the user provides multiple answers at once, map them to the corresponding questions and continue with the next unanswered question.

Only after all answers are collected, proceed to step 2.

# ===================================================== 2) DEFINE FEATURE CONTEXT (English)

Based on the answers (and any relevant files provided):

- Clarify boundaries and acceptance criteria at a high level.
- Identify impacted modules/directories and likely integration points.
- Read broadly as needed to avoid missing dependencies (no interactive confirmation needed during research).
- Compare against external repos/docs if provided:
  - identify reusable patterns
  - identify mismatches/gaps
- Record assumptions explicitly.
- Record open questions that require decisions (keep them minimal).

Do NOT:
- write implementation steps
- estimate effort
- critique architecture
- propose refactors

# ===================================================== 3) CREATE RESEARCH DOCUMENT (English)

Create:

Path:
.codex/context/research/feature_<FEATURE_SLUG>_<TIMESTAMP_YYYYMMDD-HHMM>.md

Frontmatter:
---
date: "<ISO-8601 with timezone>"
command: "/research_feature"
feature_slug: "<FEATURE_SLUG>"
status: "research-complete"
tags: ["research","feature"]
---

Body structure:

# Feature Research: <Feature Name>

## Problem Statement
## User Impact

## Scope Boundaries
### In Scope
### Out of Scope

## Authorized Files (Relevant)
List files AND directories relevant to this feature.
No Read/Write classification here.

- <path>
- <path>

## Current State Snapshot (for planning)
2–5 bullets summarizing what exists today, suitable for /plan_feature to reuse verbatim.

- <bullet>
- <bullet>

## Technical Context
- Starting scope provided by user (if any)
- Impacted modules
- Related flows
- Existing patterns

## External References (if provided)
- Docs
- External repos / similar implementations

## Reusable Patterns Identified (ONLY if applicable)
- <pattern>

## Constraints
## Assumptions
## Open Questions (minimal)

# ===================================================== 4) UPDATE `.codex/PRD.md`

Under:
- ## Open Features

Add (if missing):
- [ ] <FEATURE_SLUG> — short human-readable description

Do NOT move to Completed.

# ===================================================== 5) PRESENT SUMMARY TO USER (pt-BR)

Return:
- Resumo da feature (escopo e limites)
- Diretórios/arquivos relevantes (alto nível)
- Documento criado (path)
- Pergunte se deseja avançar para /plan_feature
