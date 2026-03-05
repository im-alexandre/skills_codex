# RESEARCH_CODEBASE_GENERIC.md

# SDD Workflow --- Deterministic Edition (Semi‑Nuclear Intelligent Mode)

## Purpose

Perform a deep structural analysis of the entire repository in order to:

-   Understand the architecture
-   Identify core modules and entrypoints
-   Detect main external dependencies and their roles
-   Infer the functional purpose of the system
-   Identify macro flows
-   Detect presence and structure of automated tests
-   Provide inputs to generate/update PRD.md and PROJECT_MAP.md

This command never modifies source code.

------------------------------------------------------------------------

# Execution Model

## Stage 0 --- Mandatory Context Read

Before any scan or size estimation, always read (if present):

-   README.md
-   AGENTS.md
-   .gitignore
-   Detect context folders (.claude/, .codex/, context/, docs/, etc.)

Purpose: - Understand project intent - Understand ignore rules -
Understand automation context

------------------------------------------------------------------------

## Stage 1 --- Repository Volume Report (MANDATORY)

Before deep scanning, estimate:

-   Approximate number of relevant files (excluding ignored/binary/data
    files)
-   Approximate repository size
-   Predominant language
-   Signals of monorepo or multi-application structure
-   Presence of large generated/vendor folders

Ignored automatically:

-   .csv, .sqlite, .db, .xlsx, .xls, .docx, .pdf
-   Binary files
-   Dumps and exported data
-   **pycache**, node_modules, dist, build, .venv, etc.
-   Everything excluded by .gitignore + heuristic complement

If risk is detected: - STOP - Warn about cost/context risk - Ask for
authorization to continue

Never proceed automatically in doubt.

------------------------------------------------------------------------

# Scan Strategy --- Semi-Nuclear Intelligent

-   Always perform full structural tree analysis
-   Identify core directories using:
    -   Language conventions (src/, app/, core/, etc.)
    -   Entrypoints (main, manage.py, app.py, index.ts, etc.)
-   Perform dynamic depth analysis based on directory size:
    -   Small → full read
    -   Medium → selective structural read
    -   Large → sampling + orchestration files

If underestimated size is detected during deep scan: - STOP - Warn -
Suggest reduced mode

------------------------------------------------------------------------

# Output Artifacts

## 1. context/research/`<timestamp>`{=html}\_codebase.md

Header must include:

-   Timestamp
-   Current branch

Required sections:

1.  Repository Overview
2.  High-Level Architecture
3.  Macro Flows
4.  Module Structure
5.  External Dependencies (name + inferred role)
6.  Test Presence (location + framework)
7.  Repository Tree (summarized, relevant only)

Style: - Technical - Structured - Clear - No opinions - No roadmap - No
improvements

------------------------------------------------------------------------

## 2. PRD.md

-   Must use PRD_TEMPLATE.md
-   Never remove template sections
-   Leave empty sections with placeholders if needed
-   Adapt content based on detected project type
-   Include:
    -   Technical description
    -   Functional description (inferred from code)
    -   Actors
    -   Macro flows
-   Preserve existing Open Features / Completed Features

If PRD exists: - Read it - Update it - Never delete sections

------------------------------------------------------------------------

## 3. PROJECT_MAP.md

-   Update incrementally if exists
-   Must include:
    -   Directory/module map
    -   Identified entrypoints
    -   Architectural layers (if present)
-   No business inference
-   No dependency mapping per module

------------------------------------------------------------------------

# Determinism Level

Research phase allows structural freedom. Determinism increases in later
stages (plan / implement).

This command is analytical, not mechanical.
