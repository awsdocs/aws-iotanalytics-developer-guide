# Creating a data store<a name="create-data-store"></a>

A data store receives and stores your messages\. It is not a database but a scalable and queryable repository of your messages\. You can create multiple data stores to store messages that comes from different devices or locations, or your can use a single data store to receive all of your AWS IoT messages\.

```
aws iotanalytics create-datastore --datastore-name mydatastore
```

To list the data stores you have already created\.

```
aws iotanalytics list-datastores
```

To get more information about a data store\.

```
aws iotanalytics describe-datastore --datastore-name mydatastore
```