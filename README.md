# WebSphere_Configure_Snowflake_JDBC

IBM WebSphere: How to create a jdbc connection for a Snowflake Database <BR> 
Version 1.0 <BR>
Created by Expertise Connect Team EMEA

- Josip Bosnjak
- Bernd Wynands
- Richard Niewolik

#

[1 Prerequisites](#1-prerequisites)

[1.1 Snowflake connection data](#11-snowflake-connection-data)

[1.2 WebSphere](#12-websphere)

[2 Configure Data Source](#2-configure-data-source)

[2.1 JDBC](#21-jdbc)

[2.1 Datasource](#22-datasource)

[3 Troubleshooting](#3-troubleshooting)

#

1 Prerequisites
===============

1.1 Snowflake connection data
------------------------------ 

Details on all the existing options can be found here: https://docs.snowflake.com/en/user-guide/jdbc-configure.html. The following data is required and is usually provided by the Snowflake database provider. 
- `Database Name` = name of the Snowflake DB
-	`URL` = JDBC driver connection string
-	`User` 				 
-	`Password` 
-	`Role` = role you got

There are also additional options and depending on your needs they may be provided as well. 		 
-	`Schema`			(optional)
-	`Warehouse` 			(optional)
-	…

For example:

```
db       = "MYDW_SI2" 
URL      = "jdbc:snowflake://abc_cdl.us.mylink.snowflakecomputing.com" 
User     = "myuser" 
password = "mypw"
role     = “F_ABC_MYROLE”
```

1.2 WebSphere
-------------

 1. You need administration permissions to be able to perform the required configuration steps.

 1. Additionally, you must install the Snowflake JDBC Driver (e.g. snowflake-jdbc-x.y.z.jar) on your WebSphere Server. The “snowflake-jdbc-3.12.16.jar” was proved as a working one. You can find it here: https://docs.snowflake.com/en/user-guide/jdbc.html
 
1. Snowflake provides a JDBC type 4 driver that supports core JDBC functionality. The JDBC driver must be installed in a 64-bit environment and requires Java 1.8 (or higher). Snowflake documentation https://docs.snowflake.com/en/user-guide/jdbc.html. So please ensure WebSphere is running with JDK 1.8 (AppServerHome/bin/versioninfo.[sh,bat]) to support JDBC.  


2 Configure Data Source
=======================

2.1 JDBC
--------
 1.	Goto IBM WebSphere Console > Resource pane and select “JDBC Drivers”  <BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165326545-afdfa3cf-e755-4cbb-8142-b4bac986a7aa.png" width="35%" height="35%">
 1.	Create a new driver  
    <img src="https://user-images.githubusercontent.com/22491423/165326965-cd6030dd-b202-4dda-98eb-1af6d237a49f.png" width="25%" height="25%">
 1. Use implementation class `net.snowflake.client.pooling.SnowflakeConnectionPoolDataSourceV and `Database Type = User-defined`
    <img src="https://user-images.githubusercontent.com/22491423/165327066-4977d408-301a-4406-8e27-3d6c6a27db5d.png" width="35%" height="35%">
 1. Set the driver path. For example: `${WAS_INSTALL_ROOT}/snowflake/snowflake-jdbc-x.y.z.jar` <BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165327510-a170ef3f-b86c-4349-a66e-d96967f34374.png" width="40%" height="40%">
 1. Finish <BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165327561-63b8c60c-efae-48dd-a7a4-e5b7fd5e047c.png" width="50%" height="50%">

 
2.2 Datasource
--------------
 1. Goto IBM WebSphere **Console > Resource** pane and select **Data sources (WebSphere Application Server V4)**. You must use the **..Server V4** option otherwise Custom Properties cannot be set later on.<BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165328397-834c59a7-8fce-4575-8eb9-8d9dfd2930a0.png" width="35%" height="35%">
 1. Select the JDBC “**Provider**” you have created before <BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165329203-241fdc05-3955-4054-a8ee-2499e240e701.png" width="35%" height="35%">
    - Set your “Data Source Name” and “JNDI Name”	
    - Set the “Database Name”. Note: Do not yet the entire JDBC connection string here as it is usually done for DB2/Oracle. You should set the DB name only.
    - Set “My Default User ID” and “Default Password” you got from Snowflake team
    - Click “Apply” then “Save” 
 1.	Now modify the DS you have just created <BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165330591-845a60ce-68d6-467a-a753-cdbdce31e761.png" width="35%" height="35%"> 
 1. Select “Custom Properties” and create “New” Property” <BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165330710-45847694-5088-458c-b5fe-32698c4b30eb.png" width="35%" height="35%">
 1. Create new  <BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165330814-d45f1b5c-9686-465f-89fa-e47d10e90e6d.png" width="35%" height="35%">
 1. Create `Url` Custom Property. For example: `jdbc:snowflake://abc_cdl.us.mylink.snowflakecomputing.com` <BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165331096-cd6df881-19b0-45db-aff1-f12ff55a8a01.png" width="35%" height="35%">
 1. Perform the same step for each SnowFlake option you got. After you finished, it should for example looks like this: <BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165331750-8cec0c83-2e11-470f-b16c-200847f93b83.png" width="35%" height="35%">
 1. Test your DS connection. If all parameters well set correctly it should be successful:  <BR> 
    <img src="https://user-images.githubusercontent.com/22491423/165331802-9797f8c8-9d70-46cd-97d1-10fbc89f6acd.png" width="35%" height="35%">


3 Troubleshooting
=================	

1. If connection fails, check if all parameters you got from Snowflake were set correctly.
Check class name as described here: https://www.ibm.com/support/pages/created-datasource-snowflake-received-classcastexception-when-click-test-connection

2. You can gather the following J2C trace file while recreating the issue and take a deeper look into the problem:
`*=info:WAS.j2c=all:RRA=all:Transaction=all`

    - How to set a trace is described here: https://www.ibm.com/support/pages/mustgather-connection-pooling-problems-websphere-application-server


 

