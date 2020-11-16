# Getting late data notifications through Amazon CloudWatch Events<a name="late-data-notification"></a>

When you create dataset contents using data from a specified time frame, some data might not arrive in time for processing\. To allow for a delay, you can specify a `deltaTime` offset for the `QueryFilter` when you [create a dataset](https://docs.aws.amazon.com/iotanalytics/latest/APIReference/API_CreateDataset.html) by applying a `queryAction` \(a SQL query\)\. AWS IoT Analytics still processes the data that arrives within the delta time, and your dataset contents have a time lag\. The late data notification feature enables AWS IoT Analytics to send notifications through [Amazon CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) when data arrives after the delta time\.

You can use the AWS IoT Analytics console, [API](https://docs.aws.amazon.com/iotanalytics/latest/APIReference/), [AWS Command Line Interface \(AWS CLI\)](https://docs.aws.amazon.com/cli/latest/reference/iotanalytics/index.html), or [AWS SDK](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sdks.html) to specify late data rules for a dataset\.

In the AWS IoT Analytics API, the `LateDataRuleConfiguration` object represents the late data rule settings of a dataset\. This object is part of the `Dataset` object associated with the `CreateDataset` and `UpdateDataset` API operations\.

## Parameters<a name="lateDataRules-parameters"></a>

When you create a late data rule for a dataset with AWS IoT Analytics, you must specify the following information:

**`ruleConfiguration` \(`LateDataRuleConfiguration`\)**  
A structure that contains the configuration information of a late data rule\.    
**`deltaTimeSessionWindowConfiguration`**  
A structure that contains the configuration information of a delta time session window\.  
[DeltaTime](https://docs.aws.amazon.com/iotanalytics/latest/APIReference/API_DeltaTime.html) specifies a time interval\. You can use `DeltaTime` to create dataset contents with data that has arrived in the data store since the last execution\. For an example of `DeltaTime`, see [Creating a SQL dataset with a delta window \(CLI\)](https://docs.aws.amazon.com/iotanalytics/latest/userguide/automate-create-dataset.html#automate-example6)\.    
**`timeoutInMinutes`**  
A time interval\. You can use `timeoutInMinutes` so that AWS IoT Analytics can batch up late data notifications that have been generated since the last execution\. AWS IoT Analytics sends one batch of notifications to CloudWatch Events at one time\.  
Type: Integer  
Valid range: 1\-60

**`ruleName`**  
The name of the late data rule\.  
Type: String

**Important**  
To specify `lateDataRules`, the dataset must use a `DeltaTime` filter\.

## Configure late data rules \(console\)<a name="config-late-data-rules-console"></a>

The following procedure shows you how to configure the late data rule of a dataset in the AWS IoT Analytics console\.

**To configure late data rules**

1. Sign in to the [AWS IoT Analytics console](https://console.aws.amazon.com/iotanalytics/)\.

1. In the navigation pane, choose **Data sets**\.

1. Under **Data sets**, choose the target data set\.

1. In the navigation pane, choose **Details**\.

1. In the **Delta window** section, choose **Edit**\.

1. Under **Configure data selection filter**, do the following:

   1. For **Data selection window**, choose **Delta time**\.

   1. For **Offset**, enter a time period, and then choose a unit\.

   1. For **Timestamp expression**, enter an expression\. This can be the name of a timestamp field or a SQL expression that can derive the time, such as *from\_unixtime\(time\)*\.

      For more information about how to write a timestamp expression, see [Date and Time Functions and Operators](https://prestodb.io/docs/0.172/functions/datetime.html) in the *Presto 0\.172 Documentation*\.

   1. For **Late data notification**, choose **Active**\.

   1. For **Delta time**, enter an integer\. The valid range is 1\-60\.

   1. Choose **Save**\.  
![\[Configure data selection filter in the AWS IoT Analytics console.\]](http://docs.aws.amazon.com/iotanalytics/latest/userguide/images/late-data-notification-1.png)

## Configure late data rules \(CLI\)<a name="config-late-data-rules-cli"></a>

In the AWS IoT Analytics API, the `LateDataRuleConfiguration` object represents the late data rule settings of a dataset\. This object is part of the `Dataset` object associated with `CreateDataset` and `UpdateDataset`\. You can use the [API](https://docs.aws.amazon.com/iotanalytics/latest/APIReference/), [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/iotanalytics/index.html), or [AWS SDK](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sdks.html) to specify late data rules for a dataset\. The following example uses the AWS CLI\.

To create your dataset with specified late data rules, run the following command\. The command assumes that the `dataset.json` file is in the current directory\.

**Note**  
You can use the [UpdateDataset](https://docs.aws.amazon.com/iotanalytics/latest/APIReference/API_UpdateDataset.html) API to update an existing dataset\. 

```
aws iotanalytics create-dataset --cli-input-json file://dataset.json
```

The `dataset.json` file should contain the following:
+ Replace *demo\_dataset* with the target dataset name\.
+ Replace *demo\_datastore* with the target data store name\.
+ Replace *from\_unixtime\(time\)* with the name of a timestamp field or a SQL expression that can derive the time\.

  For more information about how to write a timestamp expression, see [Date and Time Functions and Operators](https://prestodb.io/docs/0.172/functions/datetime.html) in the *Presto 0\.172 Documentation*\.
+ Replace *timeout* with an integer between 1–60\.
+ Replace *demo\_rule* with any name\.

```
{
    "datasetName": "demo_dataset",
    "actions": [
        {
            "actionName": "myDatasetAction",
            "queryAction": {
                "filters": [
                    {
                        "deltaTime": {
                            "offsetSeconds": -180,
                            "timeExpression": "from_unixtime(time)"
                        }
                    }
                ],
                "sqlQuery": "SELECT * FROM demo_datastore"
            }
        }
    ],
    "retentionPeriod": {
        "unlimited": false,
        "numberOfDays": 90
    },
    "lateDataRules": [
        {
            "ruleConfiguration": {
                "deltaTimeSessionWindowConfiguration": {
                    "timeoutInMinutes": timeout
                }
            },
            "ruleName": "demo_rule"
        }
    ]
}
```

## Subscribing to receive late data notifications<a name="subscribe-eventbridge"></a>

You can create rules in CloudWatch Events that defines how to process late data notifications sent from AWS IoT Analytics\. When CloudWatch Events receives the notifications, it invokes specified the target actions defined in your rules\.

### Prerequisites for creating CloudWatch Events rules<a name="cwe-rule-prereq"></a>

Before you create a CloudWatch Events rule for AWS IoT Analytics, you should do the following:
+ Familiarize yourself with events, rules, and targets in CloudWatch Events\.
+ Create and configure the [targets](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-targets.html) invoked by your CloudWatch Events rules\. Rules can invoke many types of targets, such as the following:
  +  Amazon Kinesis streams
  + AWS Lambda functions
  + Amazon Simple Notification Service \(Amazon SNS\) topics
  + Amazon Simple Queue Service \(Amazon SQS\) queues

  Your CloudWatch Events rule, and the associated targets must be in the AWS Region where you created your AWS IoT Analytics resources\. For more information, see [Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html) in the *AWS General Reference*\.

For more information, see [What is CloudWatch Events?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) and [Getting started with Amazon CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CWE_GettingStarted.html) in the *Amazon CloudWatch Events User Guide*\.

### Late data notification event<a name="late-data-notification-event"></a>

The event for late data notifications uses the following format\.

```
{
	"version": "0",
	"id": "7f51dfa7-ffef-97a5-c625-abddbac5eadd",
	"detail-type": "IoT Analytics Dataset Lifecycle Notification",
	"source": "aws.iotanalytics",
	"account": "123456789012",
	"time": "2020-05-14T02:38:46Z",
	"region": "us-east-2",
	"resources": ["arn:aws:iotanalytics:us-east-2:123456789012:dataset/demo_dataset"],
	"detail": {
		"event-detail-version": "1.0",
		"dataset-name": "demo_dataset",
		"late-data-rule-name": "demo_rule",
		"version-ids": ["78244852-8737-4650-aa4d-3071a01338fa"],
		"message": null
	}
}
```

### Create a CloudWatch Events rule to receive late data notifications<a name="create-cwe-rule-console"></a>

The following procedure shows you how to create a rule that sends AWS IoT Analytics late data notifications to an Amazon SQS queue\.

**To create a CloudWatch Events rule**

1. Sign in to the [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, under **Events**, choose **Rules**\.

1. On the **Rules** page, choose **Create rule**\.

1. Under **Event Source**, choose **Event Pattern**\.

1. In the **Build event pattern to match events by service** section, do the following:

   1. For **Service Name**, choose **IoT Analytics**

   1. For **Event Type**, choose **IoT Analytics Dataset Lifecycle Notification**\.

   1. Choose **Specific dataset name\(s\)**, and then enter the name of the target dataset\.

1. Under **Targets**, choose **Add target\***\.

1. Choose **SQS queue**, and then do the following:

   1. For **Queue\***, choose the target queue\.

1. Choose **Configure details**\.

1. On the **Step 2: Configure rule details** page, enter a name and a description\.

1. Choose **Create rule**\.