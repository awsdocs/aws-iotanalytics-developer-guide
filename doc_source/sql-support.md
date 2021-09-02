# SQL expressions in AWS IoT Analytics<a name="sql-support"></a>

Datasets are generated using SQL expressions on data in a data store\. AWS IoT Analytics uses the same SQL queries, functions and operators as Amazon Athena\.

AWS IoT Analytics supports a subset of ANSI standard SQL syntax\.

```
SELECT [ ALL | DISTINCT ] select_expression [, ...]
[ FROM from_item [, ...] ]
[[ INNER | OUTER ] LEFT | RIGHT | FULL | CROSS JOIN join_item [ ON join_condition ]]
[ WHERE condition ]
[ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] ]
[ HAVING condition ]
[ UNION [ ALL | DISTINCT ] union_query ]
[ ORDER BY expression [ ASC | DESC ] [ NULLS FIRST | NULLS LAST] [, ...] ]
[ LIMIT [ count | ALL ] ]
```

For a description of the parameters, see [Parameters](https://docs.aws.amazon.com/athena/latest/ug/select.html#select-parameters) in the *Amazon Athena documentation*\.

AWS IoT Analytics and Amazon Athena doesn't support the following:
+ `WITH` clauses\.
+ `CREATE TABLE AS SELECT` statements
+ `INSERT INTO` statements
+ Prepared statements, you can't run `EXECUTE` with `USING`\.
+ `CREATE TABLE LIKE`
+ `DESCRIBE INPUT` and `DESCRIBE OUTPUT`
+ `EXPLAIN` statements
+ User\-defined functions \(UDFs or UDAFs\)
+ Stored procedures
+ Federated connectors

**Topics**
+ [Supported SQL functionality in AWS IoT Analytics](supported-fuctionality.md)
+ [Troubleshoot common issues with SQL queries in AWS IoT Analytics](troubleshoot-queries.md)