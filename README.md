# AdventureWorks Sales Data Warehouse — SSIS

## Overview

This project builds a dimensional **Sales Data Warehouse** from AdventureWorks-style source data using **SQL Server Integration Services (SSIS)**.

It demonstrates the full warehouse-loading lifecycle: dimension ETL, surrogate-key lookups, slowly changing dimensions, an initial full fact load, and a separate incremental fact-load process.

```text
AdventureWorks Source
        │
        ▼
     SSIS ETL
        │
        ├── Customer Dimension
        ├── Product Dimension
        ├── Territory Dimension
        └── Date Dimension
        │
        ▼
 Dimensional Warehouse
        │
        ├── Full Sales Fact Load
        └── Incremental Sales Fact Load
```

---

## Why This Project Is Different

This repository focuses on an **AdventureWorks warehouse implementation** and explicitly demonstrates both:

- an initial / full fact load;
- an incremental fact load.

That makes it useful for showing how a warehouse can be populated initially and then maintained efficiently as new source data arrives.

---

## ETL Packages

### `Customer Dim ETL.dtsx`
Loads the customer dimension and uses SSIS Slowly Changing Dimension logic to manage changes in customer attributes.

### `Product Dim ETL.dtsx`
Loads the product dimension and includes SCD processing, lookups, derived columns, and update logic.

### `Terroitory Dim ETl.dtsx`
Loads territory-related dimensional data used by the sales model.

### `Date Dim ETL.dtsx`
Builds / loads the date dimension required for time-based reporting and fact-table date relationships.

### `Fact sales - Full load.dtsx`
Performs the initial population of the sales fact table.

### `Fact sales - Incremental load.dtsx`
Loads newly available sales records without rebuilding the entire fact table.

---

## Repository Structure

```text
sales_data_DW/
│
├── AdventureworksDW.sln
├── README.md
│
└── AdventureworksProject/
    ├── Customer Dim ETL.dtsx
    ├── Product Dim ETL.dtsx
    ├── Terroitory Dim ETl.dtsx
    ├── Date Dim ETL.dtsx
    ├── Fact sales - Full load.dtsx
    ├── Fact sales - Incremental load.dtsx
    ├── SourceDB.conmgr
    ├── DestinationDW.conmgr
    ├── Project.params
    └── AdventureworksProject.dtproj
```

---

## Data Warehouse Design

The project follows a dimensional modeling approach in which descriptive business entities are loaded into dimensions before facts are processed.

```text
                DimCustomer
                    │
DimProduct ───── FactSales ───── DimTerritory
                    │
                 DimDate
```

Fact rows resolve dimensional references through lookups so warehouse keys can be used instead of relying only on operational business keys.

---

## Slowly Changing Dimensions

The Customer and Product dimension packages contain SSIS **Slowly Changing Dimension** processing.

This demonstrates how warehouse dimensions can react to attribute changes instead of simply reloading every row as if nothing had changed.

The packages also use components such as:

- Lookup;
- Derived Column;
- OLE DB Command;
- SSIS SCD transformation.

---

## Full vs Incremental Loading

### Full Load

The full-load package is intended for the first population or controlled rebuild of the sales fact table.

```text
Source Sales
    │
    ▼
Dimension Lookups
    │
    ▼
Complete Fact Load
```

### Incremental Load

The incremental package focuses on processing new fact records after the initial load.

```text
Existing Warehouse
       +
New Source Sales
       │
       ▼
Dimension Lookups
       │
       ▼
New Fact Records
```

This pattern reduces unnecessary reprocessing and better reflects how production warehouse pipelines are typically maintained.

---

## Tech Stack

- SQL Server Integration Services (SSIS)
- Microsoft SQL Server
- T-SQL
- Visual Studio / SQL Server Data Tools
- Data Warehousing
- Dimensional Modeling
- Slowly Changing Dimensions
- Full and Incremental ETL

---

## How to Run

1. Install SQL Server and Visual Studio with SSIS / SSDT support.
2. Prepare the AdventureWorks source database and a destination warehouse database.
3. Open `AdventureworksDW.sln`.
4. Update:
   - `SourceDB.conmgr`
   - `DestinationDW.conmgr`
5. Run the dimension packages first.
6. Run `Fact sales - Full load.dtsx` for the initial warehouse population.
7. Use `Fact sales - Incremental load.dtsx` for subsequent loads.
8. Validate fact counts and dimension-key relationships after execution.

---

## Data Engineering Concepts Demonstrated

- batch ETL;
- dimensional modeling;
- fact and dimension separation;
- surrogate-key lookups;
- slowly changing dimensions;
- initial warehouse loading;
- incremental processing;
- source / destination connection separation;
- dependency ordering between dimensions and facts.

---

## Related Project

This repository is different from **SalesDWH**.

- **sales_data_DW** → AdventureWorks-based warehouse with Customer, Product, Territory, Date, and both full + incremental fact loading.
- **SalesDWH** → custom Sales OLTP → DWH pipeline with Customer, Product, Salesman, SCD processing, and incremental fact loading.

This distinction is intentional: the two repositories demonstrate similar SSIS concepts against different schemas and loading designs.

---

# 💫 About Me:
🔭 I’m currently working on: Building scalable data pipelines and cloud-based data platforms using Python, PySpark, SQL, dbt, and GCP.<br>
👯 I’m looking to collaborate on: Data Engineering, ETL/ELT, Big Data, Cloud, and open-source data projects.<br>
🤝 I’m looking for help with: Advanced Data Engineering architectures, real-time streaming, and scalable cloud solutions.<br>
🌱 I’m currently learning: Advanced dbt, Databricks, Apache Spark, Kafka, Terraform, and modern DataOps practices.<br>
💬 Ask me about: Python, SQL, PySpark, dbt, BigQuery, GCP, ETL/ELT pipelines, Apache Airflow, and Data Engineering.<br>
⚡ Fun fact: I enjoy turning messy raw data into clean, reliable pipelines—and explaining how they work to others.

## 🌐 Socials:
[![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://facebook.com/khaled.salah5148) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://instagram.com/khaled_salah5148) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://linkedin.com/in/khaled-salah5148) [![email](https://img.shields.io/badge/Email-D14836?logo=gmail&logoColor=white)](mailto:khaled.salah2803@gmail.com)

# 💻 Tech Stack:
![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white) ![Markdown](https://img.shields.io/badge/markdown-%23000000.svg?style=for-the-badge&logo=markdown&logoColor=white) ![PowerShell](https://img.shields.io/badge/PowerShell-%235391FE.svg?style=for-the-badge&logo=powershell&logoColor=white) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![Google Cloud](https://img.shields.io/badge/GoogleCloud-%234285F4.svg?style=for-the-badge&logo=google-cloud&logoColor=white) ![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white) ![Apache Kafka](https://img.shields.io/badge/Apache%20Kafka-000?style=for-the-badge&logo=apachekafka) ![Apache Spark](https://img.shields.io/badge/Apache%20Spark-FDEE21?style=for-the-badge&logo=apachespark&logoColor=black) ![Apache Hive](https://img.shields.io/badge/Apache%20Hive-FDEE21?style=for-the-badge&logo=apachehive&logoColor=black) ![Apache Hadoop](https://img.shields.io/badge/Apache%20Hadoop-66CCFF?style=for-the-badge&logo=apachehadoop&logoColor=black) ![Jinja](https://img.shields.io/badge/jinja-white.svg?style=for-the-badge&logo=jinja&logoColor=black) ![Snowflake](https://img.shields.io/badge/snowflake-%2329B5E8.svg?style=for-the-badge&logo=snowflake&logoColor=white) ![Apache Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?style=for-the-badge&logo=Apache%20Airflow&logoColor=white) ![Jenkins](https://img.shields.io/badge/jenkins-%232C5263.svg?style=for-the-badge&logo=jenkins&logoColor=white) ![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white) ![MySQL](https://img.shields.io/badge/mysql-4479A1.svg?style=for-the-badge&logo=mysql&logoColor=white) ![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white) ![MicrosoftSQLServer](https://img.shields.io/badge/Microsoft%20SQL%20Server-CC2927?style=for-the-badge&logo=microsoft%20sql%20server&logoColor=white) ![Canva](https://img.shields.io/badge/Canva-%2300C4CC.svg?style=for-the-badge&logo=Canva&logoColor=white) ![Figma](https://img.shields.io/badge/figma-%23F24E1E.svg?style=for-the-badge&logo=figma&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white) ![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white) ![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white) ![Playwright](https://img.shields.io/badge/-playwright-%232EAD33?style=for-the-badge&logo=playwright&logoColor=white) ![Selenium](https://img.shields.io/badge/-selenium-%43B02A?style=for-the-badge&logo=selenium&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) ![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white) ![Power Bi](https://img.shields.io/badge/power_bi-F2C811?style=for-the-badge&logo=powerbi&logoColor=black) ![Splunk](https://img.shields.io/badge/splunk-%23000000.svg?style=for-the-badge&logo=splunk&logoColor=white) ![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white)

# 📊 GitHub Stats:
![](https://github-readme-stats.shion.dev/api?username=khaledsalah5&theme=github_dark&hide_border=true&include_all_commits=true&count_private=false)<br/>
![](https://streak-stats.demolab.com/?user=khaledsalah5&theme=github_dark&hide_border=true)<br/>
![](https://github-readme-stats.shion.dev/api/top-langs/?username=khaledsalah5&theme=github_dark&hide_border=true&include_all_commits=true&count_private=false&layout=compact)

### ✍️ Random Dev Quote
![](https://quotes-github-readme.vercel.app/api?type=horizontal&theme=radical)

---
[![](https://komarev.com/ghpvc/?username=khaledsalah5&icon=0&color=0)](https://visitcount.itsvg.in)
