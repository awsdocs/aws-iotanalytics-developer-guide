# Working with AWS IoT SiteWise data<a name="dataset-itsw"></a>

AWS IoT SiteWise is a managed service that you can use to collect, model, analyze, and visualize data from industrial equipment at scale\. The service provides an asset modeling framework for building representations of your industrial devices, processes, and facilities\.

With AWS IoT SiteWise asset models, you can define what industrial equipment data to consume and how to process your data into complex metrics\. You can configure asset models to collect and process data in the AWS Cloud\. For more information, see the [AWS IoT SiteWise](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/what-is-sitewise.html) User Guide\.

AWS IoT Analytics integrates with AWS IoT SiteWise so you can run and schedule SQL queries on AWS IoT SiteWise data\. To start querying your AWS IoT SiteWise data, create a data store by following the procedures in [Configure storage settings](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/configure-storage.html) in the *AWS IoT SiteWise User Guide*\. Then, follow the steps in [Create a dataset with AWS IoT SiteWise data \(Console\)](create-dataset-itsw-console.md) or in [Create a dataset with AWS IoT SiteWise data \(AWS CLI\)](create-dataset-itsw-cli.md) to create an AWS IoT Analytics dataset and run a SQL query on your industrial data\. 

**Topics**
+ [Create an AWS IoT Analytics dataset with AWS IoT SiteWise data](create-dataset-mls.md)
+ [Access dataset contents](dataset-results-itsw.md)
+ [Tutorial: Query AWS IoT SiteWise data in AWS IoT Analytics](tutorial-query-mls.md)