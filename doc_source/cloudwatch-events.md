# Monitoring with Amazon CloudWatch Events<a name="cloudwatch-events"></a>

AWS IoT Analytics automatically publishes an event to Amazon CloudWatch Events when a runtime error occurs during an AWS Lambda activity\. This event contains a detailed error message and the keys of the Amazon Simple Storage Service \(Amazon S3\) objects that store the unprocessed channel messages\. You can use the Amazon S3 keys to reprocess the unprocessed channel messages\. For more information, see [Reprocessing channel messages](reprocessing.md), the [StartPipelineReprocessing](https://docs.aws.amazon.com/iotanalytics/latest/APIReference/API_StartPipelineReprocessing.html) API in the *AWS IoT Analytics API Reference*, and [What Is Amazon CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) in the *Amazon CloudWatch Events User Guide*\.

You can also configure targets that enable Amazon CloudWatch Events to send notifications or take further actions\. For example, you can send the notification to an Amazon Simple Queue Service \(Amazon SQS\) queue, and then invoke the `StartReprocessingMessage` API to process the channel messages saved in the Amazon S3 objects\. Amazon CloudWatch Events supports many types of targets, such as the following:
+  Amazon Kinesis streams
+ AWS Lambda functions
+ Amazon Simple Notification Service \(Amazon SNS\) topics
+ Amazon Simple Queue Service \(Amazon SQS\) queues

For the list of supported targets, see [ Amazon EventBridge Targets](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-targets.html) in the *Amazon EventBridge User Guide*\.

Your CloudWatch Events resources and the associated targets must be in the AWS Region where you created your AWS IoT Analytics resources\. For more information, see [Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html) in the *AWS General Reference*\.

The notification sent to Amazon CloudWatch Events for runtime errors in the AWS Lambda activity uses the following format\.

```
{
    "version": "version-id",
    "id": "event-id",
    "detail-type": "IoT Analytics Pipeline Failure Notification",
    "source": "aws.iotanalytics",
    "account": "aws-account",
    "time": "timestamp",
    "region": "aws-region",
    "resources": [
        "pipeline-arn"
    ],
    "detail": {
        "event-detail-version": "1.0",
        "pipeline-name": "pipeline-name",
        "error-code": "LAMBDA_FAILURE",
        "message": "error-message",
        "channel-messages": {
            "s3paths": [
                "s3-keys"
            ]
        },
        "activity-name": "lambda-activity-name",
        "lambda-function-arn": "lambda-function-arn"
    }
}
```

Example notification:

```
{
    "version": "0",
    "id": "204e672e-ef12-09af-4cfd-de3b53673ec6",
    "detail-type": "IoT Analytics Pipeline Failure Notification",
    "source": "aws.iotanalytics",
    "account": "123456789012",
    "time": "2020-10-15T23:47:02Z",
    "region": "ap-southeast-2",
    "resources": [
        "arn:aws:iotanalytics:ap-southeast-2:123456789012:pipeline/test_pipeline_failure"
    ],
    "detail": {
        "event-detail-version": "1.0",
        "pipeline-name": "test_pipeline_failure",
        "error-code": "LAMBDA_FAILURE",
        "message": "Temp unavaliable",
        "channel-messages": {
            "s3paths": [
                "test_pipeline_failure/channel/cmr_channel/__dt=2020-10-15 00:00:00/1602805530000_1602805560000_123456789012_cmr_channel_0_257.0.json.gz"
            ]
        },
        "activity-name": "LambdaActivity_33",
        "lambda-function-arn": "arn:aws:lambda:ap-southeast-2:123456789012:function:lambda_activity"
    }
}
```