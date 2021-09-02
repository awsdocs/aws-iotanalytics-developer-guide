# Create an AWS IoT Analytics dataset with AWS IoT SiteWise data<a name="create-dataset-mls"></a>

An AWS IoT Analytics dataset contains SQL statements and expressions that you use to query data in your data store along with an optional schedule that repeats the query at a day and time that you specify\. You can use expressions similar to [Amazon CloudWatch schedule expressions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) to create the optional schedules\.

**Note**  
 A dataset is typically a collection of data that might or might not be organized in tabular form\. In contrast, AWS IoT Analytics creates your dataset by applying a SQL query to data in your data store\. 

Follow these steps to get started with creating a dataset for your AWS IoT SiteWise data\.

**Topics**
+ [Create a dataset with AWS IoT SiteWise data \(Console\)](create-dataset-itsw-console.md)
+ [Create a dataset with AWS IoT SiteWise data \(AWS CLI\)](create-dataset-itsw-cli.md)