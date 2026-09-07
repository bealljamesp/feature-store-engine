# High-Performance Parquet Feature Store & Validation Engine (`feature-store-engine`)

A production data engineering and feature platform for quantitative time series and financial ML models. Enforces strict runtime data contracts, validates against data leakage and schema drift, and generates rolling risk features via lazy out-of-core execution graphs in Polars.

---

## 1. Architectural & Engineering Directives

- **Runtime & Standards:** Python 3.12+; fully typed interfaces (`mypy --strict`), Ruff linting/formatting, and zero-loop transformations.
- **Data Contracts & Validation:** Ingestion validation using **Pandera** and **Pydantic** verifying schema boundaries: non-null constraints, strict datetime monotonicity, distribution boundaries, and foreign key references.
- **Lazy Feature Engineering:** Pure functional transformations using `polars.LazyFrame` (`scan_parquet()`) to compile optimized execution graphs for rolling features (realized volatility, Parkinson volatility, multi-window RSI, rolling beta).
- **Partitioning & Lineage:** Hive-partitioned Parquet storage with explicit metadata tracking (manifest files, SHA256 data hashes, schema versioning) preventing lookahead bias and training-serving skew.
- **CI/CD Quality Gates:** GitHub Actions workflow running automated schema validation checks, unit tests, and performance benchmarks on incremental data slices.

---

## 2. Directory Layout

```text
feature-store-engine/
├── pyproject.toml
├── README.md
├── src/
│   └── feature_store/
│       ├── __init__.py
│       ├── contracts.py     # Pandera / Pydantic schema validation models
│       ├── store.py         # Partitioned Parquet reader/writer with atomic commits
│       ├── features.py      # Polars lazy execution graph definitions
│       └── registry.py      # Feature metadata catalog and lineage tracking
└── tests/
    ├── test_contracts.py
    └── test_features.py
```
