| Login with username: admin  password: wvHtEfcZEbV6WPnm
### Deftinition
- Apache Airflow is an open-source workflow orchestration and scheduling tool, originally created by Airbnb, and now a top-level Apache project.
- At a high level:
     - A workflow = a series of tasks executed in a defined order
     - Airflow is most commonly used for data pipelines (ETL / ELT), but works for any workflow
     - You define workflows as code (Python), making them versionable, testable, and reproducible
 

### Core Concepts (mapped to your bullets)
1️⃣ Workflow & Data Pipelines

- Data comes from multiple sources → transformations → stored somewhere
- This end-to-end job is a workflow, often an ETL pipeline:
    - Extract: read from DBs, APIs, files, streams
    - Transform: clean, enrich, aggregate
    - Load: data warehouse, lake, analytics DB
- Airflow orchestrates these steps — it does not do heavy data processing itself.


2️⃣ DAG (Directed Acyclic Graph)

At the heart of Airflow is a DAG.

Directed → tasks have direction (A → B)

Acyclic → no loops

Graph → visual dependency representation
