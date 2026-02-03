# Trucking Operations Analytics - Enterprise Style Solution 

This project demonstrates how Microsoft Fabric can be used to design and deliver a modern, scalable analytics solution from raw class 8 logistics operational data to decision-ready insights. 

Business Objective.
 ---
The primary goal of this project is to transform raw logistics operational data into trusted analytical hub, enable self-service and executive reporting, and demonstrate strong analytics engineering and BI design principles using Microsoft Fabric. This solution supports analytical use cases for the following question asked: 
 1. **Driver performance**: On-time rates, MPG, revenue per mile
 2. **Route profitability**: Revenue vs costs by lane
 3. **Fleet utilization**: Miles per truck, revenue per asset
 4. **Maintenance analysis**: Cost per mile, downtime impact
 5. **Fuel efficiency**: MPG trends, fuel cost by route
 6. **Customer analysis**: Revenue by customer, service levels
 7. **Safety metrics**: Incident rates, preventable accidents
 8. **Seasonal patterns**: Load volume, rate fluctuations
 
## Solution Architecture (Microsoft Fabric)

The solution follows industry best practices in:

- **Lakehouse architecture** (Bronze / Silver / Gold)
- **Data modeling and semantic layer design**
- **Orchestration with pipelines**
- **Performance-optimized reporting** using Power BI in Fabric
- **Full end-to-end documentation**
 
```java
Source Data 
    ↓
Bronze Lakehouse (Raw Ingestion)
    ↓
Silver Lakehouse (Cleansed & Modeled)
    ↓
Gold Lakehouse (Business-Ready Tables)
    ↓
Semantic Model (Metrics & Relationships)
    ↓
Power BI Reports (Insights & Storytelling)
```
<strong>Source Data Repository :</strong>
<a href="https://github.com/Xyxtvs/logistics-operations-dataset" target="_blank" rel="noopener noreferrer">
Logistics Operations Dataset
</a>

<img width="486" height="296" alt="image" src="https://github.com/user-attachments/assets/43c83b52-3580-4b22-8a26-c03849719ab2" />

 
**Architecture Design Principles**
---
- **Separation of concerns**: ingestion, transformation, and presentation are clearly isolated

- **Scalability**: Lakehouse tables designed for growth and reprocessing

- **Performance**: optimized semantic model for fast report rendering

- **Reusability**: curated Gold tables and shared measures
 
**Planned enhancements**
---
- CI/CD with Fabric Git integration <strong>--></strong><a href="https://github.com/DallasBaba/fabric-end-to-end-analytics/blob/main/Direct_git_integration.md" target="_blank" rel="noopener noreferrer">Direct Git Integration</a>
- Environment parameterization (Dev/Test/Prod)
- Incremental refresh strategies
- Advanced data quality monitoring

**Credits & Acknowledgements**
---  
- Data source: **Kaggle.com**
- Dataset by: **Yogae Rodríguez** <a href="https://www.kaggle.com/datasets/yogape/logistics-operations-database" target="_blank" rel="noopener noreferrer"> Logistics Operations Dataset </a>
- Project design & implementated by: **Babatunde Dallas** <a href="https://app.powerbi.com/groups/208d4cf2-7cbe-4e1a-bdb4-e953bc245140/reports/b9d0f0da-b011-476e-be64-1dcefd1ba552/76729e2e013e45020575?experience=power-bi" target="_blank" rel="noopener noreferrer"> Trucking Operations Analytics </a>
 
