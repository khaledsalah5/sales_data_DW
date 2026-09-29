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

---

## Author

**Khaled Salah — Data Engineer**  
[LinkedIn](https://www.linkedin.com/in/khaled-salah5148/) · [Portfolio](https://khaledsalah5.github.io/Portfolio/) · [GitHub](https://github.com/khaledsalah5) · [Email](mailto:khaled.salah2803@gmail.com)
