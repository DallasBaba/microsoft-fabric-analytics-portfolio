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

 
<img width="958" height="436" alt="01 - Create a new WS" src="https://github.com/user-attachments/assets/b6b8a9fe-55e3-46b9-a85e-1960ef6d77c6" />


---

### 2️ - Bronze Layer – Raw Data Ingestion
Raw CSV files were loaded into **Bronze_LandingStage_LH** with no transformations applied.

**Key Characteristics**
- Original schema preserved
- Files stored for traceability and reprocessing
 
<img width="959" height="459" alt="02 - Use Lakehouse to load data in Fabric WS" src="https://github.com/user-attachments/assets/83c2238b-5afa-43f9-a23b-db5a243c7943" />



---

### 3️ - Bronze Layer – Ingestion Validation
Verification that all source files loaded successfully into the Bronze Lakehouse.

 
<img width="959" height="460" alt="02 B -" src="https://github.com/user-attachments/assets/aeec9f10-6349-475f-9c50-d3677f9ba7ed" />

---

### 4️ - (Optional) Schema Initialization via Notebook
A notebook was used to create schemas and standardize table structures when required.

 <img width="958" height="460" alt="02 C - optional" src="https://github.com/user-attachments/assets/b4e8e0c9-6e90-4c97-9987-12963fbc52e3" />

 
---

### 5️ - Silver Layer – Data Cleansing & Transformation
Data was cleaned and transformed using **silver_dfgen2**, then loaded into **Silver_LH**.

**Transformations Included**
- Data type normalization
- Deduplication
- Column standardization
 
<img width="924" height="456" alt="03 - dfgen2" src="https://github.com/user-attachments/assets/dfcb768a-af5f-4f88-be78-c92e602898a4" />

---

### 6️ -  Silver Layer – Modeled Data Preview
Validated the structure and content of cleaned tables in Silver_LH.

 
<img width="923" height="459" alt="03 - Silver_LH" src="https://github.com/user-attachments/assets/ec555b33-79ab-4479-935e-433ca590a990" />



---

### 7️ - Exploratory Analysis Using Notebook
Notebook-based analysis was performed to validate metrics and aggregation logic.

**Preview of using a Notebook to perform data analysis from Silver_LH**
<img width="958" height="460" alt="04 b- SQL   R notebook" src="https://github.com/user-attachments/assets/e1a8bf03-8a1b-4efe-b57d-3af8ee7dc76e" />




---

### 8️ - Gold Layer – Pipeline-Based Promotion
A Fabric copy pipeline promoted curated datasets from Silver_LH into **Gold_DWH**.

<img width="1995" height="954" alt="image" src="https://github.com/user-attachments/assets/a8677b65-61b4-49a7-ae37-aa02308829ae" />


<img width="958" height="460" alt="05 - copy_pipeline" src="https://github.com/user-attachments/assets/02fe9c78-892b-4bfa-bc23-cb391e3cfba7" />


---

### 9️ - Gold Layer – Business-Ready Warehouse
Gold_DWH serves as the single source of truth for analytics and BI consumption.

 
<img width="957" height="458" alt="05" src="https://github.com/user-attachments/assets/f72e730b-1ba7-4ed5-b75e-09033812dd8a" />


**Lineage view**
<img width="960" height="457" alt="image" src="https://github.com/user-attachments/assets/3f9fd7d1-9bf2-405d-9bcf-04888d777567" />



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
