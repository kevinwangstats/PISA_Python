# PISA Python: Master Architecture, Multi-Agent System & Portfolio Strategy

---

## 1. Executive Summary & Strategic Value Proposition

**PISA Python** is an enterprise-grade, pure-Python data engineering platform, multi-agent curation framework, and AI knowledge base. It ingests, harmonizes, standardizes, and enriches the longitudinal survey data and official technical documentation of the OECD Programme for International Student Assessment (PISA)—spanning 8 triennial survey cycles (2000–2022), PISA for Development (PISA-D), dozens of participating nations, and millions of student records.

This project is architected as the **flagship portfolio centerpiece** demonstrating Dr. Kevin Wang's industry leadership in **Data Engineering**, **Multi-Agent AI Systems**, **Data Product Design**, and **Mathematical Reproducibility**.

```
┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    STRATEGIC VALUE PILLARS                                       │
├──────────────────────────┬──────────────────────────┬─────────────────────────┬──────────────────┤
│ 1. MULTI-AGENT SYSTEM    │ 2. DATA CURATION & ELT   │ 3. DOCUMENT INTELLIGENCE│ 4. MODERN STACK  │
│ Collaborative Developer, │ Transparent curation     │ Official OECD manuals,  │ Embedded DuckDB  │
│ Reviewer, DocIntel, and  │ matrices, Pandera data   │ frameworks & codebooks  │ OLAP, Parquet,   │
│ PM agents with automated │ contracts & mathematical │ parsed into AI-ready    │ PyArrow IPC, &   │
│ self-correction loops.   │ primary key proofs.      │ chunked RAG knowledge.  │ Hugging Face.    │
└──────────────────────────┴──────────────────────────┴─────────────────────────┴──────────────────┘
```

---

## 2. Platform Architecture Diagram

```
===================================================================================================
                                  PISA PYTHON PLATFORM ARCHITECTURE
===================================================================================================

  [1. RAW INGESTION (Bronze)]        [2. DOCUMENT INTELLIGENCE & AI]        [3. HARMONIZATION (Gold)]
  ┌─────────────────────────┐        ┌─────────────────────────────┐        ┌─────────────────────┐
  │ 28GB Symlinked Lake     │        │ Official OECD Manuals,      │        │ Longitudinal Harmon-│
  │ SPSS (.sav) & FWF (.txt)│        │ Frameworks & Questionnaires │        │ ization (2000-2022  │
  │ + PISA-D Survey Data    │        │ (PDFs / HTML / Excel)       │        │ + PISA-D Datasets)  │
  └────────────┬────────────┘        └──────────────┬──────────────┘        └──────────┬──────────┘
               │                                    │                                  │
               ▼                                    ▼                                  ▼
  [Dynamic Syntax Parsing]           [LlamaParse / DocIntel Agent]          [Stratified OECD Sampling]
  - Zero-copy column extraction      - Hierarchical Markdown chunking       - 38 Member Countries
  - Multi-test booklet joins         - Structured schema extraction         - 50 Samples/Year Bench
  - Value labels & NA masks          - Issue #14 3-way reconciliation       - Declarative CSV Matrices
               │                                    │                                  │
               └──────────────────┬─────────────────┘                                  │
                                  ▼                                                    │
                   [SILVER STANDARDIZATION LAYER]                                      │
                   - Pandera DataFrame runtime contract validation                     │
                   - SAS notation stripping (.N, .M, .V -> integer NA)                 │
                   - Standardized ISCED education & asset decodings                    │
                   - Mathematical Primary Key uniqueness proofs                        │
                                  │                                                    │
                                  └─────────────────────┬──────────────────────────────┘
                                                        ▼
                                         [PLATINUM & AI-READY PLATFORM]
                                         ┌──────────────────────────────────────────┐
                                         │ - Embedded Serverless DuckDB OLAP Engine │
                                         │ - Hive-Partitioned Snappy Parquet Lake   │
                                         │ - AI Agent RAG Vector & Knowledge Store  │
                                         │ - Hugging Face Datasets & Arrow Streams  │
                                         │ - Automated Missingness Heatmaps & Logs  │
                                         └──────────────────────────────────────────┘
===================================================================================================
```

---

## 3. Core Strategic Pillars

### Pillar 1: Multi-Agent System Architecture & Collaborative Lifecycle
The platform is developed and maintained via a multi-agent paradigm with specialized agent personas:
- **Developer Agent**: Writes clean, modular, fully typed (`mypy`), and documented Python code. Adheres to "Transform Late" (ELT).
- **Reviewer / Auditor Agent**: Enforces Pandera contracts, checks primary key uniqueness invariants ($|\text{Distinct}(PK)| = |Rows|$), verifies MD5 manifests, and inspects golden baseline regressions against legacy R outputs.
- **Document Intelligence Agent**: Parses unstructured OECD documentation (manuals, questionnaires, frameworks) into structured, chunked, AI-ready knowledge bases.
- **Product Manager & Governance Agent**: Enforces the Zero Fabrication rule, career portfolio alignment, and executive narrative.
- **Automated Self-Correction**: When the Reviewer Agent flags an anomaly, the Developer Agent introspects physical raw binary headers (`pyreadstat`), isolates the root cause, and applies a declarative fix.

### Pillar 2: High-Quality Data Curation Beyond OECD
OECD PISA data is historically messy, distributed across 8 triennial cycles with shifting variable names, renumbered questionnaire items, and inconsistent missing value codes (`.M`, `.N`, `.V`, `95-99`).
- **Comprehensive Resolution of Anomalies (Issue #14)**: Automated 3-way reconciliation comparing extracted codebook JSON, declarative CSV matrices, and physical `.sav` binary headers.
- **Transparent Curation**: Every factor recoding (`isced3a1`, `fe1ma2`, `book_levels_6`) is declared in version-controlled CSV matrices (`variable_curation/`).
- **Full Survey Scope**: Ingests the full canonical 2000–2022 student, school, and teacher datasets plus PISA for Development (PISA-D) datasets.

### Pillar 3: AI Document Intelligence & RAG Knowledge Base
- **Unstructured Documentation to AI-Ready Knowledge**: Official OECD technical reports, methodology frameworks, and survey questionnaires are chunked and converted into structured Markdown and vector embeddings.
- **Zero-Hallucination Querying for AI Agents**: Downstream AI agents can query the exact methodology, psychometric plausible value scaling, and survey administration guidelines with full citation lineage.

### Pillar 4: Mathematical Reproducibility & Modern Data Stack
- **Zero-Copy Symlink Data Lake**: Bridges the ~28GB raw data lake with zero redundant disk usage.
- **Embedded DuckDB OLAP Database**: Enables sub-second analytical SQL queries across millions of longitudinal student records.
- **Columnar Parquet & Arrow Serving**: Hive-partitioned (`year=YYYY/country=CCC/`) Snappy Parquet tables and direct export to Hugging Face `DatasetDict` and Apache Arrow IPC.

---

## 4. Phased Implementation Roadmap

| Phase | Title | Focus Area | Detailed Spec |
|---|---|---|---|
| **Phase 1** | **Foundation, Storage Bridge & Acquisition** | Symlink lake architecture (28GB), PISA-D raw acquisition, cryptographic MD5 manifests, and packaging (`pyproject.toml`). | [`PHASE1_FOUNDATION_AND_STORAGE_BRIDGE.md`](file:///Users/kevinwang/projects/PISA_Python/docs/PHASE1_FOUNDATION_AND_STORAGE_BRIDGE.md) |
| **Phase 2** | **Curation, Ingestion & AI Document Intelligence** | Pure-Python SPSS/FWF parser, OECD technical manual & framework chunking for RAG, AI codebook extraction, and Issue #14 3-way reconciliation. | [`PHASE2_CURATION_AND_AI_METADATA_ENGINE.md`](file:///Users/kevinwang/projects/PISA_Python/docs/PHASE2_CURATION_AND_AI_METADATA_ENGINE.md) |
| **Phase 3** | **Data Governance, Contracts & Multi-Agent Audits** | Pandera data contracts, primary key uniqueness proofs, missingness heatmaps, and Reviewer Agent audit loops. | [`PHASE3_DATA_GOVERNANCE_CONTRACTS_AND_QUALITY.md`](file:///Users/kevinwang/projects/PISA_Python/docs/PHASE3_DATA_GOVERNANCE_CONTRACTS_AND_QUALITY.md) |
| **Phase 4** | **Modern Data Stack & AI Platform Serving** | Embedded DuckDB database, Snappy Parquet lake, AI RAG vector index, Hugging Face Datasets export, and R-compatibility bridge. | [`PHASE4_MODERN_DATA_STACK_AND_AI_PLATFORM.md`](file:///Users/kevinwang/projects/PISA_Python/docs/PHASE4_MODERN_DATA_STACK_AND_AI_PLATFORM.md) |
| **Phase 5** | **Developer Experience, CLI & Portfolio Showcase** | Rich CLI (`pisa` command), CI/CD pipelines, interactive econometric & AI analytics notebooks, and portfolio presentation. | [`PHASE5_DEVELOPER_EXPERIENCE_CLI_AND_PORTFOLIO_SHOWCASE.md`](file:///Users/kevinwang/projects/PISA_Python/docs/PHASE5_DEVELOPER_EXPERIENCE_CLI_AND_PORTFOLIO_SHOWCASE.md) |

---

## 5. Architectural Comparison Table

| Dimension | Legacy Prototype (`learningtower_masonry`) | Modernized Platform (`PISA_Python`) |
|---|---|---|
| **Core Architecture** | Hybrid R / Python scripts | Pure Python 3.11+ Multi-Agent Data Platform |
| **Document Intelligence** | Manual inspection of PDFs | Automated LlamaParse / Chunked Markdown Knowledge Base for RAG |
| **Data Scope** | 2000–2022 Core Student & School | 2000–2022 Core + PISA-D + Official Manuals & Frameworks |
| **Storage & Serving** | R `.rds` / `.rda` files | Parquet (Snappy, Partitioned), DuckDB, Hugging Face Hub, Arrow IPC |
| **Schema Validation** | Custom R audit scripts | Declarative Pandera DataFrameModels & Pydantic v2 Contracts |
| **Anomaly Handling** | Ad-hoc R fixes for Issue #14 | 3-Way Automated Reconciliation Engine with Audit Catalog |
| **Multi-Agent Workflow**| Single-developer manual runs | Developer, Reviewer, DocIntel, and PM agents with self-correcting loops |
| **CLI & UX** | Manual Rscript / Rmd rendering | Rich CLI (`pisa` command with Typer and colored terminal UI) |
| **Reproducibility** | Checksum script | Cryptographic MD5/SHA-256 manifests, automated regression CI/CD |
