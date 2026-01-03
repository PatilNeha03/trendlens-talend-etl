# TrendLens – Talend ETL Pipeline for Short-Video Analytics

## 📌 Project Overview
TrendLens is an end-to-end **data warehousing and analytics project** built to analyze engagement patterns and virality drivers across short-form video platforms such as **TikTok** and **YouTube Shorts**.  

This repository focuses on the **Talend ETL implementation**, which extracts raw platform data, performs complex transformations, and loads it into a **PostgreSQL-based Galaxy Schema data warehouse** optimized for OLAP analysis.

The pipeline processes ~70,000 records and enables multidimensional analysis across time, geography, content categories, creator tiers, and platform types.

---

## 🛠️ Technology Stack
- **ETL Tool:** Talend Open Studio
- **Database:** PostgreSQL
- **Data Modeling:** Galaxy Schema
- **Analytics:** ROLAP / OLAP
- **Dataset:** TikTok & YouTube Shorts Trends (2025)
- **Volume:** ~70K processed records

---

## 🧩 Data Warehouse Architecture

### Fact Tables
- **FactVideoMetrics**
  - Views, Likes, Shares, Saves
  - Engagement Rate
  - Foreign keys to all dimensions

- **FactCreatorPerformanceSnapshot**
  - Aggregated creator-level performance metrics

### Dimension Tables (15+)
- DimCreator (Type 3 Slowly Changing Dimension)
- DimVideo
- DimPlatform
- DimCategory
- DimTrend
- DimCountry
- DimRegion
- DimTime
- DimDevice
- DimTrafficSource

### Bridge Tables
- Bridge_Video_Hashtag
- Bridge_Video_Language
- Bridge_Video_TrafficSource

This design supports complex many-to-many relationships and high-performance analytical queries.

---

## 🔄 ETL Pipeline Design

### 1️⃣ Extraction
- Raw data extracted from multiple operational datasets
- Includes video metadata, creator attributes, platform metrics, and engagement statistics
- Data first loaded into **staging tables** to enable validation and preprocessing

---

### 2️⃣ Transformation
Implemented using **Talend components such as tMap and tAggregateRow**:

- Data cleansing and normalization
- Handling missing values and inconsistent formats
- Data type conversions
- Standardization across platforms
- Derived metric computation:


- Creator-level aggregation for performance snapshots
- Lookup and upsert logic for dimension tables to maintain current records

---

### 3️⃣ Loading
- Dimension tables loaded first to generate surrogate keys
- Bridge tables populated to maintain many-to-many relationships
- Fact tables loaded last with validated foreign key references
- Referential integrity enforced using PostgreSQL constraints

---

## ⏱️ Control Flow & Orchestration
A centralized **Talend Control Flow Job** manages the ETL execution:

- Sequential execution of dependent jobs
- Parallel execution of independent dimension jobs using `tParallelize`
- Conditional execution using `OnSubjobOK`
- Post-load validation using SQL queries

This ensures automation, reliability, and repeatability of the pipeline.

---

## 🕰️ Slowly Changing Dimensions (SCD)
The **Creator Dimension** implements **Type 3 SCD** to track creator tier evolution.

### Columns:
- Previous_Creator_Tier
- Current_Creator_Tier
- Tier_Change_Date

This approach preserves historical context while avoiding excessive row duplication.

---

## 📊 OLAP Capabilities
The warehouse supports advanced OLAP operations, including:

- **Roll-up:** Views and Likes by Region
- **Drill-down:** Creator performance by category
- **Slice:** Platform-specific engagement metrics
- **Dice:** Seasonal and regional trend comparisons

These capabilities allow seamless navigation between high-level insights and granular data.

---

## 📈 Key Insights Enabled
- Identification of peak engagement days (Fridays show highest activity)
- Detection of saturated vs under-explored content categories
- Trend lifecycle performance comparison
- Region-specific audience behavior analysis

---

## 📂 Repository Structure

---

## 🚀 How to Run the Project
1. Import the project into **Talend Open Studio**
2. Configure PostgreSQL connection metadata
3. Update context variables (DB name, user, password)
4. Execute the **Control Flow Job**
5. Validate results using PostgreSQL queries

---

## 🎓 Academic Context
Developed as part of  
**IE 6760 – Data Warehousing & Integration**

---

## 👩‍💻 Author
**Neha Patil**  
Graduate Student – Data Engineering & Analytics  

---

## 📌 Future Enhancements
- Incremental and CDC-based loading
- Real-time ingestion using Kafka
- Predictive virality modeling
- BI dashboard integration (Power BI / Tableau)
