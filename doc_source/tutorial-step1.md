# Load and verify data<a name="tutorial-step1"></a>

The data you query in this tutorial is a sample set of AWS IoT SiteWise data that models wind engine turbines in a wind farm\. 

**Note**  
You will query three tables in your data store throughout this tutorial:  
`raw` ‐ Contains raw, unprocessed data for each asset\.
`asset_metadata` ‐ Contains general information about each asset\.
`asset_hierarchy_metadata` ‐ Contains information about the relationships between assets\.

**To run the SQL queries in this tutorial**

1. Follow the steps in [Create a dataset with AWS IoT SiteWise data \(Console\)](create-dataset-itsw-console.md) or [Create a dataset with AWS IoT SiteWise data \(AWS CLI\)](create-dataset-itsw-cli.md) to create an AWS IoT Analytics dataset for your AWS IoT SiteWise data\.

1. To update your dataset query throughout this tutorial, do the following\.

   1. In the AWS IoT Analytics console, on the **Datasets** page, choose the name of the dataset you created on the previous page\. 

   1. On the dataset summary page, choose **Edit** to edit your SQL query\.

   1. To display the results in a table following the query, choose **Test query**\.

   Alternatively, you can run the following `update-dataset` command to modify the SQL query with the AWS CLI\.

   ```
   aws iotanalytics update-dataset --cli-input-json file://update-query.json
   ```

    Contents of `update-query.json`:

   ```
   {
       "datasetName": "my_dataset",
       "actions": [
           {
               "actionName": "myDatasetUpdateAction",
               "queryAction": {
                   "sqlQuery": "SELECT * FROM my_iotsitewise_datastore.asset_metadata LIMIT 3"
               }
           }
       ]
   }
   ```

1. In the AWS IoT Analytics console or with the AWS CLI, run the following query on your data to verify that your `asset_metadata` table loaded successfully\.

   ```
   SELECT COUNT(*) FROM my_iotsitewise_datastore.asset_metadata
   ```

   Similarly, you can verify that your `asset_hierarchy_metadata` and `raw` tables aren't empty\.

## Next Step<a name="tutorial-step1.2"></a>

 [Data exploration ](tutorial-step2.md) 