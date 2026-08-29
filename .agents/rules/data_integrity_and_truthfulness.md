# Core Rule: Absolute Truthfulness, Data Integrity, and Fact-Checking

## Fundamental Principle
You may use clear, professional, and impactful language that presents the author's work, capabilities, and data engineering achievements in the best light. However, **you must never fabricate inaccuracies, fictitious metrics, unverified benchmarks, or false claims**.

## Mandatory Rules for All Edits and Responses:

1. **Grounded in Ground Truth**:
   - Every file path, column name, variable mapping, NA code, and data schema must reflect the physical source data, codebooks, or actual execution outputs.
   - Never invent or guess variable codes, missing value representations, or dataset properties.

2. **Empirical Benchmarks vs. Target Estimates**:
   - If performance numbers, file size ratios, or query runtimes are measured from actual code execution, cite them accurately with timestamp/environment context.
   - If benchmark metrics are design goals or preliminary estimates, explicitly label them as **Target Architecture Estimates** or **Expected Architectural Benchmarks** rather than claiming them as measured historical facts.

3. **Accurate Attribution & Lineage**:
   - Accurately preserve acknowledgments, contributors, and the historical lineage of the project (e.g., origin in OzUnconf 2019, Monash University, CRAN `learningtower` package).
   - Document any known data anomalies truthfully (e.g. why 2015 dishwasher has missing values, 2022 desk/wealth survey changes).

4. **Self-Verification on Every Edit**:
   - Before completing any task, edit, or documentation pass, cross-check that all statistics, tables, diagrams, and technical statements are fully factual and verifiable.
