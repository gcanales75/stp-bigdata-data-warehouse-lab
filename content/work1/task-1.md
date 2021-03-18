+++ 
title = "Task 1: Querying Data with Amazon Redshift Spectrum" 
chapter = false 
weight = 1 
+++

1. To do this lab activities, you will need to access a Windows server where SQL Workbench/J is pre-installed, to grant access to the Windows server you will need some parameters which are found in CloudFormation, please navigate to the CloudFormation Service (Service *search box* -> CloudFormation)

1. Click on the CloudFormation stack listed

	<img src="../images/cf-1.png" alt="drawing" width="600"/>

1. Click on the **Outputs** panel

	<img src="../images/cf-2.png" alt="drawing" width="600"/>

1. Copy and paste the output values from the following parameters in a notepad, you will use those parameters later in this lab and in lab #2:

	| Description |
	|---|
	| AWS account number  |
	| Oracle DW private IP  |
	| Amazon Redshift endpoint  |
	| Windows host public IP  |

1. Also you will need the Redshift JDBC URL, please navigate to the Redshift console (Service *search box* -> Redshift)

1. From the left menu, click on **Clusters**, then click on your Redshift cluster

	<img src="../images/click-on-cluster-2.png" alt="drawing" width="300"/>

1. Copy and paste in a notepad the JDBC URL of your Redshift cluster

	<img src="../images/copy-jdbc-url.png" alt="drawing" width="900"/>


1. Now you will RDP to a Windows server where SQL Workbench/J is preinstalled so you can execute SQL scripts. Use the below credentials to log in to the Windows instance using any RDP client installed in your computer

	| Key | Value  |
	|---|---|
	| Host IP  | [*Windows host public IP*]  |
	|  User | Administrator  |
	|  Password | PsoBda2020$Win  |

1. Once logged in, click **Yes**, to allow the windows intance to be network discovered

	<img src="../images/win-network.png" alt="drawing" width="300"/>

1. Start SQL Workbench/J

	<img src="../images/benchmark-access.png" alt="drawing" width="450"/>

1. You will be prompted to start a new database connection. Make sure the **Redshift-pso-bda-lab** connection is selected. Fill the information to create the connection to Redshift

	| Key | Value  |
	|---|---|
	| URL  | [*Redshift JDBC URL*]  |
	|  Username | awsuser  |
	|  Password | PsoBigData01  |

	<img src="../images/52_39_219_61.png" alt="drawing" width="900"/>

1. Click **OK**

1. Now that you have started a database connection to your Redshift cluster you can start running queries, copy and paste the below script in the SQL Workbench/J query editor text box. Before runnning the query, update the IAM role ARN with the actual AWS account number (ACCOUNT_NUMBER) you have recorded in step 4.

	{{% notice info %}}
**IMPORTANT**: Do not open a new *Statement* tab, run all queries in the *Statement 1* tab
{{% /notice %}}

	```
	create external schema tickithistory
	from data catalog
	database 'teamawesome-tickit-history'
	region 'us-west-2'
	iam_role 'arn:aws:iam::ACCOUNT_NUMBER:role/RedshiftIAMRole1'
	create external database if not exists;
	```

1. Run query.

	<img src="../images/run-query-1.png" alt="drawing" width="900"/>

	You must see a similar output:

	<img src="../images/query1-output.png" alt="drawing" width="500"/>

1. To make sure the external schema has been created, run the following query in the same *Statement 1* query window:

	```
	select * from svv_external_schemas
	```

	You must see a similar output in the below results panel

	<img src="../images/query2-output.png" alt="drawing" width="1000"/>

1. To make sure that your external tables are available for querying, run the following query in the same *Statement 1* query window:

	```
	select * from svv_external_tables
	```

	In the below panel you must see a list of tables

	<img src="../images/query3-output.png" alt="drawing" width="400"/>
	
1. Now that you have confirmed that your external tables are accessible, you can run queries from Redshift to Athena databases. 

	*Sample query question*: Using the following query, we will find what were the Top 5 ticket sellers for events in San Diego in 2008?

	```
	select sellerid, username, city, firstname ||' '|| lastname as fullname, sum(qtysold) as qtysold
	from tickithistory.sales, tickithistory.date, tickithistory.users
	where sales.sellerid = users.userid
	and sales.dateid = date.dateid
	and year = 2008
	and city = 'San Diego'
	group by sellerid, username, city, firstname ||' '|| lastname
	order by 5 desc
	limit 5;
	```