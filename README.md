# Daily Flight Data Ingestion
# Data pipeline for ingesting daily flights data and load it into Redshift.

# Prerequisites
* A good understanding of AWS Services: S3, Redshift, Glue, AWS Step functions, EventBridge.
* Good understanding of Python, and SQL.
* Knowledge of AWS security best practices, including IAM (Identity and Access Management) roles and policies.

# Project Motivation
### This project aims to leverage AWS services to process large amount of daily flight data and leverages a redshift data warehouse which can then be used by end users for comprehensive analysis and reporting.

# Architecture Diagram
![Architecture Diagram](./Architecture_Diagram/ETL-pipeline-to-load-daily-flights-data-into-Redshift.png?raw=true)

# AWS Glue Visual ETL Diagram
   ![ETL Pipeline](./Images_Watermarked/23%20Whole%20Pipeline.png?raw=true)
   
# AWS Step function Diagram
   ![AWS Step function](./Images_Watermarked/34%20Overall%20Step%20Function%20Diagram.png?raw=true)

# Architecture Diagram Steps
1. The pipeline starts with an S3 bucket where the daily flight data files are stored or ingested.
2. EventBridge Rule monitors the S3 bucket for new data arrivals and triggers the step function when a new file is detected.
3. Step Functions orchestrates and coordinates the subsequent steps in the pipeline, providing a serverless workflow management.
4.  The Glue Crawler service crawls the data in S3, extracts metadata, and infers the schema, preparing the data for further processing.
5. An Apache Spark Glue ETL job then performs data transformations, cleansing, and preparation tasks on the flight data to ensure it's in the desired format for loading into the data warehouse.
6.  Based on the outcome of the Glue Job, an email notification is sent to the appropriate SNS topic for either success or failure, informing stakeholders about the status of the data ingestion process.

## Steps and Descriptions to build the pipeline
1. **Creating S3 Bucket**
   - Initial setup of an S3 bucket to store data resources.
   ![Create S3 Bucket](./Images_Watermarked/1%20creating%20s3%20bucket.png?raw=true)

2. **Creating Folders for Data Storage**
   - Setting up folders in S3 to store dimensions and daily flights data for arrival and departure of flights.
   ![Create Folders in S3](./Images_Watermarked/2%20creating%20folders%20to%20store%20the%20dimensions%20and%20daily%20data%20of%20arrival%20and%20departure%20of%20flights.png?raw=true)

3. **Viewing Data Inside Dimension Folder**
   - Overview of data stored inside the dimension folder in S3.
   ![Data Inside Dimension Folder](./Images_Watermarked/3%20data%20inside%20dim.png?raw=true)

4. **Starting the Redshift Cluster**
   - Initiating the AWS Redshift cluster to handle and analyze data.
   ![Start Redshift Cluster](./Images_Watermarked/4%20Starting%20the%20redshift%20cluster.png?raw=true)

5. **Creating Redshift Connection in AWS Glue**
   - Establishing a connection to AWS Redshift from AWS Glue.
   ![Create Redshift Connection in Glue](./Images_Watermarked/5%20Create%20Redshift%20connection%20inside%20glue.png?raw=true)

6. **Successful Redshift Connection**
   - Confirmation of a successful connection setup to AWS Redshift.
   ![Redshift Connection Successful](./Images_Watermarked/6%20connection%20to%20redshift%20successful.png?raw=true)

7. **Creating Glue Metadata Database**
   - Setting up a metadata database in AWS Glue.
   ![Create Glue Metadata Database](./Images_Watermarked/7%20Creating%20glue%20metadata%20database.png?raw=true)

8. **Creating Tables in Redshift**
   - Executing commands to create tables in the AWS Redshift database.
   ![Create Tables in Redshift](./Images_Watermarked/8%20Commands%20to%20create%20tables%20in%20redshift.png?raw=true)

9. **Glue Crawler for Redshift Metadata**
   - Configuring a Glue crawler to extract metadata from the dimensions table in Redshift.
   ![Setup Glue Crawler for Redshift Metadata](./Images_Watermarked/9%20glue%20crawler%20to%20crawl%20the%20metadata%20of%20the%20dimensions%20table%20in%20redshift.png?raw=true)

10. **Glue Crawler for Flights Fact Table**
    - Setting up a Glue crawler for the flights fact table to organize and access flight data efficiently.
    ![Setup Glue Crawler for Flights Fact Table](./Images_Watermarked/10%20glue%20crawler%20for%20the%20flights%20fact%20table.png?raw=true)

11. **Glue Crawler for Daily Flight Data**
    - Configuring a Glue crawler to scan and index daily flight data stored in S3.
    ![Setup Glue Crawler for Daily Flight Data](./Images_Watermarked/11%20Glue%20crawler%20for%20scanning%20the%20daily%20flight%20data.png?raw=true)

12. **Sample Data in S3 Bucket**
    - Uploading sample data in S3 bucket for metadata inference by the Glue crawler. We can delete this as soon as we run the initial scanner.
    ![Upload Sample Data to S3](./Images_Watermarked/12%20putting%20sample%20data%20in%20our%20s3%20bucket%20so%20that%20metadata%20can%20be%20inferred%20by%20the%20glue%20crawler.png?raw=true)

13. **Crawler Run**
    - Glue Crawler triggered due to file upload into s3 bucket and ran successfully.
    ![Crawler Run Successful](./Images_Watermarked/13%20Crawler%20run%20successful.png?raw=true)

14. **Metadata Table Created Successfully**
    - Metadata tables created by the crawler based on the CSV data in the s3 bucket.
    ![Metadata Table Creation Successful](./Images_Watermarked/14%20metadata%20table%20created%20successfully.png?raw=true)

15. **Crawlers Picked Metadata from Both Redshift Tables and S3 Data Files**
    - AWS Glue crawlers successfully extracted metadata from both Redshift tables and S3 data files.
    ![Crawlers Metadata Extraction](./Images_Watermarked/15%20Crawlers%20picked%20metadata%20in%20both%20redshift%20tables%20and%20s3%20data%20files.png?raw=true)

16. **Glue job to filter only Flights delayed by 60 Minutes or More When Departing**
    - Applying filters in the data query to show only flights that are delayed by 60 minutes or more upon departure.
    ![Filter Flights by Delay](./Images_Watermarked/16%20Filter%20to%20show%20only%20flights%20which%20are%20late%20by%2060%20minutes%20or%20more%20when%20departing.png?raw=true)

17. **Adding Airport Dimension Data Catalog**
    - Adding a data catalog for airport dimensions in Glue Visual ETL.
    ![Add Airport Dimension Data Catalog](./Images_Watermarked/17%20Creating%20airport%20dimesnision%20daya%20catalog.png?raw=true)

18. **Join Operation on Flight Data**
    - Performing a join operation between the filtered flights data and the airports CSV file to enrich flight information.
    ![Join Operation on Flight Data](./Images_Watermarked/18%20Performing%20Join%20operation%20for%20joining%20the%20filter%20and%20airports_csv%20file%20table.png?raw=true)

19. **Schema Adjustment After Join**
    - Modifying the schema of the joined data to match the pre-existing Redshift table for consistent data integration.
    ![Schema Adjustment After Join](./Images_Watermarked/19%20Performing%20the%20join%20and%20changing%20the%20schema%20to%20match%20the%20redshift%20table%20that%20we%20created%20before.png?raw=true)

20. **Second Join Operation for Destination Details**
    - Executing a second join operation to integrate destination details into the flight data.
    ![Second Join Operation](./Images_Watermarked/20%20Performing%202nd%20join%20operation%20for%20joining%20the%20destination%20details.png?raw=true)

21. **Adjusting Schema to Match Destination Table**
    - Aligning the schema of the processed data to fit the Redshift destination table specifications.
    ![Schema Adjustment for Destination Table](./Images_Watermarked/21%20Changing%20the%20schema%20to%20match%20the%20redshift%20destination%20table.png?raw=true)

22. **Loading data into Redshift target table**
    - Node to load the transformed data into Redshift.
    ![Redshift Target Table](./Images_Watermarked/22%20Redshift%20target%20table.png?raw=true)

23. **Overview of the Complete Glue ETL Pipeline**
    - Visual representation of the entire data processing pipeline, from data ingestion to storage.
    ![Complete Pipeline Overview](./Images_Watermarked/23%20Whole%20Pipeline.png?raw=true)

24. **Enabling Job Bookmarking for Incremental Ingestion**
    - Setting job bookmarking in AWS Glue to enable incremental ingestion of data with adjustments to worker settings for efficiency.
    ![Enable Job Bookmarking](./Images_Watermarked/24%20enable%20job%20bookmarking%20for%20incremental%20ingestion%20and%20setting%20workers%20to%202%20for%20incremental%20data%20ingestion.png?raw=true)

25. **Enabling EventBridge Notifications for S3 Bucket**
    - Setting up notifications via AWS EventBridge for the source S3 bucket to trigger the step function based on data updates in the s3 bucket through eventbridge.
    ![Enable EventBridge Notifications](./Images_Watermarked/25%20Enable%20eventbridge%20notifications%20for%20the%20source%20bucket.png?raw=true)

26. **Adding Crawler to Step Function**
    - Incorporating a Glue crawler into the AWS Step Function to automate part of the data processing workflow.
    ![Add Crawler to Step Function](./Images_Watermarked/26%20Adding%20crawler%20to%20the%20step%20function.png?raw=true)

27. **Checking Crawler State**
    - Utilizing a 'Get Crawler' function to check the operational state of the AWS Glue crawler.
    ![Check Crawler State](./Images_Watermarked/27%20Get%20Crawler%20used%20to%20check%20the%20state%20of%20the%20crawler.png?raw=true)

28. **Conditional Check for Crawler State**
    - Implementing a conditional check within the step function to handle different crawler states.
    ![Conditional Check for Crawler](./Images_Watermarked/28%20Add%20the%20if%20condition%20for%20the%20running%20state.png?raw=true)

29. **Waiting and Re-checking Crawler Status**
    - Setting a wait condition in the step function before re-checking the crawler status to ensure data readiness.
    ![Wait and Re-check Crawler Status](./Images_Watermarked/29%20Wait%20for%20a%20specified%20time%20and%20re-check%20the%20status.png?raw=true)

30. **Successful Glue Crawler Run**
    - Proceeding to the next steps once glue crawler run is successful.
    ![Assume Crawler Success](./Images_Watermarked/30%20Assuming%20that%20it%20succeeded%20in%20the%20default%20case%20and%20starting%20the%20Glue%20ETL%20Job.png?raw=true)

31. **Handling Task Failure Notifications**
    - Implementing AWS SNS to send notifications in case of task failures during the pipeline execution.
    ![Task Failure Notification Setup](./Images_Watermarked/31%20Incase%20task%20fails%20send%20notification%20using%20sns.png?raw=true)

32. **Sending Success Notifications**
    - Sending success notifications via AWS SNS upon successful completion of the pipeline processes.
    ![Success Notification Sent](./Images_Watermarked/31%20Notifications%20sending%20incase%20of%20failures.png?raw=true)

33. **Sending Failed Notification**
    - Sending failure notifications via AWS SNS if the data pipeline fails.
    ![Success Notification](./Images_Watermarked/33%20success%20notification%20sent%20.png?raw=true)

34. **Complete Step Function Workflow**
    - Displaying the AWS Step Function, detailing the complete process flow and integrations.
    ![Step Function Diagram](./Images_Watermarked/34%20Overall%20Step%20Function%20Diagram.png?raw=true)

35. **Creating an EventBridge Rule**
    - Setting up an AWS EventBridge rule to manage events based on specific triggers within the pipeline.
    ![Create EventBridge Rule](./Images_Watermarked/35%20Creating%20an%20Eventbridge%20rule.png?raw=true)

36. **Building EventBridge Event Pattern**
    - Constructing an event pattern in AWS EventBridge to filter and respond to S3 Object Create events effectively.
    ![EventBridge Event Pattern](./Images_Watermarked/36%20Eventbridge%20build%20event%20pattern.png?raw=true)

37. **EventBridge Built Event**
    - Overview of a built event in AWS EventBridge, showcasing the configured event responses.
    ![EventBridge Built Event](./Images_Watermarked/37%20Eventbridge%20built%20event%20.png?raw=true)

38. **Editing the Event Pattern for CSV Files**
    - Modifying the event pattern in AWS EventBridge to specifically match CSV file patterns, enhancing targeted event handling.
    ![Edit Event Pattern for CSV Files](./Images_Watermarked/38%20Editing%20the%20event%20pattern%20to%20only%20match%20the%20csv%20file%20patterns.png?raw=true)

39. **Adding Target to EventBridge Rule to trigger AWS Step functions**
    - Adding a target to the AWS EventBridge rule to direct the event response actions.
    ![Add Target to EventBridge](./Images_Watermarked/39%20Adding%20the%20target.png?raw=true)

40. **Creating EventBridge Rule for S3 Notifications**
    - Establishing an EventBridge rule to listen for S3 create notifications and trigger the state machine.
    ![Create EventBridge Rule for S3](./Images_Watermarked/40%20Creating%20the%20eventbridge%20rule%20which%20will%20look%20for%20s3%20create%20notifications%20and%20trigger%20state%20machine.png?raw=true)

41. **Uploading a File to S3 Bucket**
    - Uploading a new file to the S3 bucket to initiate the automated data handling and analytics pipeline.
    ![Upload File to S3](./Images_Watermarked/41%20Upload%20a%20new%20file%20to%20the%20s3%20bucket.png?raw=true)

42. **Step Function Triggered**
    - AWS Step Function triggered in response to new data being uploaded to S3.
    ![Step Function Started](./Images_Watermarked/42%20step%20function%20started%20.png?raw=true)

43. **Graph View of Step Function showing that glue crawler step is running**
    - Displaying the graph view of the AWS Step Function, illustrating the sequence of operations and decision points.
    ![Graph View of Step Function](./Images_Watermarked/43%20Graph%20view%20step%20function.png?raw=true)

44. **Glue Crawler Running**
    - Showing the AWS Glue crawler in operation as it processes data inputs.
    ![Crawler Running](./Images_Watermarked/44%20Crawler%20Running.png?raw=true)

45. **Glue Job Started**
    - Initiating an AWS Glue job to transform and load data according to predefined logic and parameters.
    ![Glue Job Started](./Images_Watermarked/45%20Glue%20Job%20Started%20Running.png?raw=true)

46. **Step Function Execution Successful**
    - Confirming the successful completion of the AWS Step Function execution, marking the end of the automated process.
    ![Step Function Successful](./Images_Watermarked/46%20Step%20Function%20Execution%20Successful.png?raw=true)

47. **Step Function Success Notification**
    - Notification Sent confirming the successful execution of the step function.
    ![Success Notification Email](./Images_Watermarked/47%20Step%20function%20success%20notification%20sent%20to%20email.png?raw=true)

48. **Data Successfully Ingested into Redshift**
    - Verifying that data has been successfully ingested into the AWS Redshift data warehouse, ready for analysis.
    ![Data Ingested into Redshift](./Images_Watermarked/48%20Data%20Successfully%20ingested%20into%20the%20redshfit%20data%20warehouse.png?raw=true)

49. **Analytics on Delayed Flights**
    - Displaying analytics results on count of flights delayed by at least one hour, demonstrating the data processing and analysis capabilities.
    ![Analytics on Delayed Flights](./Images_Watermarked/49%20Number%20of%20flights%20delayed%20by%20atleast%201hr.png?raw=true)

# Potential Next Steps
1. **_AWS SageMaker Integration_**: We can Expand the analytics capabilities of the pipeline by integrating with AWS SageMaker. This will allow us to implement machine learning models directly into the pipeline, enabling predictive analytics and more complex data analysis tasks. For example, using machine learning to predict flight delays based on historical data and external factors such as weather conditions or airport traffic.
2. **_Pipeline Performance Tuning_**: Regularly review the performance of the AWS Glue jobs and Redshift queries. Adjust configurations such as DPUs in Glue and query optimization in Redshift to enhance the performance and reduce execution times. 

