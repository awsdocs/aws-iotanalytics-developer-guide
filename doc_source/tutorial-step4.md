# Cleaning up your tutorial resources<a name="tutorial-step4"></a>

After you complete the tutorial, clean up your resources to avoid incurring charges\.

**To delete your AWS IoT SiteWise demo**

The AWS IoT SiteWise demo deletes itself after a week\. If you're done using the demo resources, you can delete the demo earlier\. To delete the demo manually, use the following steps\.

1. Navigate to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **IoTSiteWiseDemoAssets** from the list of **Stacks**\.

1. Choose **Delete**\. When you delete the stack, all of the resources created for the demo are deleted\.

1. In the confirmation dialog, enter **Delete**\.

   The stack takes around 15 minutes to delete\. If the demo fails to delete, choose **Delete** in the upper\-right corner again\. If the demo fails to delete again, follow the steps in the AWS CloudFormation console to skip the resources that failed to delete, and try again\.

**To delete your data store**
+ To delete your managed data store, run the CLI command `delete-datastore`, such as in the following example\.

  ```
  aws iotanalytics delete-datastore --datastore-name my_IotSiteWise_datastore
  ```

**To delete your AWS IoT Analytics dataset**
+ To delete your dataset, run the CLI command `delete-dataset`, such as in the following example\. You don't have to delete the content of the dataset before you perform this operation\.

  ```
  aws iotanalytics delete-dataset --dataset-name my_dataset
  ```
**Note**  
This command produces no output\.