# Azure Sales Project

This project demonstrates a data pipeline workflow using Azure services to manage and process sales orders. The workflow includes data validation, storage, and processing using various Azure components.

## Project Use-case

A third-party service drops a file named `orders.csv` in the landing folder. The following checks are performed as soon as the file arrives:

1. **Check for Duplicate Order ID**: Ensure there are no duplicate order IDs in `order_status`.
2. **Check for Valid Order Status**: Validate the order status against a list of predefined statuses.

If both conditions are met, the file is moved to the staging folder; otherwise, it is moved to the discarded folder.

## Visual Representation of Use-case

![Data Validation Flow](Images/Data%20Validation%20Flow.png)

_This flowchart depicts the validation checks and file movement between directories._

## Solution Components

### Storage Account

- **Create a storage account** with an enabled hierarchical namespace.
- **Create a container** named `sales` with directories: landing, staging, discarded.

![ADLS Gen 2 Container Structure](Images/ADLS%20Gen%202%20Container%20Structure.png)

_Diagram showing the structure of the Storage Account._

### Databricks

- **Create a Databricks Workspace** to execute Spark code for necessary data validation checks.

![Databricks Workspace](Images/Databricks_Workspace.png)

_Diagram showing the Databricks workspace._

### Data Factory

- **Create a Data Factory Service** to orchestrate the data pipeline:
  - Connect to Azure Data Lake Storage Gen2 (ADLS Gen2).
  - Connect to Databricks using a Key Vault for storing passwords/secret keys.

![Component Connections and Security](Images/Component%20Connections%20and%20Security.png)

_Diagram showing the connection between Data Factory, Databricks, ADLS Gen2, and Azure SQL Database and the use of Key Vault._

### Azure SQL Database

- **Create an Azure SQL Database** for storing valid order statuses in a lookup table.
- **Migrate customer data from ADLS Gen2 to the Azure SQL Database**.

![Azure SQL Database](Images/Azure_SQL_Database.png)

_Diagram showing the Azure SQL Database._

### Databricks Notebook

- **Mount the Storage Account** to Databricks.
- **Load `orders.csv` into a DataFrame** and perform validations.
- **Connect to Azure SQL DB** for order status validation.

## Pipeline Automation

### Data Factory Pipeline

- Add the Databricks Notebook to the pipeline.
- Configure a Storage Event Trigger to execute the pipeline when new files are added in landing folder.

![Data Factory Pipeline](Images/Data%20Factory%20Pipeline.png)

_Diagram showing the Azure Data Factory pipeline._

## Visual Representation of Pipeline Triggering

![Data Ingestion and Triggering](Images/Data%20Ingestion%20and%20Triggering.png)

_Diagram depicting the trigger and initial data processing._

## Enhancements

### Dynamic Filename Handling

- Implement a parameterized approach to dynamically read and process files without hardcoding specific filenames.

![Dynamic Filename Handling](Images/Dynamic%20Filename%20Handling.png)

_Diagram depicting the dynamic filename handling process._

### Generalized Pipeline

- Develop a generic mount code to handle storage mounting dynamically.

### Secure Key Access

- Use Key Vault to securely access the Storage Account key and database password.

## Scenario Mimicking

1. **Order Items Data**: Simulate a third-party service adding `order_items.json` file in Amazon S3 and loading it to ADLS Gen2 using Azure Data Factory.
2. **Customer Data**: Publish customer data in Azure SQL DB from ADLS Gen2 customers folder using Azure Data Factory.

## Overall Pipeline View

![Data Pipeline Overview](Images/Data%20Pipeline%20Overview.png)

_An overview diagram of the data pipeline._

## Project Requirements

- **Number of orders placed by each customer**.
- **Amount spent by each customer**.

## Solution

- Join tables (orders, order_items, customers) to calculate the required metrics.

## End-to-End Workflow

🔹 **Orders Processing**

* Triggered when `orders.csv` is uploaded to ADLS Gen2.
* Validates duplicate order IDs and order status using Azure Databricks.
* Moves valid files to staging and invalid files to discarded.

🔹 **Order Items Processing (Amazon S3 → ADLS Gen2)**

* Order items are stored in Amazon S3 (JSON format).
* Azure Data Factory copies the data from S3 to ADLS Gen2 and converts it to CSV.

🔹 **Customers Data Processing (Azure SQL DB → ADLS Gen2)**

* Customer data is published in Azure SQL DB.
* ADF copies it to ADLS Gen2 for further analysis.

🔹 **Data Analysis & Reporting**

* Joins orders, order items, and customers datasets.
* Computes:
    * ✅ Total orders per customer
    * ✅ Total amount spent per customer