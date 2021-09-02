# Create a dataset with AWS IoT SiteWise data \(Console\)<a name="create-dataset-itsw-console"></a>

Use these steps to create a dataset in the AWS IoT Analytics console for your AWS IoT SiteWise data\.

**To create a dataset**

1. In the [https://console\.aws\.amazon\.com/iotanalytics/](https://console.aws.amazon.com/iotanalytics/), on the **left navigation pane**, choose **Datasets**\.

1. On the **Create dataset** page, choose **Create SQL**\.

1. On the **Specify dataset details** page, specify the details of your dataset\.

   1. Enter a name for your dataset\.

   1. For **Data store source**, choose the unique ID that identifies your AWS IoT SiteWise data store\.

   1. \(Optional\) For **Tags**, add one or more custom tags \(key\-value pairs\) to your dataset\.

1. Use SQL expressions to query your data and answer analytical questions\.

   1. In the **Author query** field, enter a SQL query that uses a wildcard to display up to five rows of data\.

      ```
      SELECT * FROM my_iotsitewise_datastore.asset_metadata LIMIT 5
      ```

      For more information about supported SQL functionality in AWS IoT Analytics, see [SQL expressions in AWS IoT Analytics](sql-support.md)\. Or, see [Tutorial: Query AWS IoT SiteWise data in AWS IoT Analytics ](tutorial-query-mls.md) for examples of statistical queries that can provide insight to your data\.

   1. You can choose **Test query** to validate that your input is correct, and to display the results in a table following the query\.
**Note**  
Because Amazon Athena [limits the maximum number of running queries](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html), you should limit your SQL query to a reasonable size so that it does not run for an extended period\. 

1. \(Optional\) When you create dataset contents using data from a specified time frame, some data might not arrive in time for processing\. To allow for a delay, you can specify an offset, or delta\. For more information, see [Getting late data notifications through Amazon CloudWatch Events](late-data-notification.md)\.

   After configuring a data selection filter on the **Configure data selection filter** page, choose **Next**\.

1. \(Optional\) On the **Set query schedule page**, you can schedule this query to run regularly to refresh the dataset\. Dataset schedules can be created and edited at any time\. 
**Note**  
Data from AWS IoT SiteWise ingests into AWS IoT Analytics every six hours\. We recommend selecting a frequency that is six hours or more\.

   Choose and option for **Frequency** and then choose **Next**\.

1. AWS IoT Analytics will create versions of this dataset content and store your analytics results for the specified period\. We recommend 90 days, however you can opt to set your custom retention policy\. You may also limit the number of stored versions of your dataset content\.

   After selecting your options on the **Configure the results of your dataset** page, choose **Next**\.

1. \(Optional\) You can configure the delivery rules of your dataset results to a specific destination, such as AWS IoT Events\. 

   After selecting your options on the **Configure dataset content delivery rules** page, choose **Next**\.

1. Review your choices and then choose **Create dataset**\.

1. Verify that your new dataset appears on the **Datasets** page\.