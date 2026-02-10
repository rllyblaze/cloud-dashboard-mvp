# Global Cloud Health Dashboard (Serverless MVP)

This project is a serverless cloud observability MVP that aggregates large-scale cloud
telemetry data into a single, easy-to-understand health snapshot.  
It demonstrates how backend analytics and a static frontend can be connected using
AWS-native, low-cost services.

The dashboard is designed to provide **operational visibility**, not real-time alerting,
and focuses on clarity, simplicity, and correct architectural separation.

---

## Problem Statement

Modern cloud environments generate millions of infrastructure metrics, but raw data
alone does not provide actionable insight. Engineering teams need a summarized view
that quickly answers one question:

**“Is the system healthy right now?”**

This project solves that problem by computing KPIs and anomaly signals over a defined
time window and exposing the result as a stable, frontend-ready artifact.

---

## Architecture Overview

Raw metrics are stored in Amazon S3 and queried directly using Amazon Athena.
Athena computes health KPIs and anomaly logic and unloads a summarized result as JSON
back into S3. A static frontend hosted on S3 fetches this JSON and renders a global
health dashboard in the browser.

The entire system is fully serverless and requires no always-on compute.

![Architecture Diagram](screenshots/Architecture.png)

---

## Technology Stack

- **Amazon S3** – Raw data storage, processed outputs, and static website hosting  
- **Amazon Athena** – SQL analytics and KPI computation  
- **HTML / CSS / JavaScript** – Frontend dashboard  
- **AWS IAM & S3 Policies** – Secure, scoped public access  
- **CORS** – Cross-bucket browser access for frontend data fetch  

---

## Data & Analytics

- Dataset size: ~2 million rows of cloud telemetry  
- Metrics include CPU usage, memory usage, network traffic, and task metadata  
- Invalid timestamps are filtered using safe casting  
- KPIs are computed over the **latest available 1-hour window**  

Health status is derived using warning and critical thresholds, producing:
- Overall health score
- Service health percentage
- Anomaly count
- System status (OK / WARNING / CRITICAL)

---

## Dataset Source

The dataset used in this project was sourced from Kaggle and represents simulated
cloud computing performance metrics, including CPU, memory, and network usage.

Original dataset:  
https://www.kaggle.com/datasets/abdurraziq01/cloud-computing-performance-metrics

The data was used solely for learning and demonstration purposes to design and validate
the analytics pipeline and dashboard architecture.

---

## Frontend Dashboard

The frontend is a static website hosted on Amazon S3.
It fetches a single JSON file (`latest.json`) and renders:

- Overall system status
- Health score
- Average and maximum CPU usage
- Average memory usage
- Average network traffic
- Time window context for transparency

The dashboard intentionally avoids presenting the data as real-time and clearly
displays the historical time window used for analysis.

---

## Screenshots

### Global Cloud Health Dashboard
![Dashboard UI](screenshots/dashboard.png)

### Athena Analytics Query
![Athena SQL Query](screenshots/athena-query.png)

### S3 Architecture – Data & Analytics
![S3 Project Architecture](screenshots/s3-project-architecture.png)

### S3 Architecture – Frontend Hosting
![S3 Frontend Architecture](screenshots/s3-frontend-architecture.png)

---

## Key Learnings

- Designing serverless analytics pipelines using Amazon Athena and S3  
- Implementing secure public access using scoped S3 bucket policies  
- Debugging real-world CORS and browser security constraints  
- Separating data, analytics, and presentation layers cleanly  
- Communicating system health without over-engineering  

---

## Future Improvements

- Automate promotion of analytics outputs to `latest.json`
- Add additional time windows (7-day, 30-day, 1-year)
- Introduce CloudFront for frontend caching
- Visualize trends using charts instead of single snapshots


