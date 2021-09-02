# Create a dataset with AWS IoT SiteWise data \(AWS CLI\)<a name="create-dataset-itsw-cli"></a>

Run the following AWS CLI commands to get started querying your AWS IoT SiteWise data\.

The examples shown here use the AWS Command Line Interface \(AWS CLI\)\. For more information on the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\. For more information about the CLI commands available for AWS IoT Analytics, see [iotanalytics](https://docs.aws.amazon.com/cli/latest/reference/iotanalytics/index.html) in the *AWS Command Line Interface Reference*\. 

**To create a dataset**

1. Run the following `create-dataset` command to create a dataset\.

   ```
   aws iotanalytics create-dataset --cli-input-json file://my_dataset.json
   ```

   Where the `my_dataset.json` file contains the following content\.

   ```
   {
       "datasetName": "my_dataset",
       "actions": [
           {
               "actionName":"my_action",
               "queryAction": {
                   "sqlQuery": "SELECT * FROM my_iotsitewise_datastore.asset_metadata LIMIT 5"
               }
           }
       ]
   }
   ```

   For more information about supported SQL functionality in AWS IoT Analytics, see [SQL expressions in AWS IoT Analytics](sql-support.md)\. Or, see [Tutorial: Query AWS IoT SiteWise data in AWS IoT Analytics ](tutorial-query-mls.md) for examples of statistical queries that can provide insight to your data\.

1. Run the following `create-dataset-content` command to create your dataset content by running your query\.

   ```
   aws iotanalytics create-dataset-content --dataset-name my_dataset
   ```