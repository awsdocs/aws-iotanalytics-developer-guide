# Data exploration<a name="tutorial-step2"></a>

After your AWS IoT SiteWise data is created and loaded into a data store, you can create an AWS IoT Analytics dataset and run SQL queries in AWS IoT Analytics to discover insights about your assets\. The following queries demonstrate how you can explore your data before running statistical queries\.

**To explore your data with SQL queries**

1. <a name="raw-limit"></a>View a sample of columns and values in each table, such as in the raw table\. 

   ```
   SELECT * FROM my_iotsitewise_datastore.raw LIMIT 5
   ```

1. <a name="metadata-distinct-names"></a>Use `SELECT DISTINCT` to query your `asset_metadata` table and list the \(unique\) names of your AWS IoT SiteWise assets\. 

   ```
   SELECT DISTINCT assetname FROM my_iotsitewise_datastore.asset_metadata ORDER BY assetname
   ```

1. <a name="metadata-where"></a>To list information about properties for a particular AWS IoT SiteWise asset, use the `WHERE` clause\.

   ```
   SELECT assetpropertyname,
        assetpropertyunit,
        assetpropertydatatype
   FROM my_iotsitewise_datastore.asset_metadata
   WHERE assetname = 'Demo Turbine Asset 2'
   ```

1. <a name="join-relationships"></a>With AWS IoT Analytics, you can join data from two or more tables in your data store, such as in the following example\.

   ```
   SELECT * FROM my_iotsitewise_datastore.raw AS raw
   JOIN my_iotsitewise_datastore.asset_metadata AS asset_metadata
   ON raw.seriesId = asset_metadata.timeseriesId
   ```

   To view all relationships between your assets, use the `JOIN` functionality in the following query\.

   ```
   SELECT DISTINCT parent.assetName as "Parent name", 
       child.assetName AS "Child name" 
   FROM (
       SELECT sourceAssetId AS parent, 
               targetAssetId AS child 
       FROM my_iotsitewise_datastore.asset_hierarchy_metadata 
       WHERE associationType = 'CHILD'
   ) 
   AS relations 
   JOIN my_iotsitewise_datastore.asset_metadata AS child 
       ON relations.child = child.assetId
   JOIN my_iotsitewise_datastore.asset_metadata AS parent
       ON relations.parent = parent.assetId
   ```

## Next step<a name="tutorial-step2.1"></a>

 [Run statistical queries](tutorial-step3.md) 