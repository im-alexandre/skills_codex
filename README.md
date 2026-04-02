# SDD Workflow

Repositorio de skill para Codex focada em Spec-Driven Development (SDD). O pacote publicado aqui vive em [`skills/.curated/sdd-workflow`](skills/.curated/sdd-workflow) e organiza um fluxo disciplinado para pesquisar, planejar, implementar e encerrar features com agentes de IA sem cair no "aceita tudo e depois a gente ve" do `vibe coding`.

## O que e SDD e por que isso importa

SDD desloca a disciplina de engenharia para artefatos que a IA consegue seguir com menos ambiguidade: especificacoes, constraints, testes e manifestos de escopo. Em vez de pedir "faz isso ai" e aceitar diffs sem leitura, voce primeiro esclarece o problema, congela o plano, limita o blast radius e so depois implementa.

## Estrutura do repositorio

```text
skills/
  AGENTS.md
  .curated/
    sdd-workflow/
      SKILL.md
      agents/openai.yaml
      references/
        RESEARCH_CODEBASE_GENERIC.md
        RESEARCH_FEATURE_GENERIC.md
        PLAN_FEATURE_GENERIC.md
        IMPLEMENT_FEATURE_GENERIC.md
        PRD_TEMPLATE.md
        PROJECT_MAP_TEMPLATE.md
```

Arquivos importantes:

- [`skills/.curated/sdd-workflow/SKILL.md`](skills/.curated/sdd-workflow/SKILL.md): regras da skill, comandos disponiveis e fluxo de fechamento.
- [`skills/.curated/sdd-workflow/agents/openai.yaml`](skills/.curated/sdd-workflow/agents/openai.yaml): metadados da interface/agent.
- [`skills/.curated/sdd-workflow/references/`](skills/.curated/sdd-workflow/references): templates e procedimentos operacionais usados por cada comando.
- [`skills/AGENTS.md`](skills/AGENTS.md): convencoes deste repositorio.

## Getting started

### Instalação 

#### Aproveita que tá usando IA e pede para o Codex usar a skill `skill-installer`. segue o prompt:

```text
Use a skill skill-installer para instalar a skill deste repositorio:
- repo: im-alexandre/skills_codex
- path: skills/.curated/sdd-workflow
```

Depois da instalacao, reinicie o Codex para carregar a nova skill.

## Como o workflow funciona

O pacote unifica cinco comandos:
1. `/research_codebase`
2. `/research_feature`
3. `/plan_feature`
4. `/implement_feature`
5. `/close_feature`

O fluxo foi desenhado para aumentar o determinismo fase a fase:

- Pesquisa: ampla e somente leitura.
- Planejamento: congelamento de escopo em um `MANIFEST` classificado em `Read`, `Modify` e `Create`.
- Implementacao: execucao estrita dentro do `MANIFEST`, por fase, com verificacao.
- Fechamento: atualizacao do estado da feature em `.context/PRD.md`.

Antes de qualquer comando, a skill faz bootstrap dos artefatos operacionais:

- `.context/`
- `.context/research/`
- `.context/plan/`
- `.context/specs/`
- `.context/impl/`
- `.context/PRD.md`
- `.context/PROJECT_MAP.md`

Isso transforma o repositorio de trabalho em um ambiente onde a IA nao age "no escuro": ela passa a operar sobre contexto pesquisado, artefatos cumulativos e regras explicitas.

## Comandos disponiveis

### `$sdd-workflow research_codebase`

Objetivo:

- analisar a estrutura inteira do repositorio
- identificar arquitetura, entrypoints, dependencias, testes e macro fluxos
- atualizar ou criar `.context/PRD.md` e `.context/PROJECT_MAP.md`
- gerar um artefato em `.context/research/`

Quando usar:

- no inicio do trabalho em um repo desconhecido
- quando o contexto do sistema ainda esta raso

Exemplo:

```text
$sdd-workflow research_codebase
```

Resultado esperado:

- um diagnostico tecnico do repo
- um `PRD.md` inicial
- um `PROJECT_MAP.md` operacional

### `$sdd-workflow research_feature`

Objetivo:

- pesquisar uma unica feature antes de planejar
- levantar problema, impacto, limites, arquivos relevantes e padroes existentes
- registrar um "Current State Snapshot" reutilizavel no planejamento

Como funciona:

- a skill faz um wizard sequencial em pt-BR
- pergunta slug, problema, arquivos, docs, repos externos, constraints e criterios de aceitacao
- cria um documento em `.context/research/feature_<slug>_<timestamp>.md`
- adiciona a feature em `## Open Features` do `.context/PRD.md`

Exemplo:

```text
$sdd-workflow research_feature
```

Exemplo de caso de uso:

```text
Quero pesquisar a feature `exportar-relatorio-xlsx`.
Problema: o sistema so exporta CSV e o financeiro precisa manter formulas no Excel.
Arquivos conhecidos: src/reports, src/export, tests/reports.
```

### `$sds-workflow plan_feature`

Objetivo:

- converter a pesquisa em um plano executavel
- congelar o escopo em um `MANIFEST` global e por fase
- definir fases, mudancas por arquivo e testes planejados

Como funciona:

- le a feature em aberto no `.context/PRD.md`
- reutiliza o `Current State Snapshot` da pesquisa
- cria `.context/plan/feature_<slug>_<timestamp>.md`
- registra o plano de forma que `$sdd-workflow implement_feature` nao precise "pensar ou inventar muito"

Exemplo:

```text
$sdd-workflow plan_feature 
```

Exemplo de caso de uso:

```text
Planeje a feature `exportar-relatorio-xlsx` em fases pequenas, sem sair dos diretorios ja autorizados.
```

### `$sdd-workflow implement_feature`

Objetivo:

- executar um plano aprovado com disciplina de escopo
- aplicar mudancas por fase
- pedir/apresentar diffs minimos
- rodar verificacoes e registrar log de implementacao

Como funciona:

- le o plano em `.context/plan/`
- valida `MANIFEST` global e por fase
- executa apenas dentro dos arquivos permitidos
- nao pode re-pesquisar nem re-planejar
- atualiza checkboxes do plano quando os testes passam
- grava `.context/impl/feature_<slug>_<date>.md`

Exemplo:

```text
$sdd-workflow implement_feature
```

Exemplo de caso de uso:

```text
Implemente o plano `.context/plan/feature_exportar-relatorio-xlsx_20260401-1030.md`.
```

### `$sdd-workflow close_feature`

Objetivo:

- fechar o ciclo documental da feature no `.context/PRD.md`
- mover a feature de `Open Features` para `Completed Features`
- preservar slug, descricao curta e adicionar a data de conclusao

Como funciona:

- le as features abertas
- confirma qual deve ser encerrada
- altera apenas `.context/PRD.md`

Exemplo:

```text
$sdd-workflow close_feature
```

Exemplo de caso de uso:

```text
Feche a feature `exportar-relatorio-xlsx`.
```

## Workflow recomendado

Para usar a skill como ela foi pensada:

1. Rode `$sdd-workflow research_codebase` quando estiver entrando em um repo novo.
2. Rode `$sdd-workflow research_feature` para cada feature relevante.
3. Rode `T$sdd-workflow plan_feature` e revise o `MANIFEST` antes de aprovar.
8. Rode `$sdd-workflow implement_feature` somente depois que o plano estiver coerente.
9. Rode `$sdd-workflow close_feature` quando a feature tiver sido entregue e verificada.

Esse encadeamento existe para evitar tres problemas comuns de agentes de codigo:

- escopo difuso
- alteracoes fora do blast radius esperado
- codigo sem teste, sem trilha documental e sem criterio claro de aceite

## Referencias

- Thoughtworks, "Spec-driven development: Unpacking one of 2025's key new AI-assisted engineering practices"  
  https://www.thoughtworks.com/insights/blog/agile-engineering-practices/spec-driven-development-unpacking-2025-new-engineering-practices
- Thoughtworks, "The Future of Software Engineering - Retreat Findings"  
  https://www.thoughtworks.com/content/dam/thoughtworks/documents/report/tw_future%20_of_software_development_retreat_%20key_takeaways.pdf
- Simon Willison, "Not all AI-assisted programming is vibe coding (but vibe coding rocks)"  
  https://simonwillison.net/2025/Mar/19/vibe-coding/
- Repository source for this skill  
  [`skills/.curated/sdd-workflow`](skills/.curated/sdd-workflow)
