+++ 
title = "Task 1: Convert your Oracle schema to Redshift" 
chapter = false 
weight = 1 
+++

1. To do this lab, you will need the Private IP of an Oracle DW deployed in this lab environment, that you will use as the source database to perform a migration to Amazon Redshift, and the public IP of a Windows host which has preinstalled the Schema Convertion Tool (SCT). These IP's were obtained in Lab #1.

1. The *Schema Conversion Tool (SCT)* is preinstalled in the same Windows host where you ran queries using SQL Workbench/J. Use the below credentials to log in to the instance using any RDP client installed in your computer

	| Key | Value  |
	|---|---|
	| Host IP  | [*Windows host public IP*]  |
	|  User | Administrator  |
	|  Password | PsoBda2020$Win  |

1. Open the AWS Schema Conversion Tool (SCT), it will take ~1 minute to launch

	<img src="../images/open-sct.png" alt="drawing" width="500"/>

1. If you see a prompt window asking for a SCT update, select **Not now**

1. Once the *Schema Conversion Tool (SCT)* has launched, in the Menu panel, click on File > New project

	<img src="../images/open-new-proyect.png" alt="drawing" width="200"/>

1. Update the New project information with the below values

	| Key | Value  |
	|---|---|
	| Project Name  | Convert Oracle DwH to Redshift  |
	|  DB category | Data warehouse (OLAP)  |
	|  Source engine | Oracle DW |
	|  Target engine | Amazon Redshift  |

1. Click **OK**

1. Once in your new project panel, you need to disable **AWS Glue** as the target for stored procedures conversion, click on Settings > Project Settings > Conversion settings > Use AWS Glue (UNCHECK)

	<img src="../images/uncheck-glue.png" alt="drawing" width="700"/>

1. Click **OK**

1. Click on **Connect to Oracle DW**

1. You will need the Oracle DW private IPv4 recorded in this lab's step 6. Update the connection information with the below values:

	| Key | Value  |
	|---|---|
	| Server name  | [*Oracle DW private IP*]  |
	|  Server port | 1521  |
	|  Oracle SID | xe |
	|  User name | TICKITDWH  |
	|  Password | tickit#2020$dw  |

1. Click **OK**

1. An SSL security warning will show in the screen, since this a lab environment we can proceed without configuring a secure SSL connection.

	<img src="../images/oracle-ssl-warning.png" alt="drawing" width="500"/>

1. Click on **Connect to Amazon Redshift**. Update the connection information with the below values:

	| Key | Value  |
	|---|---|
	| Server name  | [*Amazon Redshift endpoint*]  |
	|  Server port | 5439 |
	|  Database | psobigdata |
	|  User name | awsuser |
	|  Password | PsoBigData01 |

1. Click **OK**

1. From the **Oracle DW** pane on the left-hand side, DE-SELECT all of the items in the list

1. In the same pane, click to select the **TICKITDWH** schema

1. Right-click on the **TICKITDWH** schema and select “Convert Schema”

1. If prompted, select “Yes” to replace objects if they exist in the target and “Collect and Continue” if prompted about statistics.

1. From the right-hand pane, expand the **TICKITDWH** schema and you should see your tables and views converted to your Redshift Database format.

1. Right-click on the tickitdwh schema and select **Apply to Database**

	<img src="../images/sct-apply-db.png" alt="drawing" width="300"/>

	If prompted, select “Yes” to create the objects in the Redshift Database. 

You have now successfully converted your Oracle Data Warehouse schema to Redshift—in the next lab, we are going to migrate the actual data from your Oracle data warehouse to Redshift.