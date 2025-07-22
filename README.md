# Azure Data Engineering Project: Automated Data Pipelines

## Project Overview

This project demonstrates an end-to-end data engineering workflow using Azure Data Factory (ADF). It involves automating data ingestion from REST APIs, conditional data movement from SQL databases to Azure Data Lake Storage (ADLS), and orchestrating pipeline dependencies with scheduled triggers.

## Objectives

1. **Fetch Country Data via REST API**
   - Retrieve country information for India, US, UK, China, and Russia.
   - Save the data in separate JSON files named after each country.

2. **Schedule Pipeline Execution**
   - Automatically run the data ingestion pipeline twice daily at 12:00 AM and 12:00 PM IST.

3. **Conditional Data Copy for Customer Data**
   - Copy customer data from the database to ADLS only if the record count exceeds 500.
   - Trigger a child pipeline to copy product data if the customer record count exceeds 600.

4. **Parameter Passing Between Pipelines**
   - Dynamically pass the customer record count from the parent pipeline to the child pipeline using pipeline parameters.

## Architecture Components

- **Azure Data Factory**: Orchestrates the data pipelines.
- **Azure SQL Database**: Source of customer and product data.
- **Azure Data Lake Storage (ADLS)**: Destination for ingested data.
- **REST API Integration**: Source for country metadata.
- **Pipeline Parameters**: Facilitate communication between pipelines.
- **Schedule Trigger**: Automates pipeline execution at specified times.

## Pipeline Details

### 1. Country Data Ingestion Pipeline

- **REST Endpoint**: `https://restcountries.com/v3.1/name/{name}`
- **Countries**: India, US, UK, China, Russia
- **Output**: JSON files saved in ADLS with country names as file names.
- **Error Handling**: Includes retry logic for robustness.

### 2. Scheduled Trigger

- **Frequency**: Twice daily
- **Timings**: 12:00 AM IST and 12:00 PM IST
- **Configuration**: Uses CRON expressions for scheduling.

### 3. Customer Data Pipeline (Parent)

- **Lookup Activity**: Fetches the record count from the customer table.
- **Condition Check**: If the count exceeds 500, data is copied to ADLS.
- **Child Pipeline Trigger**: Invokes the product data pipeline if the customer count exceeds 600.
- **Parameter Passing**: Customer count is passed to the child pipeline.

### 4. Product Data Pipeline (Child)

- **Condition Check**: Executes only if the customer count parameter exceeds 600.
- **Data Copy**: Copies product table data to ADLS.
- **Parameter Validation**: Uses If Condition activity for validation.

## Sample Data Generation

- **Customer Table**: 700 synthetic records.
- **Product Table**: 700 synthetic records.
- **Fields**: IDs, names, contact info, categories, prices, and more.

## Monitoring and Logging

- **Pipeline Activity Tracking**: Via ADF monitoring.
- **Error Alerts**: Retry attempts and execution histories.
- **Trigger Logs**: Duration, status, and timestamp insights.

## Key Features

- Conditional logic for intelligent execution.
- Dynamic parameter passing between linked pipelines.
- Time zone-aware scheduling.
- Modular and reusable design.
- Scalable structure for additional data sources.

## Future Enhancements

- Add data validation checks.
- Implement incremental copy logic.
- Enable email or Teams notifications.
- Expand to dynamic country list ingestion.
- Introduce cold storage archiving for older exports.

## Author

**Shiv Sharma**
Data Engineering Enthusiast
