# Azure Data Engineering Project: Automated Data Pipelines

## Problem Statement

![Problem Statement](/screenshots/problem_statements.png)

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

## Linked Services

![Linked Service 1](/screenshots/project1.png)
![Linked Service 2](/screenshots/project4.png)
![Linked Service 3](/screenshots/project6.png)

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

![image$i](/screenshots/project11.png)

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

## Steps to Follow

### Step 1: Create a Pipeline to Fetch Country Data

1. **Open Azure Data Factory (ADF):**
   - Navigate to the Azure portal and open your ADF instance.

2. **Create a New Pipeline:**
   - Click on "Author" to create a new pipeline.

3. **Add a Web Activity:**
   - Drag and drop a "Web" activity onto the pipeline canvas.
   - Configure the Web activity to call the REST API endpoint `https://restcountries.com/v3.1/name/{name}` for each country (India, US, UK, China, Russia).

4. **Loop Through Countries:**
   - Use a "ForEach" activity to loop through the list of countries.
   - Pass the country name dynamically to the Web activity.

5. **Save Data to ADLS:**
   - Add a "Copy Data" activity inside the ForEach loop.
   - Configure the sink to save the JSON response to Azure Data Lake Storage (ADLS) with the file name set to the country name.

![image$i](/screenshots/project9.png)
![image$i](/screenshots/project8.png)

### Step 2: Add a Trigger to the Pipeline

1. **Create a New Trigger:**
   - Go to the "Manage" tab and select "Triggers."
   - Click on "+ New" to create a new trigger.

2. **Configure the Trigger:**
   - Set the trigger type to "Schedule."
   - Configure the schedule to run twice daily at 12:00 AM and 12:00 PM IST.
   - Use a CRON expression to set the schedule: `0 0 0 * * *` for 12:00 AM and `0 0 12 * * *` for 12:00 PM.

3. **Associate the Trigger with the Pipeline:**
   - Select the pipeline created in Step 1 to be triggered by this schedule.

![image$i](/screenshots/project12.png)
![image$i](/screenshots/project15.png)

### Step 3: Create a Pipeline to Copy Customer Data

1. **Create a New Pipeline:**
   - Click on "Author" to create a new pipeline for copying customer data.

2. **Add a Lookup Activity:**
   - Drag and drop a "Lookup" activity onto the pipeline canvas.
   - Configure the Lookup activity to fetch the record count from the customer table in the database.

3. **Add an If Condition Activity:**
   - Drag and drop an "If Condition" activity onto the pipeline canvas.
   - Set the condition to check if the record count is greater than 500.

4. **Add a Copy Data Activity:**
   - Inside the "If Condition" activity, add a "Copy Data" activity.
   - Configure the source to be the customer table and the sink to be ADLS.

5. **Add an Execute Pipeline Activity:**
   - Inside the "If Condition" activity, add an "Execute Pipeline" activity to trigger the child pipeline.
   - Configure the Execute Pipeline activity to pass the customer record count as a parameter to the child pipeline.

![image$i](/screenshots/project17.png)
![image$i](/screenshots/project16.png)
![image$i](/screenshots/project18.png)
![image$i](/screenshots/project22.png)

### Step 4: Design the Child Pipeline for Product Data

1. **Create a New Pipeline:**
   - Click on "Author" to create a new pipeline for copying product data.

2. **Add a Parameter:**
   - Define a parameter in the child pipeline to receive the customer record count from the parent pipeline.

3. **Add an If Condition Activity:**
   - Drag and drop an "If Condition" activity onto the pipeline canvas.
   - Set the condition to check if the customer record count parameter is greater than 600.

4. **Add a Copy Data Activity:**
   - Inside the "If Condition" activity, add a "Copy Data" activity.
   - Configure the source to be the product table and the sink to be ADLS.

![image$i](/screenshots/project24.png)

### Step 5: Test and Monitor the Pipelines

1. **Test the Pipelines:**
   - Manually trigger the pipelines to ensure they run as expected.
   - Verify that the data is correctly fetched, copied, and saved.

2. **Monitor the Pipelines:**
   - Use the ADF monitoring tools to track pipeline runs, errors, and performance.
   - Set up alerts and notifications for pipeline failures or issues.

![image$i](/screenshots/pipeline_success.png)

## Future Enhancements

- Add data validation checks.
- Implement incremental copy logic.
- Enable email or Teams notifications.
- Expand to dynamic country list ingestion.
- Introduce cold storage archiving for older exports.

### Author

Shivankur Sharma
CT_CSI_DE_6133
Data Engineering Intern
