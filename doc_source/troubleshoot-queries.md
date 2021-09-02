# Troubleshoot common issues with SQL queries in AWS IoT Analytics<a name="troubleshoot-queries"></a>

Use the following information to help troubleshoot issues with your SQL queries in AWS IoT Analytics\.
+ **To escape a single quote**, precede it with another single quote\. Don't confuse this with a double quote\.  
**Example**  

  ```
  SELECT 'O''Reilly'
  ```
+ **To escape underscores**, use backticks to enclose data store column names that begin with an underscore\.  
**Example**  

  ```
  SELECT `_myMessageAttribute` FROM myDataStore
  ```
+ **To escape names with numbers**, enclose data store names that include numbers in double quotes\.  
**Example**  

  ```
  SELECT * FROM "myDataStore123"
  ```
+ **To escape reserved keywords**, enclose reserved keywords in double quotes\. For more information, see [List of Reserved Keywords](https://docs.aws.amazon.com/athena/latest/ug/reserved-words.html#list-of-reserved-words-sql-select) in *SQL SELECT Statements*\.