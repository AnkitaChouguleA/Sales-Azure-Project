# Azure Sales Project

This project demonstrates a data pipeline workflow using Azure services to manage and process sales orders. The workflow includes data validation, storage, and processing using various Azure components.

## Project Use-case

A third-party service drops a file named `orders.csv` in the landing folder. The following checks are performed as soon as the file arrives:

1. **Check for Duplicate Order ID**: Ensure there are no duplicate order IDs in `order_status`.
2. **Check for Valid Order Status**: Validate the order status against a list of predefined statuses.

If both conditions are met, the file is moved to the staging folder; otherwise, it is moved to the discarded folder.

## Solution Components

### Storage Account
- **Create a storage account** with an enabled hierarchical namespace.
- **Create a container** named `sales` with directories: landing, staging, discarded.

### Databricks
- **Create a Databricks Workspace** to execute Spark code for necessary data validation checks.

### Data Factory
- **Create a Data Factory Service** to orchestrate the data pipeline:
  - Connect to Azure Data Lake Storage Gen2 (ADLS Gen2).
  - Connect to Databricks using a Key Vault for storing passwords/secret keys.

### Azure SQL Database
- **Create an Azure SQL Database** for storing valid order statuses in a lookup table.
- **Migrate customer data from ADLS Gen2 to the Azure SQL Database**.

### Databricks Notebook
- **Mount the Storage Account** to Databricks.
- **Load `orders.csv` into a DataFrame** and perform validations.
- **Connect to Azure SQL DB** for order status validation.

## Pipeline Automation

### Data Factory Pipeline
- Add the Databricks Notebook to the pipeline.
- Configure a Storage Event Trigger to execute the pipeline when new files are added in landing folder.

## Enhancements

### Dynamic Filename Handling
- Implement a parameterized approach to dynamically read and process files without hardcoding specific filenames.

### Generalized Pipeline
- Develop a generic mount code to handle storage mounting dynamically.

### Secure Key Access
- Use Key Vault to securely access the Storage Account key and database password.

## Scenario Mimicking
1. **Order Items Data**: Simulate a third-party service adding `order_items.json` file in Amazon S3 and loading it to ADLS Gen2 using Azure Data Factory.
2. **Customer Data**: Publish customer data in Azure SQL DB from ADLS Gen2 customers folder using Azure Data Factory.

## Project Requirements
- **Number of orders placed by each customer**.
- **Amount spent by each customer**.

## Solution
- Join tables (orders, order_items, customers) to calculate the required metrics.

---

Regarding images, I suggest adding relevant diagrams or flowcharts to visualize the data pipeline workflow and component interactions. For example:

- **Image 1**: An overview diagram of the data pipeline.
- **Image 2**: A flowchart depicting the validation checks and file movement between directories.
- **Image 3**: A diagram showing the connection between Data Factory, Databricks, ADLS Gen2, and Azure SQL Database.

You can add these images to the relevant sections in your README file to provide a clear and visual representation of the project's workflow and components. Here’s an example markdown syntax for adding images:

```markdown
## Overview Diagram
![Overview Diagram](path/to/overview_diagram.png)

## Validation Checks Flowchart
![Validation Checks Flowchart](path/to/validation_checks_flowchart.png)

## Component Connections
![Component Connections](path/to/component_connections.png)
```
