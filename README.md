# FMCG Sales Analytics Using Databricks

An end-to-end data analytics project built on **Databricks** using the
**Medallion Architecture** (Bronze, Silver, and Gold). The project
organizes raw FMCG sales data into structured Delta tables and explores
the cleaned Silver-layer data to understand sales, customers, products,
stores, and purchasing behaviour.

> **Project status:** Bronze and Silver layers have been prepared, and
> exploratory data analysis (EDA) has been performed on the Silver
> layer. The Gold schema has been created as the next layer for curated,
> business-ready datasets.

------------------------------------------------------------------------

## Table of Contents

-   [Project Overview](#project-overview)
-   [Objectives](#objectives)
-   [Architecture](#architecture)
-   [Dataset and Data Model](#dataset-and-data-model)
-   [Data Preparation](#data-preparation)
-   [Exploratory Data Analysis](#exploratory-data-analysis)
-   [Tools and Technologies](#tools-and-technologies)
-   [Project Structure](#project-structure)
-   [Key Dataset Metrics](#key-dataset-metrics)
-   [How to Explore the Project](#how-to-explore-the-project)
-   [Future Enhancements](#future-enhancements)

------------------------------------------------------------------------

## Project Overview

FMCG (Fast-Moving Consumer Goods) businesses generate large volumes of
transactional data across customers, stores, products, and sales
channels. Turning this raw data into reliable information requires a
structured data-processing workflow.

In this project, I use Databricks to organize the source data through a
Medallion Architecture:

1.  **Bronze:** Raw source data is ingested and retained in its landing
    layer.
2.  **Silver:** Data is cleaned, standardized, validated, and organized
    into analytical tables.
3.  **Gold:** A schema is created for future business-level aggregations
    and reporting datasets.

After preparing the Silver layer, I performed SQL-based EDA to examine
sales patterns, product and brand performance, customer behaviour, store
performance, and data quality.

## Objectives

-   Implement a lakehouse-style data organization using the Medallion
    Architecture.
-   Ingest and organize multiple raw CSV files in Databricks.
-   Clean and validate data using PySpark and SQL.
-   Build reusable, structured Silver-layer Delta tables.
-   Explore sales performance and customer purchasing behaviour.
-   Investigate product hierarchy, brand, store, geography, time,
    payment, and loyalty patterns.
-   Prepare a foundation for curated Gold-layer tables and downstream
    reporting.

## Architecture

``` text
                 Raw FMCG CSV Files (8 files)
                            |
                            v
                 +----------------------+
                 |   BRONZE LAYER       |
                 |   workspace.bronze   |
                 |   Raw ingested data  |
                 +----------------------+
                            |
                     Cleaning and
                     standardization
                            |
                            v
                 +----------------------+
                 |   SILVER LAYER       |
                 |   workspace.silver   |
                 |   Clean Delta tables |
                 +----------------------+
                            |
                    SQL-based EDA
                            |
                            v
                 +----------------------+
                 |    GOLD LAYER        |
                 |   workspace.gold     |
                 |   Created for        |
                 |   curated datasets   |
                 +----------------------+
```

### Layer responsibilities

  -----------------------------------------------------------------------
  Layer                   Schema                  Purpose
  ----------------------- ----------------------- -----------------------
  Bronze                  `workspace.bronze`      Raw data landing layer;
                                                  preserves source data
                                                  for traceability and
                                                  reprocessing.

  Silver                  `workspace.silver`      Cleaned, standardized,
                                                  validated, and
                                                  structured Delta tables
                                                  used for analysis.

  Gold                    `workspace.gold`        Intended for
                                                  business-ready
                                                  aggregations and
                                                  reporting models.
  -----------------------------------------------------------------------

## Dataset and Data Model

The project starts with **8 raw CSV files** and prepares the following
**7 Silver-layer tables**:

  Table               Description
  ------------------- -------------------------------------------------
  `customer_master`   Customer-related attributes.
  `store_master`      Store information and attributes.
  `sku_master`        SKU-level product information.
  `transactions`      Transaction-level sales information.
  `line_items`        Individual product entries within transactions.
  `sales`             Structured sales dataset used for analysis.
  `dim_date`          Date dimension for time-based analysis.

The product hierarchy used in the analysis is:

``` text
SKU  →  Product  →  Category  →  Department
```

A transaction can contain multiple line items. The `line_item_id`
identifies an individual item entry, while `transaction_id` identifies
the overall purchase and `sku_id` identifies the product/SKU.

## Data Preparation

The Silver-layer preparation includes data-quality and standardization
steps such as:

-   Identifying and handling duplicate transaction records.
-   Treating `ANON` customer identifiers as `NULL` to represent
    anonymous customers consistently.
-   Removing invalid product records.
-   Preparing structured tables for analysis.
-   Storing prepared datasets as Delta tables.

### Data-cleaning results

The recorded preparation results include:

  Data-quality operation                  Records
  ----------------------------------- -----------
  Duplicate transaction IDs removed        77,494
  Invalid product records removed         454,598
  Final sales line items                4,846,789

These figures describe the recorded cleaning workflow and resulting
sales dataset.

## Exploratory Data Analysis

EDA was performed on the Silver-layer tables using SQL in Databricks.

The analysis covered the following areas:

  -----------------------------------------------------------------------
  Analysis area                       Questions explored
  ----------------------------------- -----------------------------------
  Sales overview                      How many transactions and line
                                      items are present? What does
                                      overall sales activity look like?

  Product hierarchy                   How are SKUs distributed across
                                      products, categories, and
                                      departments?

  Brand analysis                      Which brands contribute the most
                                      line items? How do private-label
                                      products compare?

  Customer analysis                   How many customers are represented,
                                      including anonymous customers?

  Store and geography                 How does sales activity vary across
                                      stores and locations?

  Time analysis                       How does sales activity change
                                      across dates and time periods?

  Payment analysis                    What payment methods are used by
                                      customers?

  Loyalty analysis                    How many customers or transactions
                                      participate in the loyalty
                                      programme?

  Data quality                        Are there duplicate identifiers,
                                      missing values, unusual values, or
                                      invalid records?

  Statistical analysis                What are the minimum, average,
                                      median, and maximum values for
                                      selected numeric fields? Are there
                                      potential outliers?
  -----------------------------------------------------------------------

SQL techniques used during the analysis include:

-   `GROUP BY` and aggregate functions
-   `COUNT(DISTINCT ...)`
-   `CASE WHEN`
-   Common Table Expressions (CTEs)
-   Window functions
-   Percentile and median calculations
-   `UNION ALL`
-   Percentage contribution calculations
-   Sorting and ranking

## Key Dataset Metrics

The prepared dataset contains the following recorded metrics:

  Metric                   Value
  ------------------ -----------
  Transactions         1,375,105
  Stores                      21
  Customers               14,359
  Sales line items     4,846,789

## Tools and Technologies

-   **Databricks** -- Lakehouse environment and notebook development
-   **Apache Spark / PySpark** -- Data preparation and transformation
-   **Spark SQL** -- Data exploration and analysis
-   **Delta Lake** -- Storage format for prepared tables
-   **Medallion Architecture** -- Bronze, Silver, and Gold data
    organization
-   **SQL** -- Aggregations, data-quality checks, and analytical queries

## Project Structure

The notebooks currently documented for the workflow are:

``` text
FMCG-Sales-Analytics/
│
├── 1-5-prepare-silver-layer.ipynb
├── 1-6-exploratory-data-analysis.ipynb
│
└── README.md
```

The raw CSV files and Databricks tables are maintained in the Databricks
environment. Add exported notebooks, screenshots, or supporting files to
this repository if you want visitors to be able to reproduce or review
the work directly from GitHub.

## How to Explore the Project

1.  Open the notebooks in Databricks.
2.  Ensure the source CSV files are available in the expected Databricks
    storage location.
3.  Run the Silver-layer preparation notebook to review the
    data-cleaning and table-building workflow.
4.  Review the EDA notebook to explore the SQL analysis and findings.
5.  Use the `workspace.silver` tables to run additional analysis.

> **Note:** The notebook workflow may depend on the original Databricks
> catalog, schema, volume, and file paths. Update those paths if you run
> the notebooks in a different workspace.

## Future Enhancements

-   Build Gold-layer fact and dimension tables for business reporting.
-   Create KPI-ready aggregate tables for sales, products, customers,
    and stores.
-   Develop a Power BI dashboard using curated Gold-layer data.
-   Add automated data-quality checks and validation rules.
-   Document additional business insights and visualizations as the
    analysis progresses.

------------------------------------------------------------------------

## About

This project demonstrates practical work with Databricks, PySpark, Spark
SQL, Delta tables, data cleaning, Medallion Architecture, and
exploratory data analysis on FMCG sales data.

If you find this project useful or have suggestions, feel free to
connect or share feedback.
