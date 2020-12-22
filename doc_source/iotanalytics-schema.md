# File formats<a name="iotanalytics-schema"></a>

AWS IoT Analytics data stores currently support JSON and Parquet file formats\. The default file format is JSON\.
+ [JSON \(JavaScript Object Notation\)](https://www.json.org/json-en.html) \- A text format that supports name\-value pairs and ordered lists of values\.
+ [Apache Parquet](https://parquet.apache.org/documentation/latest/) \- A columnar storage format used to efficiently store and query large volumes of data\.

To configure the file format of the AWS IoT Analytics data store, you can use the `FileFormatConfiguration` object when you create the data store\.

`fileFormatConfiguration`  
Contains the configuration information of file formats\. AWS IoT Analytics data stores support JSON and Parquet\.  
The default file format is JSON\. You can specify only one format\. You can't change the file format after you create the data store\.    
`jsonConfiguration`  
Contains the configuration information of the JSON format\.  
`parquetConfiguration`  
Contains the configuration information of the Parquet format\.    
`schemaDefinition`  
Information needed to define a schema\.    
`columns`  
Specifies one or more columns that store your data\.  
Each schema can have up to 100 columns\. Each column can have up to 100 nested types\.    
`name`  
The name of the column\.  
Length constraints: 1\-255 chars\.  
`type`  
The type of data\. For more information about the supported data type, see [Common data types](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-common.html) in the *AWS Glue Developer Guide*\.  
Length constraints: 1\-131072 characters\.

AWS IoT Analytics supports all data types listed on the [Data Types in Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/data-types.html) page, except for `DECIMAL(precision, scale)` \- `precision`\.

## Create a data store \(console\)<a name="create-datastore-console"></a>

The following shows you how to create a data store that saves data in Parquet format\.

1. Sign in to the [AWS IoT Analytics console](https://console.aws.amazon.com/iotanalytics/)\.

1. In the navigation pane, choose **Data stores**\.

1. Choose **Create**\.

1. On the **Set ID and tags** page, enter a unique data store ID, and then choose **Next**\.

1. On the **Set storage type** page, choose storage type, and then choose **Next**\. This tutorial uses **AWS managed S3 bucket**\.

1. On the **Set data format** page, choose **Parquet**, and then choose **Add column**\.

1. On the **Add column to Parquet schema** page, enter a column name and choose the data type, and then choose **Add column**\.
**Note**  
You can add up to 100 columns\.
You can add columns to the existing schema by using the [UpdateDatastore](https://docs.aws.amazon.com/iotanalytics/latest/APIReference/API_UpdateDatastore.html) API\.
You can't edit the existing schema in the console after you create the data store\.

1. Choose **Create**\.

   You can't change the file format after you create the data store\.