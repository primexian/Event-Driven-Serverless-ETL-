Overview
In this project, we've implemented an automated data consolidation process for toll plaza transactions, utilizing various AWS services. The aim is to process data as soon as it lands in the S3 bucket's landing zone, ensuring the most up-to-date information is presented to customers.

Technologies Used

AWS Lambda: Serverless computing for event-driven data processing.
Amazon S3: Scalable object storage for landing and staging data.
AWS Glue: Fully managed ETL service for data categorization, cleaning, and enrichment.
Amazon Redshift: Fully managed, scalable cloud data warehouse for fast analytics.

Architecture


![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/d421d14f-93b6-4fe6-afe1-5de2be45fa62)

Data Ingestion

AWS Glue Data Catalog: Used to catalog data from multiple sources, providing tables for ETL jobs.
AWS Glue Crawlers: Populate the Data Catalog by crawling structured or unstructured data in the S3 landing zone.
AWS Glue Jobs: Defined ETL jobs that use Data Catalog tables as both source and target. The script connects with source data to process and writes to the S3 bucket.

Serverless Workflow

S3 Event Notification: Triggered on a PUT event in the landing zone.
AWS Lambda Function: Receives the S3 event notification and initiates the AWS Glue workflow.
AWS Glue Workflow: Orchestrates multiple crawlers, jobs, and triggers to process the data.

Data Processing and Storage

Amazon Redshift: ETL jobs issue COPY statements against Redshift for efficient data transfer.
S3 Staging Bucket: Receives data from AWS Glue jobs before loading it into Redshift.
Data API and Query Editor: Users can query the Redshift data warehouse using SQL or the built-in query editor for real-time insights.

Implementation Steps
1. Setting Up S3 Buckets
Landing Bucket: Receives data from the toll plaza application. Event notification triggers Lambda function, initiating the Glue workflow.
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/722254c5-1a61-43e8-9c8f-e3e8d9e3b315)

![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/05c7ffb3-8ba1-4da5-a0d3-6bd3a53a9c03)
		In the landing bucket we create event notification with suffix of .json file and PUT action in AWS
		
		
		
Staging Bucket: Receives data from AWS Glue jobs.
Setting up redshift 
	Implement Query editor on Amazon Redshift cluster
	Effective to run query on database hosted by Amazon Redshift Cluster immediately 
		In the query editor - connect database, select clusters, database name, and database user - admin by default 


![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/e8a095b6-df9e-4780-b0c8-07513bf91d32)
