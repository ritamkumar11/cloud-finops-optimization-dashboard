# ☁️ cloud-finops-optimization-dashboard

> A Power BI dashboard for cloud cost waste analysis and optimization — identifying idle instances, recommending shutdowns/rightsizing, and projecting **~35% cost savings** using FinOps principles.

---

## 📊 Dashboard Preview

| Metric | Value |
|---|---|
| 💰 Total Cloud Cost | $517.54K |
| 🗑️ Total Waste Cost | $179.36K (34.66%) |
| 💡 Estimated Savings | $184.62K |
| ✅ Optimized Cost | $332.92K |
| 🖥️ Total Instances | 100K |
| 😴 Idle Instances | 20K (20%) |

---

## 🧠 Problem Statement

Raw cloud logs had no classification of idle, wasted, or optimized resources. The goal was to transform this data into actionable FinOps insights to reduce unnecessary cloud spend.

---

## 🔄 Methodology

### 1. Data Cleaning & Transformation
- Removed duplicates and null values
- Standardized region names (`ap-south-1`, `eu-west-1`, `us-east-1`) and instance types (`t3.micro` → `r5.2xlarge`)
- Converted date formats for time-series analysis

### 2. Calculated Columns (Power BI)
| Column | Logic |
|---|---|
| `Idle Flag` | `IF CPU < 10% AND Uptime > 72 hrs → "Idle"` |
| `Daily Waste Cost` | `IF Idle → Daily Cost, ELSE 0` |
| `Recommendation` | `Idle → Shutdown`, `Low CPU → Downgrade`, `Else → Keep` |

### 3. DAX Measures
```DAX
Total Cost = SUM(Daily Cost)
Total Waste Cost = SUM(Daily Waste Cost)
Waste % = Waste / Total Cost
Total Instances = COUNT(Instance ID)
Estimated Savings = Based on recommendation logic
Optimized Cost = Total Cost – Est. Savings
```

---

## 📋 Dashboard Pages

### Page 1 — Cost Overview
- Total cost, waste, and utilization KPIs
- Project-wise cost distribution (bar chart)
- Daily cost trend (line chart)

### Page 2 — Waste Analysis
- Idle instance trend across October
- Waste breakdown by **region** and **project type**
- Idle Resource Flagger table (instance-level detail)

### Page 3 — Optimization & Savings
- Recommendation distribution (Shutdown / Downgrade / Optimal)
- Estimated savings by recommendation type
- Optimized cost projection

---

## 🔍 Key Insights

- **Data-Science-Sandbox** is the core inefficiency driver — almost all $179K waste originates here
- Idle instances remain **~650–750 daily**, indicating poor resource scheduling and no auto-shutdown policy
- Waste is **globally distributed** across all three regions (~$59–60K each), not region-specific
- Many instances show **CPU < 10% with uptime > 72 hours** — classic zombie instances consuming cost without value
- **Shutdown alone** contributes ~$179K in savings — far exceeding downgrade savings (~$5.26K)
- **57–58% of instances are already Optimal**, showing good resource allocation for the majority

---

## ✅ Recommendations

| Action | Instances | Est. Saving |
|---|---|---|
| 🔴 Shutdown (Idle) | ~20,210 | ~$179K |
| 🟠 Downgrade: `r5.2xlarge` → `c5.xlarge` | 532 | ~$4K |
| 🟡 Downgrade: `c5.xlarge` → `m5.large` | 557 | ~$1K |
| 🟢 Keep / Monitor | ~78,700 | — |

---

## 🛠️ Tech Stack

- **Power BI Desktop** — Dashboard creation and DAX measures
- **Microsoft Excel / CSV** — Raw data source

---

## 📌 Dataset Details

| Field | Description |
|---|---|
| `Instance_ID` | Unique cloud instance identifier |
| `Project_Name` | Associated project (e.g., Data-Science-Sandbox) |
| `Region` | Cloud region (us-east-1, eu-west-1, ap-south-1) |
| `Instance_Type` | EC2-style instance type (t3.micro, m5.large, etc.) |
| `CPU_%` | Average CPU utilization |
| `Memory_%` | Average memory utilization |
| `Uptime_Hours` | Total uptime during the period |
| `Daily_Cost` | Daily cost in USD |
| `Idle_Flag` | Idle / Active classification |

- **Data Range:** October 1–30, 2023
- **Total Instances:** 100,000
- **Granularity:** Daily

---

## 📬 Connect with Me

Feel free to reach out or explore more of my work!

- 🔗 [LinkedIn](https://www.linkedin.com/in/ritam-kumar-111005rkb/)
- 📧 ritamkumar111005@gmail.com

---

Designed and developed as part of my Data Analytics portfolio — transforming raw cloud data into strategic cost decisions.
