# Healthcare Billing and Claims Analytics Platform

Healthcare Revenue Cycle Management (RCM) is the process hospitals and healthcare providers use to manage the financial lifecycle of a patient encounter — from the time an appointment is scheduled until the provider receives full payment for the services rendered.

RCM begins when **patient and insurance details are collected** prior to or during a visit. This step determines the financial responsibility split between the insurance provider and the patient. For example, a total medical charge of $20,000 may be partially covered by insurance ($15,000) while the remaining balance ($5,000) is the patient’s responsibility.

Once **services are provided, the hospital generates medical bills and submits insurance claims**.
These **claims are reviewed by insurance companies, which may approve the claim fully, partially, or deny it altogether**.
Based on the insurer’s response, payments are received and **any remaining balances are followed up with the patient**.

**RCM also involves continuous monitoring and improvement** to ensure timely payments, reduce claim denials, and maintain the financial health of the healthcare provider while continuing to deliver quality patient care.

### Core Financial Components
- **Accounts Receivable (AR):** Tracks payments owed to the healthcare provider by patients and insurance after services are delivered.  
- **Accounts Payable (AP):** Tracks payments the healthcare provider owes to vendors, suppliers, or service providers for operational and clinical expenses.

### Risk Factors
In Healthcare Revenue Cycle Management, key risk factors impact the hospital’s financial health:

- **Patient Out-of-Pocket Payments:** Payments that must be collected directly from patients carry a higher risk, as uncollected amounts may not be recovered.  
- **Aged Accounts (>90 days):** Accounts receivable that have been outstanding for more than 90 days are considered high-risk for non-payment or default. Monitoring the **month-on-month movement** of payments into this aged category is critical to identify potential revenue leakage and prioritize follow-ups.  

Effective tracking and management of these risk factors help ensure the hospital maintains healthy cash flow and minimizes financial losses.

### Data Sources

1. **EMR Data (Electronic Medical Records)**
   - Stored in **Azure SQL DB across multiple hospitals** (different database instances). 
   - Core tables:
     - `Patients`: Personal information, demographics, unique patient ID, contact details.
     - `Providers`: Doctor details including specialization, department, NPI (National Provider Identifier).
     - `Departments`: Hospital departments like Cardiology, Emergency, Dermatology, etc.
     - `Encounters`: Patient visits, encounter type (inpatient, outpatient, emergency, routine), associated provider and department.
     - `Transactions`: Payments, billing, service codes, insurance or patient-paid amounts, claim IDs, ICD codes.
   - Notes:
     - One patient → multiple encounters → multiple transactions per encounter.
     - Large tables: `Transactions`, `Encounters`. Smaller tables: `Providers`, `Departments`.
     - Multiple hospitals may have different schemas; **surrogate keys and a common data model** are implemented to merge data cleanly.

2. **Claims Data**
   - Provided by payers/insurance companies as **flat files (CSV)** in a Landing Zone **Azure Data Lake Storage Gen2**.
   - Includes claim ID, transaction ID, patient ID, encounter ID, provider ID, service date, claim amount, paid amount, claim status.
   - Monthly updates; ingested to **Bronze layer** in Parquet format for further processing.

3. **NPI Data**
   - Retrieved via  **API** providing up-to-date provider details with unique 10-digit National Provider Identifier.
   - Enables validation and enrichment of provider information across hospital databases.

4. **ICD Codes**
   - Standardized diagnosis codes and descriptions used to classify patient conditions.
   - **Retrieved via API** and mapped to transactions/encounters to enable disease-specific analytics and KPIs.

   



