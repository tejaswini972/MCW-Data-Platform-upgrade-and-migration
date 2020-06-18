# Data Platform upgrade and migration hands-on lab step-by-step

## Abstract and learning objectives

In this hands-on lab, you implement a proof of concept (POC) for conducting a site analysis for a customer to compare cost, performance, and level of effort required to migrate from Oracle to SQL Server. You evaluate the dependent applications and reports that need to be updated and come up with a migration plan. Also, you help the customer take advantage of new SQL Server features to improve performance and resiliency and perform a migration from an old version of SQL Server to Azure SQL Database.

At the end of this hands-on lab, you will be better able to design and build a database migration plan and implement any required application changes associated with changing database technologies.

## Overview

World Wide Importers (WWI) has experienced significant growth in the last few years. In addition to predictable growth, they’ve had a substantial amount of growth in the data they store in their data warehouse. Their data warehouse is starting to show its age, slowing down during extract, transform, and load (ETL) operations and during critical queries. The data warehouse is running on SQL Server 2008 R2 Standard Edition.

The WWI CIO has recently read about new performance enhancements of Azure SQL Database and SQL Server 2017. She is excited about the potential performance improvements related to clustered ColumnStore indexes. She is also hoping that table compression can improve performance and backup times.

WWI is concerned about upgrading their database to Azure SQL Database or SQL Server 2017. The data warehouse has been successful for a long time. As it has grown, it has filled with data, stored procedures, views, and security. WWI wants assurance that if it moves its data store, it won’t run into any incompatibilities with the storage engine of Azure SQL Database or SQL Server 2017.

WWI’s CIO would like a POC of a data warehouse move and proof that the new technology can help ETL and query performance.

## Solution architecture

Below is a diagram of the solution architecture you build in this lab. Please study this carefully, so you understand the whole of the solution as you are working on the various components.

![This solution diagram is divided into Microsoft Azure, and On Premises. Microsoft Azure includes SQL Server 2017 in a VM as an Always On Secondary, and Azure SQL Stretch Database to extend the audit table to Azure. On Premise includes the following elements: API App for vendor connections; Web App for Internet Sales Transactions; ASP.NET Core App for inventory management; SQL Server 2017 OLTP for Always On and JSON store; SSRS 2017 for Reporting of OLTP, Data Warehouse, and Cubes; SSIS 2017 for a Data Warehouse Load; Excel for reporting; SQL Server 2017 Enterprise for a Data Warehouse; and SSAS 2017 for a Data Warehouse. ](./media/preferred-solution-architecture.png "Preferred Solution diagram")

The solution begins with using the Microsoft Data Migration Assistant to perform an assessment to see what potentials issues need to be addressed in upgrading the database to SQL Server 2017 or Azure SQL Database. After correcting any issues, the SQL Server 2008 database is migrated to Azure SQL Database, using the Azure Database Migration Service. Two features of Azure SQL Database, Table Compression and ColumnStore Index, will be applied to demonstrate value and performance improvements from the upgrade. For the ColumnStore Index, a new table based on the existing FactResellerSales table will be created, and a ColumnStore index applied. Next, the Oracle XE database supporting the application will be migrated to an on-premises SQL Server 2017 Enterprise instance using SQL Server Migration Assistant (SSMA) 7.x for Oracle. Once the Oracle database has been migrated, the Northwind MVC application will be updated, so it targets SQL Server 2017 instead of Oracle. The entity models are updated against SQL Server, and code updates are made to use the new Entity Framework context based on SQL Server. Corrections to stored procedures are made due to differences in how stored procedures are accessed in Oracle versus SQL Server. Azure SQL Stretch Database will be used to extend the audit log table to Azure, helping to prevent the recurrence of a system crash caused by the audit log table filling up.

## Requirements

- Microsoft Azure subscription must be pay-as-you-go or MSDN.
  - Trial subscriptions will not work.
- A virtual machine configured with:
  - Visual Studio 2019 Community
  
  **Contents**

- Data Platform upgrade and migration hands-on lab step-by-step](#data-platform-upgrade-and-migration-hands-on-lab-step-by-step)
  - Abstract and learning objectives](#abstract-and-learning-objectives)
  - Overview
  - Solution architecture
  - Requirements
  - Exercise 1: Configure SQL Server instances
    - Task 1: Connect to the SqlServer2008 VM
    - Task 2: Install AdventureWorks sample database
    - Task 3: Update SQL Server settings using Configuration Manager
    - Task 4: Connect to the SqlServer2017 VM
    - Task 5: Update SQL Server settings using Configuration Manager 2017
  - Exercise 2: Migrate SQL Server to Azure SQL Database using DMS
    - Task 1: Assess the on-premises database
    - Task 2: Migrate the database schema
    - Task 3: Create a migration project
    - Task 4: Run the migration
    - Task 5: Verify data migration
  - Exercise 3: Post upgrade enhancement
    - Task 1: Table compression
    - Task 2: Clustered ColumnStore index
  - Exercise 4: Setup Oracle 11g Express Edition
    - Task 1: Install Oracle XE
    - Task 2: Install Oracle Data Access components
    - Task 3: Install SQL Server Migration Assistant for Oracle
    - Task 4: Install dbForge Fusion tool
    - Task 5: Create the Northwind database in Oracle 11g XE
    - Task 6: Configure the Starter Application to use Oracle
  - Exercise 5: Migrate the Oracle database to SQL Server 2017
    - Task 1: Migrate the Oracle database to SQL Server 2017 using SSMA
  - Exercise 6: Migrate the Application
    - Task 1: Create a new Entity Model against SQL Server
    - Task 2: Modify Application Code
