# scripts/

R only. Numbered by pipeline stage, e.g.:

- `01_pull_*.R` — API extraction (incremental, resumable, rate-limit aware)
- `02_clean_*.R` — preprocessing, stable identifiers (incl. release-year identifier)
- `03_merge_*.R` — joins with covariate datasets (doom indices etc.)
- `04_analyze_*.R` — estimation
- `05_output_*.R` — figures/tables into outputs/

Pipeline tooling (targets vs. plain scripts) is an open decision — DECISIONS.md O6.
Each script states its inputs, outputs, and the brief (briefs/) it implements.
