+++
title = "Amazon Redshift Spectrum"
date = 2021-02-17T17:04:42-06:00
weight = 2
chapter = false
pre = "<b>1. </b>"
+++

## Lab 1: Querying Data with Amazon Redshift Spectrum

### Overview
The IT team at UnicornNation setup a Redshift Cluster and are planning on migrating their existing Oracle Data Warehouse, which is stores current data in their Tickit database. They also want to make the historical data available from S3 from the Tickit-History database, using Redshift Spectrum.

For this lab, we have prepared a number of tables in the Glue Data Catalog based on this Tickit History data. We are going to register an external schema so you can use this data in Amazon Redshift, using Redshift Spectrum to access the external schema and tables.

First, we are going to connect to the AWS Glue Tickit-History database from S3 data. But first, we need to register the Tickit-History database/schema as an external schema in Redshift. To create this external schema, we will use the SQL Workbench/J to execute the SQL queries,which is pre-installed in a Windows server provisiones as part of this lab environment.
