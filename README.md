<!--
=====================================================================
HOW TO USE THIS TEMPLATE
=====================================================================
1. Replace ALL [bracketed] placeholders with your own info.
2. No claims in the intro — the categories and bullets do the work.
3. Every project bullet: [Verb] + [what] + [tech] + [result/scale].
   No number? Use scope instead ("3 services" beats "robust system").
4. <details> blocks keep the page short until someone clicks into
   the category they care about — use them, don't delete them.
5. Curate PINNED REPOS per job application (6 slots, top of your
   GitHub profile) — that's the tailorable layer. This README is
   the full, permanent catalog underneath.
=====================================================================
-->

<h1 align="center">Hi, I'm Nariman 👋</h1>
<h3 align="center">Data Engineer</h3>

<p align="center">
🔭 Currently: open to Data Engineer roles
</p>

---

🗂️ Projects by Domain

<details open>
<summary><strong>🏙️ Public Sector — Chicago 311 Service Requests</strong></summary>
<br>
Batch lakehouse processing 4.4M+ Chicago 311 service request records (2018–present) for SLA compliance, backlog visibility, and geographic-equity reporting.


Kimball star schema with an explicitly documented SCD strategy per dimension — Type 0 for immutable geography and dates, Type 1 for department names that can change
Two fact tables with different grains: an accumulating-snapshot fact for ticket lifecycle, and a separate transaction fact (completed tickets only) for SLA/equity analysis
Implemented a Write-Audit-Publish pattern via BigQuery audit-table swap, since BigQuery doesn't support native Iceberg branching
Defined and documented explicit pipeline SLAs (freshness, ingestion completion time, failure recovery) separate from the city's own 311 response-time targets
Two Shiny dashboards serving distinct stakeholder personas: daily operational triage vs. weekly executive/SLA reporting
Stack: BigQuery/BigLake Iceberg, dbt, Prefect, Terraform, Shiny, Polars  |  🔗 dezc-capstone-311-chicago-sr


</details>
<details open>
<summary><strong>🧭 Population & Demographics — DemographIQ</strong></summary>
<br>
A pipeline and interactive mapping tool" modeling US socioeconomic patterns from Census ACS, TIGER/Line, and IRS migration data across 2012–2024.


Bronze → Silver → Gold medallion on S3 + Athena + Iceberg, ingesting 3 heterogeneous APIs into one coherent storage layer at state/county/census-tract granularity (~84k tracts)
Orchestrated with Dagster using dynamic partitions + a sensor instead of cron schedules — the pipeline reacts to actual data releases rather than guessing a release date
Interactive choropleth map rendering 84k census tracts via GPU-accelerated WebGL (lonboard/deck.gl) without shipping raw geometry to the browser
Data quality enforced with Dagster asset checks at every layer transition plus dbt tests on the mart models
Stack: Dagster, dbt-athena, Polars, Apache Iceberg, Terraform, Flask, lonboard  |  🔗 population-demographics-de-project


</details>
<details>
<summary><strong>🛢️ Oil & Gas — WellStream</strong></summary>
<br>
Combines a batch market-data lakehouse with a real-time operational telemetry stream, built entirely within a 4GB memory constraint.


Batch: dlt ingests EIA market data into S3, cataloged with DuckLake, transformed with dbt-duckdb, served via Motherduck, orchestrated by Kestra
Streaming: simulated well telemetry flows through Redpanda Serverless → Bytewax (1-minute tumbling windows) → idempotent upserts into Neon serverless Postgres → Grafana
Implemented a dead-letter-queue pattern so malformed IoT payloads are isolated without interrupting the live pipeline
Deliberately offloaded the heaviest components (Kafka broker) to managed cloud tiers to protect local compute for the Python transformation layer
Stack: dlt, DuckDB/DuckLake, dbt, Kestra, Redpanda, Bytewax, Neon Postgres, Terraform  |  🔗 oil-de-pipeline


</details>
<details>
<summary><strong>📈 Marketing — MomentumCRM Customer Intelligence Platform</strong></summary>
<br>
A feature-store-centered platform serving three ML models (churn, campaign response, CLV) from one shared, point-in-time-correct pipeline for a simulated B2B SaaS company.


Built a Dagster asset-based pipeline (Polars transforms) feeding a Feast feature store — S3 offline store, Redis online store for sub-10ms serving lookups
Designed the pipeline specifically to prevent training/serving skew: the same build_feature_table(as_of_date) function powers both training and serving paths
Simulated 50k customers with persona-conditioned behavioral state machines (not random data) so churn and engagement signals are genuinely learnable
Served predictions via FastAPI, with models tracked and versioned through the MLflow Model Registry
Stack: Dagster, Polars, Feast, LightGBM, MLflow, FastAPI, Redis, Terraform, Docker  |  🔗 momentumcrm-marketing-de-project


</details>
<details>
<summary><strong>⚡ Energy & IoT — Wattstream</strong></summary>
<br>
Real-time monitoring platform for solar and wind assets across Austria, from raw sensor telemetry to operational and executive dashboards.


Bridged MQTT (HiveMQ Cloud) into Kafka-compatible Redpanda Serverless, with consumer groups, manual offset commits, and at-least-once delivery
Built a Bytewax dataflow for windowed KPI aggregation (60-minute tumbling + session windows) with on-disk SQLite state recovery
Modeled a 4-fact dbt star schema on Snowflake (hourly telemetry → daily → monthly rollups) with documented ADRs for every major tooling decision
Two visualization layers on different cadences: Grafana for sub-minute ops monitoring, Evidence.dev (code-as-dashboard) for business reporting
Stack: Python, Redpanda/Kafka, Bytewax, Snowflake, dbt, Terraform, Grafana, Evidence.dev  |  🔗 green-energy-iot-de-project


</details>
<details>
<summary><strong>🏥 Healthcare — MedFlow Decision Engine</strong></summary>
<br>
A batch pipeline + decision engine that ingests CSV exports from EHR systems and returns a per-patient readmission-risk decision.


Built a Bronze → Silver → Gold medallion architecture on S3 + Apache Iceberg (PySpark, Glue, Athena), runs natively on an 8GB M1 MacBook with zero cluster cost
Combined three decision layers: deterministic clinical rules, an XGBoost readmission model, and an LLM-generated plain-English explanation — the rules engine is the authoritative escalation path, the ML score is a soft signal
Served via a BentoML API with per-item batch error handling, so one malformed patient record doesn't fail an entire cohort scoring run
Streamlit dashboard giving care coordinators a cohort view plus per-patient drill-down
Stack: PySpark, Apache Iceberg, Prefect, XGBoost, BentoML, Streamlit, Terraform  |  🔗 healthcare-csv-decision-engine


</details>
<details>
<summary><strong>💰 Finance — SupplyLens (EDGAR Filings Intelligence)</strong></summary>
<br>
Pulls real SEC EDGAR filings for 16 large-cap issuers and serves a research API with vector search over the filing text.


Ingested 5 filing types (10-K, 10-Q, 8-K, DEF 14A, Form 4) through a pure-Iceberg medallion architecture — S3 + Glue + Athena as a serverless warehouse, no managed platform
Embedded 24,556 real filing chunks into Qdrant for vector search, using a local zero-cost GGUF embedding model (no per-token API charges, no data egress)
Built 6 dbt marts on Athena, bucketed by company ID to match the API's query patterns
Three-layer data quality: Soda row-level checks, dbt column tests, and dbt source-freshness monitoring
Stack: PyIceberg, dbt-athena, Qdrant, Prefect, FastAPI, Terraform, Docker  |  🔗 edgar-supply-graph


</details>

---

### 🧰 Side Projects
Smaller tools and scripts I've built outside the domain projects above — utilities, automations, one-offs.

- **[Tool Name]** — [one-line description of what it does] &nbsp;|&nbsp; 🔗 [Repo]

---

### 🛠️ Tech Stack

**Languages:** ![Python](https://img.shields.io/badge/-Python-3776AB?logo=python&logoColor=white) ![SQL](https://img.shields.io/badge/-SQL-4479A1?logo=postgresql&logoColor=white)

**Cloud & Storage:** ![AWS](https://img.shields.io/badge/-AWS-232F3E?logo=amazon-aws&logoColor=white) ![GCP](https://img.shields.io/badge/-GCP-4285F4?logo=google-cloud&logoColor=white) ![S3](https://img.shields.io/badge/-S3-569A31?logo=amazons3&logoColor=white)

**Orchestration & Processing:** ![Airflow](https://img.shields.io/badge/-Airflow-017CEE?logo=apacheairflow&logoColor=white) ![Spark](https://img.shields.io/badge/-Spark-E25A1C?logo=apachespark&logoColor=white) ![Kafka](https://img.shields.io/badge/-Kafka-231F20?logo=apachekafka&logoColor=white)

**Infra:** ![Docker](https://img.shields.io/badge/-Docker-2496ED?logo=docker&logoColor=white) [![Terraform](https://shields.io)](https://terraform.io)


---

### 📫 Elsewhere
Links are in the sidebar — here's what you'll find at each:

- **[Portfolio site]** — projects, background, and articles
- **[Substack]** — technical writing on data engineering (cross-posted from my site)
- **[LinkedIn]** — professional background and network
- 📓 Older notes & DE learnings (not actively maintained): [link]

<!--
=====================================================================
WORDING CHEAT SHEET
=====================================================================
Instead of...                     Use...
"I have no experience but..."  →  (say nothing — let projects speak)
"Learned how to use X"         →  "Built/deployed X"
"Small project about..."       →  "Pipeline for..." / "System that..."
"Tried to implement..."        →  "Implemented..."
"I'm passionate about data"    →  (cut — show it via project count/range)
=====================================================================
-->
