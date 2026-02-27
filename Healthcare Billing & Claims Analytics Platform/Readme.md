# 🏥 End-to-End Healthcare Revenue Cycle Management (RCM) Data Engineering Pipeline on Azure

## 📌 Project Overview
Healthcare Revenue Cycle Management (RCM) is the financial lifecycle that healthcare providers use to track patient encounters, insurance claims, and payments from scheduling to final reimbursement.

This project builds an enterprise-grade, scalable data engineering platform on Azure to monitor revenue leakage, reduce claim denials, and track high-risk financial accounts using modern Medallion Architecture (Bronze → Silver → Gold).

The solution integrates multi-hospital EMR data, insurance claims, and external healthcare APIs to generate actionable financial KPIs for healthcare providers.

---

## 🎯 Business Problem
Healthcare providers face major financial risks due to:
- Delayed insurance reimbursements
- Claim denials or partial approvals
- Uncollected patient out-of-pocket balances
- Aging Accounts Receivable (>90 days)

Without centralized data pipelines, hospitals struggle to monitor revenue leakage and optimize collections.

This project solves the problem by creating a unified, governed data platform for RCM analytics.

---

## 🏗️ Solution Architecture
![Diagram](https://github.com/nikhithababy-DE/Data-Engineering-/blob/main/Healthcare%20Billing%20%26%20Claims%20Analytics%20Platform/Diagram/Acchitecture.png)



### High-Level Flow:
1. EMR data ingested from Azure SQL DB (multiple hospitals)
2. Claims & CPT files loaded from ADLS Gen2 landing zone
3. NPI & ICD data extracted via APIs
4. Bronze Layer → Raw Parquet ingestion
5. Silver Layer → Cleaned, standardized, SCD Type 2
6. Gold Layer → Business KPIs & financial analytics tables

---

## 🛠️ Tech Stack

### Cloud & Storage
- Azure Data Lake Storage Gen2 (Bronze, Silver, Gold)
- Azure SQL Database (Multi-hospital EMR sources)

### Data Engineering
- Azure Data Factory (Parameterized & dynamic pipelines)
- Azure Databricks (PySpark, Delta Lake, Structured Processing)
- Delta Lake (ACID transactions, incremental loads)

### Governance & Security
- Unity Catalog (Data lineage, RBAC, centralized governance)
- Azure Key Vault (Secret management)

### APIs & External Data
- NPI API for provider enrichment
- ICD Codes API for diagnosis classification

---

## 📂 Data Sources

### 1. EMR Data (Azure SQL DB)
Tables:
- Patients
- Providers
- Departments
- Encounters
- Transactions

Relationships:
One patient → Multiple encounters → Multiple transactions per encounter

---
### 2. Claims Data (ADLS Gen2 CSV)
Contains:
- Claim ID
- Transaction ID
- Paid Amount
- Claim Status
- Service Dates

Monthly incremental ingestion into Bronze layer.

---

### 3. External APIs
- NPI API → Enrich provider details with National Provider Identifier
- ICD Codes API → Map diagnosis codes for disease analytics

---

## ⚙️ Key Engineering Highlights
- Multi-hospital schema integration using surrogate keys
- Parameterized ADF pipelines for scalable ingestion
- Incremental + Full load framework using watermark columns
- Audit logging framework using Delta tables
- Parallel processing using ForEach batch count
- Unity Catalog for governance, lineage, and RBAC
- Secure secret management via Azure Key Vault
- Medallion Architecture (Bronze → Silver → Gold)

---

## 🔄 End-to-End Data Pipeline Flow

### Step 1: EMR to Landing
- Ingest patient, provider, encounter, and transaction data
- Use dynamic ADF pipelines with configuration-driven ingestion

### Step 2: Landing to Bronze
- Convert raw CSV and SQL data into Parquet format
- Maintain raw historical data for replay and lineage

### Step 3: Bronze to Silver
Transformations:
- Data cleaning (remove nulls & duplicates)
- Schema standardization (Common Data Model)
- SCD Type 2 implementation for historical tracking
- Delta tables for incremental processing

### Step 4: Silver to Gold
Business-ready curated tables created:
- Financial KPIs
- AR aging analysis
- Claim approval & denial trends
- Department-wise revenue performance

---

## 📊 Business KPIs Generated
- Accounts Receivable Aging > 90 days
- Insurance vs Patient Payment Ratio
- Claim Approval vs Denial Rate
- Revenue Leakage Tracking
- Department-wise Financial Performance
- High-risk outstanding patient balances

---

## 📈 Performance & Optimization Achievements
- Reduced pipeline runtime by ~40% using parallel ADF processing
- Optimized Spark transformations by replacing UDFs with built-in functions
- Enabled scalable ingestion across multiple hospital databases
- Implemented incremental loads to minimize compute cost

---

## 🧠 Design Decisions
- Adopted Medallion Architecture for data quality layering
- Used Delta Lake for ACID compliance and incremental processing
- Implemented Unity Catalog for centralized governance & lineage
- Parameterized pipelines for multi-hospital scalability
- Stored secrets in Azure Key Vault for secure access

Detailed design notes available in: `docs/design-decisions.md`

---

## 📁 Repository Structure
