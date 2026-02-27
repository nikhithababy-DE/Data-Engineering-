
# 🧠 Design Decisions & Trade-offs

This document explains the key design decisions made while building the **End-to-End Healthcare Revenue Cycle Management (RCM) Data Engineering Platform on Azure**, along with the trade-offs considered. It demonstrates scalable, secure, and governance-ready architecture suitable for enterprise healthcare analytics.

---

## 1️⃣ Architecture Choice: Medallion Architecture (Bronze → Silver → Gold)

**Decision:**  
- **Bronze:** Raw ingestion from EMR, Claims, and external APIs  
- **Silver:** Cleaned, standardized, and historical-tracked data (SCD Type 2)  
- **Gold:** Curated business tables for KPIs, analytics, and dashboards  

**Trade-offs:**  
- **Pros:**  
  - Clear separation between raw and refined data  
  - Supports incremental processing & replay of historical data  
  - Easier to implement governance and lineage  
- **Cons:**  
  - Higher storage consumption due to multiple layers  
  - Pipeline orchestration becomes more complex  

---

## 2️⃣ Pipeline Framework: Parameterized Azure Data Factory (ADF) Pipelines

**Decision:** Dynamic, parameterized pipelines using Lookup, ForEach, and conditional activities.  

**Trade-offs:**  
- **Pros:**  
  - Scalable across multiple hospitals and schemas  
  - Supports incremental & full loads using a single pipeline design  
- **Cons:**  
  - Increases pipeline complexity  
  - Requires robust parameter and error management  

---

## 3️⃣ Incremental vs Full Load

**Decision:**  
- Full load for initial ingestion  
- Incremental load using watermark columns for daily/weekly updates  

**Trade-offs:**  
- **Pros:**  
  - Reduces runtime and compute costs  
  - Handles terabytes of EMR and claims data efficiently  
- **Cons:**  
  - Requires accurate audit/log framework to track last fetched date  
  - More complex logic for schema changes or missing data  

---

## 4️⃣ Data Storage: Delta Lake

**Decision:**  
- Store cleaned & transformed data in Delta tables for ACID compliance and SCD Type 2  

**Trade-offs:**  
- **Pros:**  
  - Versioning and rollback support  
  - Reliable incremental processing  
  - Optimized Spark performance  
- **Cons:**  
  - Compaction and optimization for large tables can increase compute costs  

---

## 5️⃣ Governance & Security: Unity Catalog + Key Vault

**Decision:**  
- Unity Catalog for centralized metadata, lineage, and RBAC  
- Azure Key Vault for managing storage and database secrets  

**Trade-offs:**  
- **Pros:**  
  - Enterprise-grade governance  
  - Role-based access control ensures compliance  
  - Secure management of sensitive healthcare data  
- **Cons:**  
  - Initial setup requires careful configuration  
  - Requires ongoing maintenance of access policies  

---

## 6️⃣ Parallel vs Sequential Processing

**Decision:**  
- Removed sequential execution in ForEach loops and enabled batch processing for parallel execution  

**Trade-offs:**  
- **Pros:**  
  - ~40% faster pipeline execution  
  - Better utilization of compute resources  
- **Cons:**  
  - Parallelism requires careful monitoring to avoid throttling or failures  
  - Error handling is more complex than sequential pipelines  

---

## 7️⃣ Use of External APIs (NPI & ICD)

**Decision:**  
- Fetch up-to-date provider and diagnosis info from NPI and ICD APIs before transformation  

**Trade-offs:**  
- **Pros:**  
  - Ensures data enrichment with current provider and diagnostic information  
  - Enables disease-specific KPIs and analytics  
- **Cons:**  
  - API failures or throttling can delay pipeline execution  
  - Requires retry logic and monitoring  

---

## 8️⃣ Other Key Design Choices

| Choice | Pros | Cons |
|--------|------|------|
| Parameterized SQL datasets | Reusable across multiple hospitals | More setup & maintenance |
| Audit logging in Delta tables | Tracks incremental loads and errors | Slight overhead in processing |
| SCD Type 2 for historical changes | Enables time-based analytics | Extra storage required |

---

## ✅ Summary

This design ensures:

- **Scalability:** Works for multiple hospital EMRs and large claims datasets  
- **Performance:** Parallel processing, incremental loads reduce runtime and compute cost  
- **Governance & Security:** Unity Catalog + Key Vault for secure and compliant data access  
- **Business Impact:** Enables financial KPIs, AR aging analysis, and claim denial tracking  


