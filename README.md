# 🚀 YT_ETL – YouTube ELT Pipeline with Apache Airflow

An end-to-end **ELT data pipeline** that extracts YouTube video data using the YouTube Data API, processes it through Apache Airflow, loads it into a PostgreSQL data warehouse (staging + core layers), and performs automated data quality validation using Soda.

---

## 📌 Project Overview

This project demonstrates a production-style data engineering workflow:

1. Extract video & playlist metadata from YouTube API
2. Store raw data as JSON
3. Load data into a PostgreSQL data warehouse
4. Apply layered modeling (staging → core)
5. Run automated data quality checks
6. Fully orchestrated using Apache Airflow

The pipeline runs automatically on a scheduled basis.

---

## 🏗️ Architecture

```
YouTube API
     ↓
Airflow DAG 1 (produce_json)
     ↓
Raw JSON File (data/)
     ↓
Airflow DAG 2 (update_db)
     ↓
PostgreSQL (staging schema)
     ↓
PostgreSQL (core schema)
     ↓
Airflow DAG 3 (data_quality)
     ↓
Soda Data Quality Validation
```

---

## ⚙️ Workflow Details

### 🔹 DAG 1 – `produce_json`

* Scheduled daily
* Fetches playlist ID
* Extracts video IDs
* Retrieves video statistics
* Saves raw JSON to local `data/` folder
* Triggers `update_db` DAG

### 🔹 DAG 2 – `update_db`

* Loads JSON into staging schema
* Transforms and loads into core schema
* Triggers `data_quality` DAG

### 🔹 DAG 3 – `data_quality`

* Runs Soda checks on:

  * staging schema
  * core schema
* Validates schema consistency and data integrity

---

## 🗄️ Data Warehouse Design

The project follows a **layered architecture**:

* **Staging Schema**

  * Raw structured data from JSON
  * Minimal transformation

* **Core Schema**

  * Cleaned and analytics-ready tables
  * Structured for reporting & BI use

---

## 🛠️ Tech Stack

* Python
* Apache Airflow
* PostgreSQL
* Soda (Data Quality Framework)
* Docker
* YouTube Data API

---

## ⏰ Scheduling

* Timezone-aware scheduling
* DAG run timeout handling
* Controlled concurrency
* Cross-DAG triggering using `TriggerDagRunOperator`

---

## 📂 Project Structure

```
YT_ETL/
│
├── dags/
│   └── main_dag.py
│
├── api/
│   └── video_stats.py
│
├── datawarehouse/
│   └── dwh.py
│
├── dataquality/
│   └── soda.py
│
├── data/
│   └── YT_data_<date>.json
│
├── docker-compose.yml
└── README.md
```

---

## ▶️ How to Run

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/Amritpal985/YT_ETL.git
cd YT_ETL
```

### 2️⃣ Start Airflow (Docker)

```bash
docker-compose up --build
```

### 3️⃣ Open Airflow UI

```
http://localhost:8080
```

Unpause DAGs and wait for scheduled execution.

---

## 📊 Key Features

* Modular DAG design
* Event-driven DAG triggering
* Staging → Core data modeling
* Automated data quality validation
* Production-style scheduling
* Containerized deployment




