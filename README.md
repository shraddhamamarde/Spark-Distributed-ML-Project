# Spark-Distributed-ML-Project
Spark Distributed ML Project

# Distributed Machine Learning using Apache Spark and Docker

## Project Overview
This project demonstrates distributed data processing and machine learning using Apache Spark deployed on a Docker-based cluster. The system consists of one Spark master and two Spark worker nodes.

The project includes:
- Manual dataset splitting across nodes
- Distributed data loading and partitioning
- Spark SQL-based data processing
- Machine learning using Spark MLlib
- Performance comparison between node-level and distributed models

---

## Technologies Used
- Apache Spark
- PySpark
- Docker & Docker Compose
- Python
- Jupyter Notebook

---

## Cluster Architecture
- 1 Spark Master
- 2 Spark Worker Nodes
- Docker-based deployment

---

## How to Run the Project

### Step 1: Start Spark Cluster
```bash
docker compose up -d

