# Reprocessing channel messages<a name="reprocessing"></a>

AWS IoT Analytics enables you to reprocess channel data\. This can be useful in the following cases:
+ You want to replay existing ingested data rather than starting over\.
+ You make an update to a pipeline and want to bring existing data up\-to\-date with the changes\.
+ You want to include data that was ingested before you made changes to the customer managed storage options, permissions for channels, or data store\.

## Parameters<a name="reprocess-parameters"></a>

When you reprocess channel messages through the pipeline with AWS IoT Analytics, you must specify the following information:

`StartPipelineReprocessing`  
Starts reprocessing channel messages through the pipeline\.    
`ChannelMessages`  
Specifies one or more sets of channel messages that you want to reprocess\.  
If you use the `channelMessages` object, you must not specify a value for `startTime` and `endTime`\.    
`s3Paths`  
Specifies one or more keys that identify the Amazon Simple Storage Service \(Amazon S3\) objects that save your channel messages\. You must use the full path for the key\.  
Example path: `00:00:00/1582940490000_1582940520000_123456789012_mychannel_0_2118.0.json.gz`  
Type: Array of strings  
Array members constraints: 1\-100 items\.  
Length constraints: 1\-1024 characters\.  
`endTime`  
The end time \(exclusive\) of the channel data that is reprocessed\.  
If you specify a value for the `endTime` parameter, you must not use the `channelMessages` object\.  
Type: Timestamp  
`startTime`  
The start time \(inclusive\) of raw message data that is reprocessed\.  
If you specify a value for the `startTime` parameter, you must not use the `channelMessages` object\.  
Type: Timestamp

`pipelineName`  
The name of the pipeline on which to start reprocessing\.  
Type: String  
Length constraints: 1\-128 characters\.

## Reprocessing channel messages \(console\)<a name="reprocessing-console"></a>

This tutorial shows you how to reprocess the channel data that is stored in the specified Amazon S3 object in the AWS IoT Analytics console\.

Before you begin, make sure that the channel messages that you want to reprocess are saved in a customer managed Amazon S3 bucket\.

1. Sign in to the [AWS IoT Analytics console](https://console.aws.amazon.com/iotanalytics/)\.

1. In the navigation pane, choose **Pipelines**\.

1. Choose your target pipeline\.

1. Choose **Reprocess messages** from **Actions**\.

1. On the **Pipeline reprocessing** page, choose **S3 objects** for **Reprocess messages**\.

   The AWS IoT Analytics console also provides the following options:
   + **All available range** \- Reprocess all valid data in the channel\.
   + **Last 120 days** \- Reprocess data that arrived in the last 120 days\.
   + **Last 90 days** \- Reprocess data that arrived in the last 90 days\.
   + **Last 30 days** \- Reprocess data that arrived in the last 30 days\.
   + **Custom range** \- Reprocess data that arrived in the specified time range\. You can choose any time range\.

1. Enter the key of the Amazon S3 obejct that stores your channel messages\.

   To find the key, do the following: 

   1. Go to the [Amazon S3 console](https://console.aws.amazon.com/s3/)\.

   1. Choose the target Amazon S3 object\.

   1. Under **Properties**, in the **Object overview** section, copy the key\.

1. Choose **Start reprocessing**\.

## Reprocessing channel messages \(API\)<a name="reprocessing-api"></a>

When you use the `StartPipelineReprocessing` API, note the following:
+ The `startTime` and `endTime` parameters specify when the raw data was ingested, but these are rough estimates\. You can round to the nearest hour\. The `startTime` is inclusive, but the `endTime` is exclusive\.
+ The command launches the reprocessing asynchronously and returns immediately\.
+ There is no guarantee that reprocessed messages are processed in the order they were originally received\. It is roughly the same, but not exact\.
+ You can make up to 1000 `StartPipelineReprocessing` API requests for every 24 hours to reprocess the same channel messages through a pipeline\.
+ Reprocessing your raw data incurs additional costs\.

For more information, see the [StartPipelineReprocessing](https://docs.aws.amazon.com/iotanalytics/latest/APIReference/API_StartPipelineReprocessing.html) API, in *AWS IoT Analytics API Reference*\.

## Canceling channel reprocessing activities<a name="cancel-reprocessing"></a>

To cancel a pipeline reprocessing activity, use the [CancelPipelineReprocessing](https://docs.aws.amazon.com/iotanalytics/latest/APIReference/API_CancelPipelineReprocessing.html) API or choose **Cancel reprocessing** on the **Activities** page in the AWS IoT Analytics console\. If you cancel the reprocessing, the remaining data won't be reprocessed\. You must start another reprocessing request\.

Use the [DescribePipeline](https://docs.aws.amazon.com/iotanalytics/latest/APIReference/API_DeletePipeline.html) API to check the status of the reprocessing\. See the `reprocessingSummaries` field in the response\.