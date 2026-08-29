# Phase 4: Modern Data Stack & AI Platform Serving

---

## 1. Objective

Elevate the PISA data pipeline into a modern, cloud-native, and AI-ready platform. This phase delivers:
1. **Embedded High-Performance OLAP Engine**: Zero-configuration, serverless analytical database powered by **DuckDB** (`Data/pisa.duckdb`).
2. **Cloud-Native Columnar Parquet Lake**: Hive-partitioned, Snappy-compressed Parquet files providing massive speedups and compression over legacy formats.
3. **AI Knowledge Base & RAG Index**: Chunked and embedded repository of OECD methodological frameworks and technical reports for zero-hallucination AI agent querying.
4. **AI & Machine Learning Data Ready**: One-command export to **Hugging Face `datasets`**, Apache Arrow IPC streams, and tokenized metadata catalogs.
5. **Cross-Language Backward Compatibility**: R-compatible `.rds` / `.rda` output exporters to seamlessly sync with the CRAN `learningtower` R package.

---

## 2. Storage Tier Architecture (Medallion & Platinum)

```text
===================================================================================================
                                 PLATINUM STORAGE & SERVING TIERS
===================================================================================================

  [Gold Harmonized Data & Document Chunks] (In-Memory Arrow / Pandas)
            │
            ├───────────────────────┬───────────────────────┬───────────────────────┬──────────────┐
            ▼                       ▼                       ▼                       ▼              ▼
   [Partitioned Parquet]    [Embedded DuckDB]     [AI RAG Knowledge Base]   [Hugging Face & Arrow] [R Bridge]
   - Hive partitioning:     - Data/pisa.duckdb    - Data/Documentation/     - DatasetDict          - .rds/.rda
     year=YYYY/country=CCC/ - Analytical views      chunked/ & vector index - Apache Arrow IPC     - Transfer
   - Snappy compression     - Sub-second OLAP     - Methodological RAG      - Hugging Face Hub     - Checksums
   - 90% size reduction     - Direct Python API   - Zero-hallucination agent- ML/LLM Benchmarking
===================================================================================================
```

---

## 3. Embedded DuckDB Analytical Engine (`pisa_python.storage.duckdb_engine`)

DuckDB provides in-process, serverless SQL execution with vectorized execution kernels:

### Key Features
- **Zero-Copy Ingestion**: Reads directly from Parquet files or Arrow tables in memory without serialization overhead.
- **Analytical Views**:
  - `v_student_longitudinal`: Unified student view across all 8 survey cycles.
  - `v_school_longitudinal`: Unified school view.
  - `v_pisa_d_longitudinal`: PISA for Development integrated view.
  - `v_oecd_benchmark_sample`: Stratified sample of 38 OECD countries (50 students/country/cycle).
  - `v_country_trends`: Pre-aggregated score trajectories (Math, Reading, Science) and ESCS indices by country and year.
- **Sub-Second Complex Queries**:
  ```python
  import duckdb
  
  con = duckdb.connect("Data/pisa.duckdb")
  df = con.execute("""
      SELECT 
          year,
          country,
          AVG(math) AS mean_math,
          AVG(read) AS mean_read,
          AVG(science) AS mean_scie,
          CORR(math, escs) AS math_socioeconomic_gradient
      FROM v_student_longitudinal
      WHERE country IN ('USA', 'FIN', 'SGP', 'AUS', 'DEU')
      GROUP BY year, country
      ORDER BY country, year
  """).df()
  ```

---

## 4. AI & Hugging Face Platform Integration (`pisa_python.storage.hf_exporter`)

To empower AI researchers, educational data scientists, and ML practitioners:

1. **Hugging Face `DatasetDict` Export**:
   - Converts harmonized student, school, and benchmark datasets into standard Hugging Face format.
   - Includes rich feature metadata, dataset card (`README.md`), and variable descriptions.
   - Provides one-command upload to the Hugging Face Hub (`pisa export-hf --repo-id user/pisa-longitudinal`).

2. **RAG & LLM Context Engine**:
   - Generates structured schema dictionaries and JSON-LD knowledge graphs describing variable definitions, survey questions, and historical shifts across cycles.
   - Connects with the chunked OECD documentation knowledge base to enable LLMs to accurately query and interpret PISA statistics without hallucinating variable names or NA definitions.

---

## 5. R-Compatibility Bridge (`pisa_python.storage.r_bridge`)

To maintain seamless interoperability with the CRAN `learningtower` R ecosystem:
- **`export_r_data()`**: Uses `pyreadr` / `rpy2` (or standardized feather/csv transfer) to generate exact `.rda` and `.rds` files formatted for R package distribution.
- **`checksums.json`**: Automatically generates transfer manifests with cryptographic hashes matching the `learningtower_masonry` transfer protocol.

---

## 6. Deliverables & Verification for Phase 4

1. ✅ `pisa_python.storage.parquet_lake` (Partitioned Parquet manager).
2. ✅ `pisa_python.storage.duckdb_engine` (DuckDB database & analytical view builder).
3. ✅ `pisa_python.storage.rag_knowledge` (OECD document chunk index & RAG query engine).
4. ✅ `pisa_python.storage.hf_exporter` (Hugging Face Datasets & Arrow exporter).
5. ✅ `pisa_python.storage.r_bridge` (R transfer artifact generator).
6. ✅ Automated tests in `tests/test_duckdb.py`, `tests/test_rag_knowledge.py`, and `tests/test_storage.py`.
