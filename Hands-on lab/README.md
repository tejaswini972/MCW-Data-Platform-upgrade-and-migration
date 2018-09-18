# Data Platform upgrade and migration hands-on lab

## Contents

- [Abstract](#abstract)
- [Overview](#overview)
- [Solution architecture](#solution-architecture)
- [Requirements](#requirements)
- [Before the hands-on lab](#before-the-hands-on-lab)
- [Hands-on lab](#hands-on-lab)

## Abstract

In this hands-on lab, you will implement a proof of concept (POC) for conducting a site analysis for a customer to compare cost, performance, and level of effort required to migrate from Oracle to SQL Server. You will evaluate the dependent applications and reports that will need to be updated, and come up with a migration plan. In addition, you will help the customer take advantage of new SQL Server features to improve performance and resiliency, as well as conduct a migration from an old version of SQL Server to Azure SQL Database.

At the end of this hands-on lab, you will be better able to design and build a database migration plan and implement any required application changes associated with changing database technologies.

## Overview

World Wide Importers (WWI) has experienced significant growth in the last few years. In addition to predictable growth, they’ve had a substantial amount of growth in the data they store in their data warehouse. Their data warehouse is starting to show its age; slowing down during extract, transform, and load (ETL) operations and during critical queries. It was built on SQL Server 2008 R2 Standard Edition.

The WWI CIO has recently read about new performance enhancements of Azure SQL Database and SQL Server 2017. She is excited about the potential performance improvements related to clustered ColumnStore indexes. She is also hoping that table compression will improve performance and backup times.

WWI is concerned about upgrading their database to Azure SQL Database or SQL Server 2017. The data warehouse has been successful for a long time. As it has grown, it has filled with data, stored procedures, views, and security. WWI wants assurance that if it moves its data store, it won’t run into any incompatibilities with the storage engine of Azure SQL Database or SQL Server 2017.

WWI’s CIO would like a POC of a data warehouse move and proof that the new technology will help ETL and query performance.

## Solution architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully, so you understand the whole of the solution as you are working on the various components.

![This solution diagram is divided in to Microsoft Azure, and On Premises. Microsoft Azure includes SQL Server 2017 in a VM as an Always On Secondary, and Azure SQL Data Warehouse for a stretch table. On Premise includes the following elements: API App for vendor connections; Web App for Internet Sales Transactions; ASP.NET Core App for inventory management; SQL Server 2017 OLTP for Always On and JSON store; SSRS 2017 for Reporting of OLTP, Data Warehouse, and Cubes; SSIS 2017 for a Data Warehouse Load; Excel for reporting; SQL Server 2017 Enterprise for a Data Warehouse; and SSAS 2017 for a Data Warehouse. ](./media/preferred-solution-architecture.png "Preferred Solution diagram")

The solution begins with using the Microsoft Data Migration Assistant to perform an assessment to see what potentials issues need to be addressed in upgrading the database to SQL Server 2017 or Azure SQL Database. After correcting any issues, the SQL Server 2008 database is migrated to Azure SQL Database, using the Azure Database Migration Service. Two features of Azure SQL Database, Table Compression and ColumnStore Index, will be applied to demonstrate value and performance improvements from the upgrade. For the ColumnStore Index, a new table based on the existing FactResellerSales table will be created, and a ColumnStore index applied. Next, the Oracle XE database supporting the application will be migrated to an on-premises SQL Server 2017 Enterprise instance using SQL Server Migration Assistant (SSMA) 7.x for Oracle. Once the Oracle database has been migrated, the NorthwindMVC application will be updated, so it targets SQL Server 2017 instead of Oracle. The entity models are updated against SQL Server, and code updates are made to use the new Entity Framework context based on SQL Server. Corrections to stored procedures are made due to differences in how stored procedures are accessed in Oracle versus SQL Server.

## Requirements

- Microsoft Azure subscription must be pay-as-you-go or MSDN
  - Trial subscriptions will not work
- A virtual machine configured with:
  - Visual Studio Community 2017 or later
  - Azure SDK 2.9 or later (Included with Visual Studio 2017)

## Before the hands-on lab

Before attending the hands-on lab workshop, you should set up your environment for use in the rest of the hands-on lab.

You should follow all the steps provided in the [Before the hands-on lab](./Before%20the%20HOL%20-%20Data%20Platform%20upgrade%20and%20migration.md) section to prepare your environment before attending the hands-on lab. Failure to complete the Before the hands-on lab setup may result in an inability to complete the lab with in the time allowed.

## Hands-on lab

Step-by-step instructions for completing the lab are provided in the [Step-by-step guide](./HOL%20step-by-step%20-%20Data%20Platform%20upgrade%20and%20migration.md)