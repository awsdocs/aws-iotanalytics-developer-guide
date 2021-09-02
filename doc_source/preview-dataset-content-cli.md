# Access dataset content in AWS IoT Analytics \(AWS CLI\)<a name="preview-dataset-content-cli"></a>

If your dataset contains any data, you can preview and download your SQL query results\.

The examples shown here use the AWS Command Line Interface \(AWS CLI\)\. For more information on the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\. For more information about the CLI commands available for AWS IoT Analytics, see [iotanalytics](https://docs.aws.amazon.com/cli/latest/reference/iotanalytics/index.html) in the *AWS Command Line Interface Reference*\. 

**To access your AWS IoT Analytics dataset results \(AWS CLI\)**

1. Run the following `get-dataset-content` command to view the result of your query\.

   ```
   aws iotanalytics get-dataset-content --dataset-name my_iotsitewise_dataset
   ```

1. If your dataset contains any data, then the output from `get-dataset-content`, has `"state": "SUCCEEDED"` in the `status` field, such as in the following example\.

   ```
   {
       "timestamp": 1508189965.746,
       "entries": [
           {
             "entryName": "my_entry_name",
             "dataURI": "https://aws-iot-analytics-datasets-f7253800-859a-472c-aa33-e23998b31261.s3.amazonaws.com/results/f881f855-c873-49ce-abd9-b50e9611b71f.csv?X-Amz-"
             
           }
       ],
       "status": {
         "state": "SUCCEEDED",
         "reason": "A useful comment."
       }
   }
   ```

1. The output from `get-dataset-content` includes a `dataURI`, which is a signed URL to the output results\. It is valid for a short period of time \(a few hours\)\. Visit the `dataURI` URL to access your SQL query results\.
**Note**  
 Depending on your workflow, you might want to always call `get-dataset-content` before you access the content because calling this command generates a new signed URL\.