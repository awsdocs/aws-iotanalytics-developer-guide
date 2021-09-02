# Run statistical queries<a name="tutorial-step3"></a>

Now that you've explored your AWS IoT SiteWise data, you can run statistical queries that provide valuable insights to your industrial equipment\. The following queries demonstrate some of the information that you can retrieve\.

**To run statistical queries on AWS IoT SiteWise demo wind farm data**

1. <a name="join-latest-values"></a>Run the following SQL command to find the latest values of all properties with numeric values for a particular asset \(Demo Turbine Asset 4\)\.

   ```
   SELECT assetName,
       assetPropertyName,
       assetPropertyUnit,
       max_by(value, timeInSeconds) AS Latest
   FROM (
       SELECT *,
           CASE assetPropertyDataType
           WHEN 'DOUBLE' THEN
           cast(doubleValue AS varchar)
           WHEN 'INTEGER' THEN
           cast(integerValue AS varchar)
           WHEN 'STRING' THEN
           stringValue
           WHEN 'BOOLEAN' THEN
           cast(booleanValue AS varchar)
           ELSE NULL
           END AS value
       FROM my_iotsitewise_datastore.asset_metadata AS asset_metadata
       JOIN my_iotsitewise_datastore.raw AS raw
           ON raw.seriesId = asset_metadata.timeSeriesId
       WHERE startYear=2021
           AND startMonth=7
           AND startDay=8
           AND assetName='Demo Turbine Asset 4'
   )
   GROUP BY assetName, assetPropertyName, assetPropertyUnit
   ```

1. <a name="child-max-speeds"></a>Join both metadata tables and your raw table to identify the maximum wind speed properties for all assets, in addition to their parent assets\.

   ```
   SELECT child_assets_data_set.parentAssetId,
           child_assets_data_set.childAssetId,
           asset_metadata.assetPropertyId,
           asset_metadata.assetPropertyName,
           asset_metadata.timeSeriesId,
           raw_data_set.max_speed
   FROM (
      SELECT sourceAssetId AS parentAssetId,
           targetAssetId AS childAssetId
       FROM my_iotsitewise_datastore.asset_hierarchy_metadata
       WHERE associationType = 'CHILD'
   ) 
   AS child_assets_data_set
   JOIN mls_demo.asset_metadata AS asset_metadata
       ON asset_metadata.assetId = child_assets_data_set.childAssetId
   JOIN (
       SELECT seriesId, MAX(doubleValue) AS max_speed
       FROM my_iotsitewise_datastore.raw
       GROUP BY seriesId
   ) 
   AS raw_data_set
   ON raw_data_set.seriesId = asset_metadata.timeseriesid
   WHERE assetPropertyName = 'Wind Speed'
   ORDER BY max_speed DESC
   ```

1. <a name="average-single-property"></a>To find the average value of a particular property \(Wind Speed\) for an asset \(Demo Turbine Asset 2\), run the following SQL command\. You must replace `my_bucket_id` with the ID of your bucket\.

   ```
   SELECT AVG(doubleValue) as "Average wind speed"
   FROM my_iotsitewise_datastore.raw
   WHERE seriesId = 
       (SELECT timeseriesId
       FROM my_iotsitewise_datastore.asset_metadata as asset_metadata
       WHERE asset_metadata.assetname = 'Demo Turbine Asset 2'
               AND asset_metadata.assetpropertyname = 'Wind Speed')
   ```

## Next step<a name="tutorial-step3.1"></a>

 [Cleaning up your tutorial resources](tutorial-step4.md) 