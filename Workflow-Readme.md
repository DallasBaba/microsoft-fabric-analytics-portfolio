# Microsoft Fabric Workflow

This project showcases practical usage of how raw operational data can be ingested, transformed, modeled, and delivered as decision-ready datasets using a Lakehouse based **Bronze → Silver → Gold architecture**.
- Microsoft Fabric Workspaces
- Lakehouse storage
- Dataflows Gen2 for transformation
- Notebook for analysis
- Pipelines for orchestration  
- A curated Gold data warehouse for analytics consumption

## End-to-End Workflow Breakdown

### 1️ - Workspace Initialization
A dedicated Microsoft Fabric workspace was created to isolate development artifacts and manage the end-to-end analytics lifecycle.

**Purpose**
- Centralize Fabric artifacts (Lakehouse, notebooks, pipelines)
- Establish a development environment aligned with enterprise practices

 
<img width="958" height="436" alt="01 - Create a new WS" src="https://github.com/user-attachments/assets/96473991-bb8a-4425-bed4-578dbe9e7961" />

---

### 2️ - Bronze Layer – Raw Data Ingestion
Raw CSV files were loaded into **Bronze_LandingStage_LH** with no transformations applied.

**Key Characteristics**
- Original schema preserved
- Files stored for traceability and reprocessing
 
<img width="959" height="459" alt="02 - Use Lakehouse to load data in Fabric WS" src="https://github.com/user-attachments/assets/6d6a6150-2b85-4f05-9127-1d75ab96d270" />



---

### 3️ - Bronze Layer – Ingestion Validation
Verification that all source files loaded successfully into the Bronze Lakehouse.

 
<img width="959" height="460" alt="02 B -" src="https://github.com/user-attachments/assets/6ddbc046-e7ed-4b67-addf-f686bc9fdec1" />

---

### 4️ - (Optional) Schema Initialization via Notebook
A notebook was used to create schemas and standardize table structures when required.

 
<img width="958" height="460" alt="02 C - optional" src="https://github.com/user-attachments/assets/3c4e8f19-1fbf-43fa-aae2-6b4d446a4776" />
 
---

### 5️ - Silver Layer – Data Cleansing & Transformation
Data was cleaned and transformed using **silver_dfgen2**, then loaded into **Silver_LH**.

**Transformations Included**
- Data type normalization
- Deduplication
- Column standardization
 
<img width="924" height="456" alt="03 - dfgen2" src="https://github.com/user-attachments/assets/b4d56bb9-96bc-45c8-9dec-5a706cb83f14" />
 

---

### 6️ -  Silver Layer – Modeled Data Preview
Validated the structure and content of cleaned tables in Silver_LH.

 
<img width="923" height="459" alt="03 - Silver_LH" src="https://github.com/user-attachments/assets/3797f572-5564-405c-ae09-8ded8c2c7fc0" />


---

### 7️ - Exploratory Analysis Using Notebook
Notebook-based analysis was performed to validate metrics and aggregation logic.

**Preview of using a Notebook to perform data analysis from Silver_LH**
<img width="958" height="460" alt="04 b- SQL   R notebook" src="https://github.com/user-attachments/assets/03dfa763-f138-427d-b262-7081050b3590" />


---

### 8️ - Gold Layer – Pipeline-Based Promotion
A Fabric copy pipeline promoted curated datasets from Silver_LH into **Gold_DWH**.

 <img width="1995" height="954" alt="image" src="https://github.com/user-attachments/assets/b4373b11-3cce-410a-a76d-2899ccaea447" />

<img width="958" height="460" alt="05 - copy_pipeline" src="https://github.com/user-attachments/assets/c72661e1-3f4c-44fa-a0b3-a984268d34d6" />

---

### 9️ - Gold Layer – Business-Ready Warehouse
Gold_DWH serves as the single source of truth for analytics and BI consumption.

 
<img width="957" height="458" alt="05" src="https://github.com/user-attachments/assets/52224fc6-5701-4bf3-8847-f56964960959" />

**Lineage view**
<img width="954" height="432" alt="image" src="https://github.com/user-attachments/assets/c5e228ac-83dc-4a96-a830-8aec29a6ca74" />


## Key Takeaways
- Full Microsoft Fabric Lakehouse lifecycle implemented
- Clear separation of ingestion, transformation, and consumption layers
- Enterprise-aligned analytics engineering practices


## Future Enhancements
- Semantic model and Power BI reporting
- CI/CD with Fabric Git integration
- Multi-environment parameterization
- Automated data quality checks

## Author
**Babatunde Dallas**  
Business Intelligence & Analytics Engineer   
