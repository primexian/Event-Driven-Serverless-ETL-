Overview
In this project, we've implemented an automated data consolidation process for toll plaza transactions, utilizing various AWS services. The aim is to process data as soon as it lands in the S3 bucket's landing zone, ensuring the most up-to-date information is presented to customers.

Technologies Used

AWS Lambda: Serverless computing for event-driven data processing.

Amazon S3: Scalable object storage for landing and staging data.

AWS Glue: Fully managed ETL service for data categorization, cleaning, and enrichment.

Amazon Redshift: Fully managed, scalable cloud data warehouse for fast analytics.

Architecture
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/4a9b10a9-f21a-436a-8dbe-0a807440a101)



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
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/5573aa17-5bb2-4e98-94e9-d82a71fa06e2)



In the landing bucket we create event notification with suffix of .json file and PUT action in AWS
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/dfa62cfd-13c3-43a7-904c-d33731d0a218)
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/8b6e9925-7bac-4da6-9e57-0182422eac6b)
		
		
		
Staging Bucket: Receives data from AWS Glue jobs.
Setting up redshift 
	Implement Query editor on Amazon Redshift cluster
	Effective to run query on database hosted by Amazon Redshift Cluster immediately 
		In the query editor - connect database, select clusters, database name, and database user - admin by default 
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/1a784bce-59a8-4c96-803c-81b49bedaa3c)
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/92124569-9848-4ded-a4d7-a455db3cda47)

2. AWS Glue Configuration
Data Connection: Configured in AWS Glue to connect with Redshift, specifying database instance, clusters, and credentials.
Crawler Configuration: Crawlers classify data, determine format/schema, and write metadata to the Data Catalog.
We have to select our VPC for data source, subnets within your VPC, and security groups will be the redshift security Group for data access.
Data source will be JDBC and connect will be redshift that we implemented earlier.
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/2f936214-a8f4-468e-b60d-431e9cd3d5ee)
Once the crawler is created: 
	The crawler will classifies data to determine the format schema, and associated properties of the raw data
	Group the data into tables or partitions 
	Write metadata to data catalog, ETL job reads from and writes to the data store from the landing bucket to staging bucket 
	In the crawler we have to add the data source which is the JDBC 

3. Visual ETL Job
AWS Glue Job: Created visual ETL jobs to process data from the landing zone to the staging area.
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/39bc77a0-4155-4f6e-b0a2-92d1a6cd5402)
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/48da85e5-a15d-4ad4-b32b-57038a981248)

4. Monitoring and Testing
Interactive Session: Set up monitoring and testing with AWS Glue interactive session to rapidly build, test, and run data preparation and analytics applications.


Workflow
S3 Event Notification: PUT event triggers Lambda function.
Lambda Function: Reads and starts the Glue workflow.
AWS Glue Workflow: Executes crawlers and ETL jobs.
S3 Buckets: Landing zone and staging bucket show the workflow progress.
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/afce8ebc-2963-4e3c-971f-11ac49bc66af)

When we go to the S3 buckets for landing zone, we can see the jobs being created with event type, type of files accepted, functions triggered with the PutApi and workflow in the AWS Glue job!
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/0708a565-ff1c-46d1-8bb3-9cffb9909386)

The lambda function to read and start the workflow is listed below:
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/aae0b39e-d5c4-45fa-ba93-154b277563c7)
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/a5816a03-56e7-470f-8a65-522fd74ecb60)
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/8a00e483-e9d2-4b69-a06e-cca49c63deee)
![image](https://github.com/primexian/Event-Driven-Serverless-ETL-/assets/52623198/7dcf476d-21a0-4808-92fc-523f5286c1b9)

Conclusion

This project demonstrates an effective and scalable solution for toll plaza data consolidation using AWS services. The serverless architecture ensures real-time processing, providing customers with the most up-to-date information. The combination of AWS Lambda, S3, Glue, and Redshift delivers a reliable and efficient data pipeline for analytics and reporting.





