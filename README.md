## Data-Integration-Pipelines-for-NYC-Payroll-Data-Analytics

## Project Overview

This project utilizes high-quality and dynamic data pipelines that can be automated and monitored for efficient operation.  
The goal is to analyze and summarize employee payroll data for the City of New York by department and financial year. 
The data include the financial years 2020 and 2021 and is sourced from the CSV files which are Employee master data and monthly payroll data. 
The source data resides in Azure Data Lake Gen2 needs to be processed in a NYC data warehouse in Azure Synapse Analytics. 

The two main goals are:

1.	To analyze how the City’s financial resources are allocated and how much of the City’s budget is being devoted to overtime.
2.	To make the data available to the interested public to show how the City’s budget is being spent on salary and overtime pay for all municipal employees.

## Schema
![db-schema](https://user-images.githubusercontent.com/124577508/225587548-27baf99b-f6d6-4b5b-91dd-bf9f825555e7.jpeg)


## Project Environment

For this project, we'll work in the Azure Portal, using several Azure resources including:

- Azure Data Lake Gen2 
- Azure SQL DB
- Azure Data Factory
- Azure Synapse Analytics

## Step 1: Preparing the Data Infrastructure

1.  Create an Azure data lake Storage Gen2 and upload the data in the corresponding directories

  -	Create an Azure Data Lake Storage Gen2(storage account) and associated storage container resource named adlsnycpayroll-ioanna-vitouni

  -	Create three directories in this storage container named: dirpayrollfiles, dirhistoryfiles, dirstaging

  -	Upload EmpMaster.csv, AgencyMaster.csv, TitleMaster.csv, nyc_payroll_2021.csv to the dirpayrollfiles folder

  -	Upload nycpayroll_2020.csv to the dirhistoryfilesfolder
  

![Capture](https://user-images.githubusercontent.com/124577508/225591676-7f78d3a3-c50d-4a5f-a46e-6fc079bb91d2.PNG)


2.  Create an Azure Data Factory Resource

3.  Create a SQL Database to store the current year of the payroll data

-	In the Azure portal, create a SQL Database resource named db_nycpayroll 
  and create a table called NYC_Payroll_Data with this SQL Script:
  

![11](https://user-images.githubusercontent.com/124577508/225595086-d73fd474-c6a8-4eb7-ad0c-d6579998bd97.PNG)

![Capture2](https://user-images.githubusercontent.com/124577508/225595364-d72535e7-dcef-4589-b484-80604005aea0.PNG)

4.  Create a Synapse Analytics workspace 

-	Create an Azure Data Lake Gen2 and file system for Synapse Analytics

-	Create a SQL dedicated pool in the Synapse Analytics workspace

-	Create master data tables and payroll transaction tables using these SQL scripts

![12](https://user-images.githubusercontent.com/124577508/225598782-05aac53c-3cee-489f-99f6-e5f7f78524c4.PNG)

![13](https://user-images.githubusercontent.com/124577508/225598812-f175292e-cab4-41e1-8a1b-9e14d91918fb.PNG)


![Capture1](https://user-images.githubusercontent.com/124577508/225598993-f2c282dd-6bca-4803-b730-799146b21883.PNG)

## Step 2: Create Linked Services

1.	Create a Linked Service in Azure Data Factory for Azure Data Lake that contains the data files

2.	Create a Linked Service to SQL Database that has the 2021 data

3.	Create a Linked Service for Synapse Analytics by creating a linked service to the SQL pool

![Capture3](https://user-images.githubusercontent.com/124577508/225600403-f05e3720-3cb4-461d-93e5-293a6ad2685a.PNG)\

## Step 3: Create the datasets in Azure Data factory


1.  Create the dataset for the nycpayroll_2021.csv file on Azure Data Lake Gen2

2.	Repeat the same process to create datasets for the rest of the data files: 
    EmpMaster.csv, TitleMaster.csv, AgencyMaster.csv
     
3.  Create the dataset for transaction data table that should contain 2021 data in SQL DB

4.  Create the datasets for destination tables in Synapse Analytics:

    -	Dataset for NYC_Payroll_EMP_MD
    -	Dataset for NYC_Payroll_TITLE_MD
    -	Dataset for NYC_Payroll_AGENCY_MD
    -	Dataset for NYC_Payroll_Data


![Capture4](https://user-images.githubusercontent.com/124577508/225602950-1e48548c-c23a-420d-86b6-f773a163a2e6.PNG)

![Capture5](https://user-images.githubusercontent.com/124577508/225602978-2fba1401-070d-4f9e-acd2-c1f64aecd8d9.PNG)

![Capture10](https://user-images.githubusercontent.com/124577508/225605488-fdfd87ea-6426-458a-90c6-929228005aa0.PNG)

![Capture6](https://user-images.githubusercontent.com/124577508/225603177-6a988914-49c5-415d-8e5e-e21bdaf1ca20.PNG)


## Step 4: Create Data Flows

1.	In Azure Data Factory, create the data flow to load 2021 Payroll Data to SQL DB transaction table

![Capture8](https://user-images.githubusercontent.com/124577508/225606899-9b1b2277-99b2-43b8-bdcd-0fe92738d0a3.PNG)


2.	Create a Pipeline to load 2021 Payroll data into transaction table in SQL DB

![Capture9](https://user-images.githubusercontent.com/124577508/225633256-c50048b3-90f8-4392-affa-a91d0523ca8e.PNG)


3.	Create data flows to load the data from the data lake files into the Synapse Analytics data tables

![Capture7](https://user-images.githubusercontent.com/124577508/225629300-879dd08f-fe94-41a9-b1b2-be2405c89bb9.PNG)

![Capture7(1)](https://user-images.githubusercontent.com/124577508/225629320-fd776dc9-ce72-4e2a-9214-d49ab2114594.PNG)

![Capture7(2)](https://user-images.githubusercontent.com/124577508/225629347-3905c45b-6e0a-427b-b79a-50368f5a5c84.PNG)

4.	Create a data flow to load 2021 data from SQL DB to Synapse Analytics

![Capture7(3)](https://user-images.githubusercontent.com/124577508/225629380-16fb98d9-4afb-42b3-857e-5de6ad1bc640.PNG)


5.	Create a master pipeline which contains all the data flows we created for Employee, Title, Agency, and year 2021 Payroll transaction data to Synapse Analytics

![Capture11](https://user-images.githubusercontent.com/124577508/225631582-6254821d-56ef-4094-b91e-a23acc3d0930.PNG)


6.	Trigger and monitor the Pipelines

![Capture12](https://user-images.githubusercontent.com/124577508/225631315-86c51299-a266-4743-8822-4983364c0d45.PNG)

## Step 5: Data Aggregation and Parameterization

1.	Create a Summary table in Synapse with the following SQL script and create a dataset named table_synapse_nycpayroll_summary:

![Capture18](https://user-images.githubusercontent.com/124577508/225640138-7e66142f-0876-44e1-8210-bfd6b009231d.PNG)


![Capture13](https://user-images.githubusercontent.com/124577508/225640179-1a73f663-74af-44ee-8106-2fc9e5e19004.PNG)

2.	Create a new dataset for the Azure Data Lake Gen2 folder that contains the dirhistoryfiles

![Capture15](https://user-images.githubusercontent.com/124577508/225640283-64a6fa3b-c07d-4a23-acac-366ca645920b.PNG)

3.	Create new data flow and name it Dataflow Aggregate Data
4.	Add a new Union activity in the data flow with the history files and a Filter activity after Union with the expression:
![Capture19](https://user-images.githubusercontent.com/124577508/225640396-22a43452-035b-46e9-8a12-00eabe0140fa.PNG)

5.	Derive a new TotalPaid column with the expression:

![Capture20](https://user-images.githubusercontent.com/124577508/225640453-195f658e-e333-4903-92b1-856d7eb8f37d.PNG)

6.	Add an Aggregate activity to the data flow next to the TotalPaid activity and group by AgencyName and Fiscal Year
7.	Add a Sink activity to the Data Flow for the Synapse Analytics Payroll Summary Table

![Capture17](https://user-images.githubusercontent.com/124577508/225640502-2b97f0e8-157d-4b21-8b3e-903cd17ba27b.PNG)

8.	Create a new Pipeline for the Dataflow Aggregate Data
9.	Trigger and monitor the pipeline


![Capture16](https://user-images.githubusercontent.com/124577508/225640549-40dd46ef-b316-4faf-8bb3-27b1dc730ccb.PNG)



