# WebSphere_Configure_Snowflake_JDBC

**IN CONSTRUCTION**

IBM WebSphere <BR>
How to create a Data Source connection for a Snowflake Database <BR> 
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
Snowflake provides a JDBC type 4 driver that supports core JDBC functionality. The JDBC driver must be installed in a 64-bit environment and requires Java 1.8 (or higher). 
1. Snowflake documentation: https://docs.snowflake.com/en/user-guide/jdbc.html. So please ensure WebSphere is running with JDK 1.8 (AppServerHome/bin/versioninfo.[sh,bat]) to support JDBC.  


2 Configure Data Source
=======================

2.1 JDBC
--------

1.	Goto IBM WebSphere Console > Resource pane and select “JDBC Drivers” 

2.	Create a new driver


Use implementation class net.snowflake.client.pooling.SnowflakeConnectionPoolDataSource and “Database Type” = “User-defined”
 

Set the driver path. For example: ${WAS_INSTALL_ROOT}/snowflake/snowflake-jdbc-x.y.z.jar

Finish

2.2 Datasource
--------------


1.	Goto IBM WebSphere Console > Resource pane and select “Data sources (WebSphere Application Server V4)”. You must use the “..Server V4” option otherwise Custom Properties cannot be set.
 
2.	- Select the JDBC “Provider” you have created before
- Set your “Data Source Name” and “JNDI Name”	
- Set the “Database Name”. Note: Do not yet the entire JDBC connection string here as it is usually done for DB”/Oracle. You should set the DB name only.
- Set “My Default User ID” and “Default Password” you got from Snowflake team
 
- Click “Apply” then “Save”
 

3.	Now modify the DS you have just created
 


Select “Custom Properties” and create “New” Property”
 

Create new
 

Create all the required Snowflake properties.  You  must create properties for each option you got from Snowflake
 
Create `Url` Custom Property. For example: `jdbc:snowflake://abc_cdl.us.mylink.snowflakecomputing.com`

 


Perform the same step for each SnowFlake option you got. After you finished, it should for example looks like this:

 

4.	Test your DS connection. If all parameters well set correctly it should be successful:
 

3 Troubleshooting
=================	

1. If connection fails, check if all parameters you got from Snowflake were set correctly.
Check class name as described here: https://www.ibm.com/support/pages/created-datasource-snowflake-received-classcastexception-when-click-test-connection

2. You can gather the following J2C trace file while recreating the issue and take a deeper look into the problem:
*=info:WAS.j2c=all:RRA=all:Transaction=all

    - How to set a trace is described here: https://www.ibm.com/support/pages/mustgather-connection-pooling-problems-websphere-application-server


 

