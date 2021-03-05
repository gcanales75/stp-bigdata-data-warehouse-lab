+++ 
title = "Task 2: Create a DMS replication instance" 
chapter = false 
weight = 2 
+++

1. Now you are going to create a replication instance. From the left panel, click on **Replication instances**

1. Click **Create replication instance**

1. Enter a name for your replication instance, type `teamawesome-replication-instance`

1. Enter a short Description (i.e. "*Datawarehouse DMS replication instance*")

1. For the instance class, select **dms.t3.medium**

1. For the VPC, select the **VPC-Lab-Datawarehouse** and uncheck **Public accessible**

	<img src="../images/dms-instance-vpc.png" alt="drawing" width="500"/>

1. Click **Create**

	Please wait 3-4 minutes for the replication instance to fully deploy

1. DMS instance status must change to **Available**