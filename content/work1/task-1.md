+++ 
title = "Task 1: Querying Data with Amazon Redshift Spectrum" 
chapter = false 
weight = 1 
+++

1. From the console, navigate to the **Amazon Redshift** service. Service *search box* > Redshift

	<img src="../images/service-redshift.png" alt="drawing" width="600"/>

1. We need to create a new database connection, click on the **EDITOR** icon, then click on **Query editor**

	<img src="../images/click-on-editor.png" alt="drawing" width="100"/>

1. In this lab, we will use the previous Redshift query editor version, so please click on **old query editor** from the blue banner at the top of the page.

	<img src="../images/old-query-editor.png" alt="drawing" width="800"/>
	
1. In the **Connect to database** dialog box, update fields with this parameters:

	| Key | Value  |
	|---|---|
	| Database name  | psobigdata  |
	|  Database user | awsuser  |
	|  Database password | PsoBigData01  |

1. Click **Connect to database**

	<img src="../images/redshift-connect-to-database.png" alt="drawing" width="500"/>

1. Copy and paste the below script in the Amazon Redshift query editor text box. Before you need to update the IAM role ARN with the actual AWS account number (ACCOUNT_NUMBER), you will find it in the console upper right corner, it's a 12 digits number. Remove hyphens '-'.

	<img src="../images/aws-account-number.png" alt="drawing" width="500"/>

	```
	create external schema tickithistory
	from data catalog
	database 'teamawesome-tickit-history'
	region 'us-west-2'
	iam_role 'arn:aws:iam::ACCOUNT_NUMBER:role/RedshiftIAMRole1'
	create external database if not exists;
	```

1. Run query.

	<img src="../images/redshift-run-query.png" alt="drawing" width="700"/>


1. To make sure the external schema has been created, run the following query in the same Query window:

	```
	select * from svv_external_schemas
	```

	You must see a similar output in the below **Rows returned** panel

	<img src="../images/redshift-new-schema.png" alt="drawing" width="600"/>

1. To make sure that your external tables are available for querying, run the following query:

	```
	select * from svv_external_tables
	```

	Below **Rows returned** you must see a list of tables
	
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