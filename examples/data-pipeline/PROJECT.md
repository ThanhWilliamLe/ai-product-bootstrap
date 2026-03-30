# DataPipe

CSV/JSON ingestion with YAML-driven transforms, outputting to PostgreSQL or BigQuery. Built for the internal analytics team (15 people).

## Goals

- G1: Reliable ingestion — read CSV and JSON files without data loss or silent corruption
- G2: YAML transforms — users define transformation rules in YAML, no code required
- G3: PostgreSQL output — write transformed data to PostgreSQL with schema management
- G4: BigQuery output — write transformed data to BigQuery with partitioning and cost control
- G5: Airflow orchestration — schedule and monitor pipelines via Airflow DAGs
- G6: dbt integration — use dbt for warehouse-side transforms and data quality tests

## State

Version: 0.1
Phase: discovery
Last shipped: — (none yet)

## Queue

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 1 | Research BigQuery connector options (google-cloud-bigquery vs. alternatives, auth patterns, streaming vs. batch, cost model) | G4 | research | — | ready |
| 2 | Define YAML schema for transform rules (field mapping, filtering, type casting, joins, aggregations) | G2 | decide | — | ready |
| 3 | Scaffold project structure (src/, tests/, dags/, dbt/, pyproject.toml, ruff/mypy config) | G1 | build | — | ready |
| 4 | Implement CSV/JSON reader with schema inference and validation | G1 | build | 3 | — |
| 5 | Implement YAML config parser with schema validation (pydantic models) | G2 | build | 2, 3 | — |
| 6 | Build transform engine that executes YAML-defined rules on DataFrames | G2 | build | 4, 5 | — |
| 7 | Build PostgreSQL output connector (SQLAlchemy, upsert, schema migration) | G3 | build | 3, 6 | — |
| 8 | Build BigQuery output connector (google-cloud-bigquery, partitioning, load jobs) | G4 | build | 1, 6 | — |
| 9 | Create Airflow DAG template using TaskFlow API for ingest→transform→load | G5 | build | 6, 7 | — |
| 10 | Integrate dbt models for post-load warehouse transforms and tests | G6 | build | 7, 8 | — |
| 11 | Integration tests for PostgreSQL connector (Docker-based) | G3 | verify | 7 | — |
| 12 | Integration tests for BigQuery connector (emulator or sandbox) | G4 | verify | 8 | — |
| 13 | Research Airflow error handling and retry patterns for data pipelines | G5 | research | — | ready |
| 14 | Document YAML reference with examples for all supported transform types | G2 | build | 6 | — |

## Decisions

- D1: No GUI — YAML config files only — constrains: all user interaction is via config files and CLI, no web interface
- D2: Python + ruff + mypy strict — constrains: all code must pass `ruff check .` and `mypy .` with strict mode
- D3: 2-person backend team — constrains: favor simplicity over abstraction, minimize operational surface area

## Done

(none yet)
