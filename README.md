# Healthcare Insurance Data Warehouse & ETL

An end-to-end **Healthcare Insurance Data Warehouse and ETL project** built with **Microsoft SQL Server Integration Services (SSIS)** and **SQL Server**.

The project focuses on transforming healthcare insurance data through an ETL pipeline and organizing it into structured **Silver and Gold layers** for analytical purposes.

---

## 📌 Project Overview

Healthcare insurance organizations manage large amounts of data related to insured members, insurance policies, dates, age groups, and financial claims.

The goal of this project is to build a centralized **Data Warehouse** that integrates and transforms insurance-related data into an analytical structure.

The ETL process is implemented using **SQL Server Integration Services (SSIS)**, where data is extracted, transformed, and loaded into different warehouse tables.

The final warehouse provides a structured foundation for analyzing insurance policies, insured members, and financial claims.

---

## 🏗️ Architecture

The project follows a layered Data Warehouse architecture:

```text
                    ┌──────────────────────┐
                    │     Source Data      │
                    │ Healthcare Insurance │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │        SSIS          │
                    │   ETL Pipelines      │
                    └──────────┬───────────┘
                               │
                ┌──────────────┴──────────────┐
                │                             │
                ▼                             ▼
       ┌─────────────────┐           ┌─────────────────┐
       │  Silver Layer   │           │   Dimensions    │
       │ Cleaned Data    │           │ Date / Insured  │
       │ Financial Claim │           │ Policy / Age    │
       └────────┬────────┘           └────────┬────────┘
                │                             │
                └──────────────┬──────────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │      Gold Layer      │
                    │  fact_insurance      │
                    └──────────────────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │     Analytics / BI   │
                    └──────────────────────┘
```

---

## 🔄 ETL Process

The ETL workflow is implemented using multiple **SSIS packages**.

### 1. Data Extraction

Insurance-related data is extracted and prepared for processing through SSIS data-flow tasks.

### 2. Data Transformation

The ETL pipelines perform transformations required to prepare the data for the warehouse, including the creation and loading of:

* Insured members
* Insurance policies
* Dates
* Age bands
* Financial claims
* Supporting identifiers

### 3. Data Loading

The transformed data is loaded into the SQL Server Data Warehouse.

The project separates data into **Silver** and **Gold** layers.

For example, the `load_financial_claims.dtsx` package loads financial claim data into the:

```text
silver.financial_claim
```

table.

The final fact-loading package loads data into:

```text
gold.fact_insurance
```

using an ADO.NET destination.

---

## ⭐ Main ETL Packages

| Package                      | Purpose                                  |
| ---------------------------- | ---------------------------------------- |
| `load_insured.dtsx`          | Load insured member data                 |
| `load_policy.dtsx`           | Load insurance policy data               |
| `load_DateInsured.dtsx`      | Load insured-date information            |
| `load_IdComponent.dtsx`      | Load supporting ID/component information |
| `load_age_band.dtsx`         | Create and load age-band information     |
| `load_financial_claims.dtsx` | Load and transform financial claims      |
| `load_gold_date.dtsx`        | Load the Gold date dimension             |
| `load_gold_insured.dtsx`     | Load the Gold insured dimension          |
| `load_gold_policy.dtsx`      | Load the Gold policy dimension           |
| `load fact.dtsx`             | Load the final insurance fact table      |

These packages are included directly in the repository.

---

## 🗄️ Data Warehouse Layers

### Silver Layer

The Silver layer contains cleaned and transformed insurance data before it is integrated into the final analytical model.

Example:

```text
silver.financial_claim
```

is populated by the `load_financial_claims` SSIS package.

### Gold Layer

The Gold layer contains the final analytical structures.

The main fact table is:

```text
gold.fact_insurance
```

which is populated by the `load fact` SSIS package.

The repository also contains dedicated ETL packages for the Gold date, insured, and policy structures.

---

## 🧩 Data Warehouse Model

The project is organized around insurance-related entities such as:

```text
                 ┌───────────────┐
                 │  Dim Insured  │
                 └───────┬───────┘
                         │
                         │
┌─────────────┐    ┌─────▼─────────────┐    ┌─────────────┐
│  Dim Date   │────│  Fact Insurance   │────│ Dim Policy  │
└─────────────┘    └─────────┬─────────┘    └─────────────┘
                             │
                             │
                      ┌──────▼───────┐
                      │ Age / Other  │
                      │ Dimensions   │
                      └──────────────┘
```

This structure allows insurance-related measures to be analyzed across dimensions such as insured members, policies, and dates.

---

## 🛠️ Technologies

### Data Engineering

* **SQL Server**
* **SQL Server Integration Services (SSIS)**
* **T-SQL**
* **ADO.NET**

### Data Warehouse

* Silver Layer
* Gold Layer
* Fact tables
* Dimension tables
* ETL pipelines

### Development Tools

* Visual Studio
* SQL Server Data Tools (SSDT)
* Git
* GitHub

---

## 📂 Repository Structure

```text
Healthcare-Insurance-Data-Warehouse/
│
├── Package1.dtsx
├── Project.params
│
├── datawarehouse finalllllll.database
├── datawarehouse finalllllll.dtproj
├── datawarehouse finalllllll.slnx
│
├── load_insured.dtsx
├── load_policy.dtsx
├── load_DateInsured.dtsx
├── load_IdComponent.dtsx
├── load_age_band.dtsx
│
├── load_financial_claims.dtsx
│
├── load_gold_date.dtsx
├── load_gold_insured.dtsx
├── load_gold_policy.dtsx
│
├── load fact.dtsx
│
├── .gitignore
└── .gitattributes
```

The repository currently contains the SSIS project files, project configuration, warehouse database definition, and individual ETL packages.

---

## ⚙️ How to Run the Project

### Prerequisites

Before running the project, install:

* SQL Server
* Visual Studio
* SQL Server Data Tools (SSDT)
* SQL Server Integration Services
* Git

### 1. Clone the Repository

```bash
git clone https://github.com/1dhouha/Healthcare-Insurance-Data-Warehouse.git
```

```bash
cd Healthcare-Insurance-Data-Warehouse
```

### 2. Open the SSIS Project

Open the solution/project file:

```text
datawarehouse finalllllll.slnx
```

or the corresponding SSIS project:

```text
datawarehouse finalllllll.dtproj
```

### 3. Configure SQL Server

The SSIS packages currently use a SQL Server connection targeting:

```text
localhost
```

and the database:

```text
insurrance_DataWarehouse
```

The repository's SSIS configuration shows Windows Integrated Security being used for this connection.

You should therefore verify the connection manager before executing the packages.

### 4. Execute the ETL Packages

Run the packages according to the ETL dependency order.

A typical workflow is:

```text
Source Data
     │
     ▼
Silver ETL
     │
     ├── Insured
     ├── Policy
     ├── Date
     ├── Age Band
     └── Financial Claims
     │
     ▼
Gold ETL
     │
     ├── Gold Date
     ├── Gold Insured
     ├── Gold Policy
     └── Fact Insurance
     │
     ▼
gold.fact_insurance
```

---

## 📊 Analytical Use Cases

The resulting warehouse can be used to support analysis such as:

* Insurance policy analysis
* Insured member analysis
* Financial claim analysis
* Claims by date
* Claims by age group
* Policy-related analysis
* Financial performance analysis
* Insurance trends over time

The Gold layer provides a more structured foundation for connecting the warehouse to BI tools such as **Power BI**.

---

## 🎯 Project Objectives

The main objectives of this project are to:

* Build a healthcare insurance Data Warehouse
* Design an ETL pipeline using SSIS
* Transform raw insurance data into structured warehouse data
* Separate data processing into Silver and Gold layers
* Build fact and dimension structures
* Automate data loading through SSIS packages
* Prepare data for analytical and Business Intelligence use cases
* Practice real-world Data Engineering concepts

---

## 🔐 Data Privacy

This project is intended for **educational and portfolio purposes**.

No real patient or insured-person information should be used in the repository.

When working with healthcare-related data, synthetic or anonymized data should be preferred to protect sensitive personal information.

---

## 🚀 Future Improvements

Possible improvements include:

* Add a complete data model / star-schema diagram
* Add source-to-target mapping documentation
* Add data-quality validation checks
* Add SSIS error handling and logging
* Add incremental loading
* Add ETL execution documentation
* Add Power BI dashboards
* Add project screenshots
* Add automated deployment
* Improve connection management using SSIS parameters/environment variables
* Add documentation for each SSIS package

---

## 📚 Learning Outcomes

This project demonstrates practical experience with:

* Data Warehousing
* ETL Development
* SSIS
* SQL Server
* Dimensional Modeling
* Fact and Dimension Tables
* Data Transformation
* Data Integration
* Data Engineering
* Healthcare Insurance Analytics

---

## 👩‍💻 Author

**Dhouha Ichaoui**

Data Engineering & AI Engineering Student

---

## 🔗 Repository

[Healthcare Insurance Data Warehouse](https://github.com/1dhouha/Healthcare-Insurance-Data-Warehouse)

---

⭐ If you find this project useful, feel free to explore the repository and the SSIS ETL packages.
