# Distributed Machine Learning with Apache Spark (MLlib)

This project demonstrates how to build and run a **distributed machine learning pipeline using Apache Spark MLlib**, deployed on a **multi-node Spark cluster** with Docker.  

The focus is on understanding **distributed data processing, ML pipelines, and cluster-level execution**, including performance monitoring and comparisons between per-node and unified dataset training.

---

## Project Overview

In this project, I:

- Built a Spark cluster (1 Master + 2 Workers) using **Docker Compose**.
- Used **PySpark and Spark MLlib** to train classification models.
- Worked with a real-world dataset on **AI’s impact on jobs by 2030**.
- Trained:
  - A **global model** using the full dataset.
  - **Per-node models** using data partitions assigned to individual nodes.
- Monitored execution using the **Spark Web UI**.
- Compared **training time, F1-score, and resource utilization** for both distributed and per-node training.

---

## Dataset

**AI Impact on Jobs 2030** (Kaggle)  

The dataset contains structured information about job roles and their exposure to AI and automation.

### Key Features
- `Job_Title` (categorical)
- `Education_Level` (categorical)
- `Average_Salary`
- `Years_Experience`
- `AI_Exposure_Index`
- `Automation_Probability_2030`
- `Tech_Growth_Factor`
- `Skill_1 … Skill_10`

### Target Variable
- `Risk_Category` — categorical label indicating job risk level due to AI

---

## Architecture

- **Spark Master**
- **Two Spark Workers**
- **JupyterLab (PySpark)**
- Shared volume mounted at `/data` across all containers

The dataset was manually split into three files to simulate **distributed data ownership**:
- `jobs_master.csv`
- `jobs_worker1.csv`
- `jobs_worker2.csv`

Each worker loads only its portion of the dataset, while the master coordinates execution. This setup mimics **real distributed systems** where data is not centralized.

---

## Step-by-Step Execution

### Step 1: Installation and Start Spark Session
- Built Docker containers for Spark Master and Workers.
- Started Spark session in JupyterLab.
- **Observation:** Spark session successfully created; cluster ready for distributed execution.

### Step 2: Manual Data Splitting Across Nodes
- Dataset split into 3 parts to simulate nodes.
- **Observation:** Each node has roughly 1/3 of the data; enables parallel per-node training.

### Step 3: Load Dataset on Each Node
- Loaded CSV files into Spark DataFrames (`master_df`, `worker1_df`, `worker2_df`).
- **Observation:** Each node sees only its own data; demonstrates distributed data loading.

### Step 4: Data Partitioning and Spark SQL Processing
- Created a unified DataFrame (`df_all`) combining all nodes' data.
- Repartitioned by `AI_Exposure_Index` for parallelism.
- Performed SQL-based exploration (counts, cleaning, transformations).
- **Observation:** Data properly partitioned; SQL queries confirm dataset integrity.

### Step 5: Machine Learning Pipeline
- Spark MLlib pipeline:
  - StringIndexer for categorical features and labels
  - OneHotEncoder for categorical columns
  - VectorAssembler to assemble numeric and encoded features
  - Logistic Regression classifier
- Trained **global model** on unified dataset:
  - **F1 Score:** 0.995
  - **Training Time:** 16.78 seconds
- **Observation:** Model achieves high predictive performance; unified dataset provides stable results.

---

## Part 2 — Distributed vs. Per-Node Machine Learning

### Step 1: Simultaneous ML Jobs on Each Node
- Trained separate models on each node’s dataset.
- **Observations:**

| Node        | Dataset Count | F1 Score | Training Time (s) |
|------------|---------------|----------|-----------------|
| Master     | 1006          | 0.9572   | 9.43            |
| Worker 1   | 965           | 0.9898   | 8.77            |
| Worker 2   | 1029          | 0.9766   | 6.48            |

- Smaller datasets trained faster but with more variance in F1 scores.

### Step 2: Distributed Machine Learning on Unified Dataset
- Trained single model on the full dataset distributed across all nodes.
- **Observations:**
  - **F1 Score:** 0.9950
  - **Training Time:** 7.61 seconds
  - Distributed model achieves high accuracy and efficient parallel training.

### Step 3: Performance Monitoring & Comparison
- Compared F1 scores, training times, and dataset counts.
- **Observations:**
  - Unified dataset provides **stable accuracy**.
  - Per-node training is faster on small datasets, but with slight variance.
  - Training time for distributed model is competitive due to parallel execution across nodes.

| Node / Model           | Dataset Count | F1 Score | Training Time (s) |
|------------------------|---------------|----------|-----------------|
| Master Node            | 1006          | 0.9911   | 9.43            |
| Worker 1               | 965           | 0.9979   | 8.77            |
| Worker 2               | 1029          | 0.9951   | 6.48            |
| Distributed Model      | 3000          | 0.9950   | 7.61            |

---

## Key Observations

- Spark efficiently distributes data and computation across multiple nodes.
- Per-node training is faster on small datasets but shows variance in performance.
- Distributed training balances speed and stability.
- Training times confirm the advantage of **parallelism in distributed ML**.
- Spark Web UI is essential for monitoring memory, CPU, and task distribution.
- Docker provides a reproducible environment for multi-node clusters.

---

## Suggested GitHub Content

Include:

- Jupyter Notebook(s) (`.ipynb`) with full code and outputs
- Docker Compose file and Dockerfiles
- Dataset files (or link to Kaggle dataset)
- Screenshots:
  - Spark Web UI (Executors page)
  - Docker containers running (`docker ps`)
- README.md (this file)
- Optional: cluster architecture diagram

---

## Technologies Used

- Apache Spark 3.x  
- Spark MLlib  
- PySpark  
- Docker & Docker Compose  
- JupyterLab  
- Python 3  

---

## Key Takeaways

- Spark MLlib allows **scalable ML pipelines** with minimal code changes.  
- Distributed training improves generalization and training efficiency.  
- Resource monitoring and tuning are critical for performance.  
- Docker simplifies **reproducible big data environments**.  

---
