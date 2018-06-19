![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Data platform upgrade and migration
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
March 2018
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.
© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Data platform upgrade and migration hands-on lab unguided](#data-platform-upgrade-and-migration-hands-on-lab-unguided)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution Architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Exercise 1: Deploy SQL Server instances](#exercise-1-deploy-sql-server-instances)
        - [Task 1: Provision an Azure VM](#task-1-provision-an-azure-vm)
            - [Tasks to Complete](#tasks-to-complete)
            - [Exit Criteria](#exit-criteria)
        - [Task 2: Allow remote systems to connect with the SQL Server VM](#task-2-allow-remote-systems-to-connect-with-the-sql-server-vm)
            - [Tasks to Complete](#tasks-to-complete-1)
            - [Exit Criteria](#exit-criteria-1)
        - [Task 3: Install SQL Server 2017](#task-3-install-sql-server-2017)
            - [Tasks to Complete](#tasks-to-complete-2)
            - [Exit Criteria](#exit-criteria-2)
        - [Task 4: Configure the VM as an application server](#task-4-configure-the-vm-as-an-application-server)
            - [Tasks to Complete](#tasks-to-complete-3)
            - [Exit Criteria](#exit-criteria-3)
        - [Task 5: Install SQL Server 2008 R2](#task-5-install-sql-server-2008-r2)
            - [Tasks to Complete](#tasks-to-complete-4)
            - [Exit Criteria](#exit-criteria-4)
        - [Task 6: Open port 1433 on the Windows Firewall of the VM](#task-6-open-port-1433-on-the-windows-firewall-of-the-vm)
            - [Tasks to Complete](#tasks-to-complete-5)
            - [Exit Criteria](#exit-criteria-5)
        - [Task 7: Install the AdventureWorks sample database](#task-7-install-the-adventureworks-sample-database)
            - [Tasks to Complete](#tasks-to-complete-6)
            - [Exit Criteria](#exit-criteria-6)
        - [Task 8: Update SQL Server service accounts using Configuration Manager](#task-8-update-sql-server-service-accounts-using-configuration-manager)
            - [Tasks to Complete](#tasks-to-complete-7)
            - [Exit Criteria](#exit-criteria-7)
    - [Exercise 2: Perform an assessment for a move to Azure SQL Database](#exercise-2-perform-an-assessment-for-a-move-to-azure-sql-database)
        - [Task 1: Perform an assessment](#task-1-perform-an-assessment)
            - [Tasks to Complete](#tasks-to-complete-8)
            - [Exit Criteria](#exit-criteria-8)
    - [Exercise 3: Upgrade the SQL Server 2008 database to SQL Server 2017](#exercise-3-upgrade-the-sql-server-2008-database-to-sql-server-2017)
        - [Task 1: Create a shared folder for the database migration](#task-1-create-a-shared-folder-for-the-database-migration)
            - [Tasks to Complete](#tasks-to-complete-9)
            - [Exit Criteria](#exit-criteria-9)
        - [Task 2: Migrate using Data Migration Assistant](#task-2-migrate-using-data-migration-assistant)
            - [Tasks to Complete](#tasks-to-complete-10)
    - [Exercise 4: Post Upgrade Enhancement](#exercise-4-post-upgrade-enhancement)
        - [Task 1: Table Compression](#task-1-table-compression)
            - [Tasks to Complete](#tasks-to-complete-11)
            - [Exit Criteria](#exit-criteria-10)
        - [Task 2: Clustered ColumnStore Index](#task-2-clustered-columnstore-index)
            - [Tasks to Complete](#tasks-to-complete-12)
            - [Exit Criteria](#exit-criteria-11)
    - [Exercise 5: Setup Oracle 11g Express Edition](#exercise-5-setup-oracle-11g-express-edition)
        - [Task 1: Install Oracle XE](#task-1-install-oracle-xe)
            - [Tasks to Complete](#tasks-to-complete-13)
            - [Exit Criteria](#exit-criteria-12)
        - [Task 2: Install Oracle Data Access Components](#task-2-install-oracle-data-access-components)
            - [Tasks to Complete](#tasks-to-complete-14)
            - [Exit Criteria](#exit-criteria-13)
        - [Task 3: Install SQL Server Migration Assistant (SSMA) 7.x for Oracle](#task-3-install-sql-server-migration-assistant-ssma-7x-for-oracle)
            - [Tasks to Complete](#tasks-to-complete-15)
            - [Exit Criteria](#exit-criteria-14)
        - [Task 4: Install dbForge Fusion tool (trial version)](#task-4-install-dbforge-fusion-tool-trial-version)
            - [Tasks to Complete](#tasks-to-complete-16)
            - [Exit Criteria](#exit-criteria-15)
        - [Task 5: Create the Northwind database in Oracle 11g XE](#task-5-create-the-northwind-database-in-oracle-11g-xe)
            - [Tasks to Complete](#tasks-to-complete-17)
            - [Exit Criteria](#exit-criteria-16)
        - [Task 6: Configure the Starter Application to use Oracle](#task-6-configure-the-starter-application-to-use-oracle)
            - [Tasks to Complete](#tasks-to-complete-18)
            - [Exit Criteria](#exit-criteria-17)
    - [Exercise 6: Migrate the Oracle database to SQL Server 2017](#exercise-6-migrate-the-oracle-database-to-sql-server-2017)
        - [Task 1: Migrate the Oracle database to SQL Server 2017 using SSMA](#task-1-migrate-the-oracle-database-to-sql-server-2017-using-ssma)
            - [Tasks to Complete](#tasks-to-complete-19)
            - [Exit Criteria](#exit-criteria-18)
    - [Exercise 7: Migrate the application](#exercise-7-migrate-the-application)
        - [Task 1: Create a new Entity Model against SQL Server](#task-1-create-a-new-entity-model-against-sql-server)
            - [Tasks to Complete](#tasks-to-complete-20)
            - [Exit Criteria](#exit-criteria-19)
        - [Task 2: Modify Application Code](#task-2-modify-application-code)
            - [Tasks to Complete](#tasks-to-complete-21)
            - [Exit Criteria](#exit-criteria-20)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Task 1: Delete the resource group](#task-1-delete-the-resource-group)

<!-- /TOC -->


# Data platform upgrade and migration hands-on lab unguided

## Abstract and learning objectives

This hands-on lab is designed to help attendees better understand how to build a Proof-of-Concept (POC) and conduct a site analysis for a customer to compare cost, performance, and level of effort required to migrate from Oracle to SQL Server. You will evaluate the dependent applications and reports that will need to be updated, and come up with a migration plan. In addition, attendees will help the customer take advantage of new SQL Server features to improve performance and resiliency, as well as explore ways to migrate from an old version of SQL Server to the newest version and consider the impact of migrating from on-premises to the cloud.

Learning Objectives:

-   Migrate from Oracle to SQL Server using SQL Server Migration Assistant

-   Migrate between different SQL Server editions using Data Migration Assistant

-   Use advanced SQL Server features, such as JavaScript Object Notation (JSON) data store, table compression, Transparent Data Encryption, and clustered ColumnStore indexing

-   Consider the steps required to update existing applications to use the new data platform

-   Analyze and improve database performance

-   Implement high availability using Stretch Database and AlwaysOn Availability Groups

## Overview

World Wide Importers (WWI) has experienced significant growth in the last few years. In addition to predictable growth, they've had a substantial amount of growth in the data they store in their data warehouse. Their data warehouse is starting to show its age; slowing down during extract, transform, and load (ETL) operations and during critical queries. It was built on SQL Server 2008 R2 Standard Edition.

The WWI CIO has recently read about new performance enhancements of SQL Server 2017. She is excited about clustered ColumnStore indexes. In addition, she has chosen to upgrade to Enterprise Edition. She's hoping that table compression will improve performance, backup times, and lessen the load on the Storage Area Network (SAN).

WWI is concerned about upgrading their database to SQL Server 2017 or Azure SQL Database. The data warehouse has been successful for a long time. As it has grown, it has filled with data, stored procedures, views, and security. WWI wants assurance that if it moves its data store, it won't run into any incompatibilities with the storage engine of SQL Server 2017.

WWI's CIO would like a POC of a data warehouse move and proof that the new technology will help ETL and query performance.

## Solution Architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully, so you understand the whole of the solution as you are working on the various components.

![This solution diagram is divided in to Microsoft Azure, and On Premises. Microsoft Azure includes SQL Server 2017 in a VM as an Always On Secondary, and Azure SQL Data Warehouse for a stretch table. On Premise includes the following elements: API App for vendor connections; Web App for Internet Sales Transactions; ASP.NET Core App for inventory management; SQL Server 2017 OLTP for Always On and JSON store; SSRS 2017 for Reporting of OLTP, Data Warehouse, and Cubes; SSIS 2017 for a Data Warehouse Load; Excel for reporting; SQL Server 2017 Enterprise for a Data Warehouse; and SSAS 2017 for a Data Warehouse. ](images/Hands-onlabunguided-Dataplatformupgradeandmigrationimages/media/image2.png "Preferred Solution diagram")

The solution begins with using the Microsoft Data Migration Assistant to perform an assessment to see what potentials issues need to be addressed in upgrading the database to SQL Server 2017 or Azure SQL Database. After correcting any issues, the SQL Server 2008 database is migrated to SQL Server 2017 Enterprise, using Data Migration Assistant. A shared folder is created for storing a backup of the database, which is used by Data Migration Assistant to move the database to SQL Server 2017. Two new features of SQL Server 2017, Table Compression and ColumnStore Index, will be applied to demonstrate vale from the upgrade. For the ColumnStore Index, a new table based on the existing FactResellerSales table will be created, and a ColumnStore index applied. Next, the Oracle XE database supporting the application will be migrating to SQL Server 2017 Enterprise using SQL Server Migration Assistant (SSMA) 7.x for Oracle. Once the Oracle database has been migrated, the NorthwindMVC application is updated, so it targets SQL Server 2017 instead of Oracle. The entity models are updated against SQL Server, and code updates are made to use the new Entity Framework context based on SQL Server. Corrections to stored procedures are made due to differences in how stored procedures are accessed in Oracle versus SQL Server.

## Requirements

-   Microsoft Azure subscription must be pay-as-you-go or MSDN.

    -   Trial subscriptions will not work.

-   A virtual machine configured with:

    -   Visual Studio Community 2017 or later

    -   Azure SDK 2.9 or later (Included with Visual Studio 2017)


## Exercise 1: Deploy SQL Server instances

Duration: 60 minutes

In this exercise, you will create an Azure VM that contains instances of both SQL Server 2008 and SQL Server 2017.

### Task 1: Provision an Azure VM

In this task, you will provision a virtual machine (VM) in Azure which can host instances of both SQL Server 2008 and SQL Server 2017. The VM with use the Windows Server 2012 R2 Datacenter image. Note, we are using an older version of Windows Server because SQL Server 2008 R2 is not supported on Windows Server 2016.

#### Tasks to Complete

-   Provision a VM of Windows Server 2012 R2.

    -   Be sure to use an admin account user name of holuser, with a password of Password.1!!.

#### Exit Criteria

-   You are able to Remote Desktop into the VM.

### Task 2: Allow remote systems to connect with the SQL Server VM

In this task, you will create a network security rule for the VM that allows remote systems to connect with SQL Server.

#### Tasks to Complete

-   Add a network security rule allowing connectivity with SQL Server on port 1433.

#### Exit Criteria

-   Port 1433 is open on the VM.

### Task 3: Install SQL Server 2017

In this task, you will install SQL Server 2017 and Microsoft SQL Server Management Studio (SSMS) on the VM.

#### Tasks to Complete

-   Install SQL Server 2017 Developer Edition on the VM

    -   <https://www.microsoft.com/sql-server/sql-server-downloads>

    -   Ensure the SQL Server Database Engine runs under the holuser Windows account.

    -   Enable Mixed Mode Authentication

        -   Ensure the SA password is set to Password.1!!

    -   Make sure to add the holuser to the SQL Server Administrators group.

-   Install SQL Server Management Studio (SSMS) 17.

#### Exit Criteria

You have successfully installed SQL Server 2017 Developer Edition and SSMS 17 on the VM.

### Task 4: Configure the VM as an application server

In this task, you will configure the VM to be an application server. This is necessary to install the required .NET components on the server prior to installing SQL Server 2008 R2.

#### Tasks to Complete

-   Add the Application Server role the VM, and include .NET Framework 3.5 features

#### Exit Criteria

-   The VM is configured as an Application Server, with the necessary .NET 3.5 features installed.

### Task 5: Install SQL Server 2008 R2

In this task, you will install SQL Server 2008 R2 on the VM.

#### Tasks to Complete

-   Install SQL Server 2008 R2 Express with Advanced Services on the VM

    -   <https://www.microsoft.com/download/details.aspx?id=30438>

    -   Be sure that the SQL Database Engine runs under the holuser Windows account.

    -   Enable Mixed Mode Authentication

        -   Ensure the SA password is set to Password.1!!

    -   Ensure the holuser account is added to the SQL Server Administrators group

#### Exit Criteria

-   You have successfully installed both versions of SQL Server on the same VM.

### Task 6: Open port 1433 on the Windows Firewall of the VM

In this task, you will add rules to the VM's Windows firewall to allow access to SQL Server via port 1433 by other machines.

#### Tasks to Complete

-   Add an inbound port rule to the Windows Firewall on the VM to allow communication over TCP port 1433.

#### Exit Criteria

-   Port 1433 is open on the VM's firewall

### Task 7: Install the AdventureWorks sample database

In this task, you will install the AdventureWorks database in SQL 2008 R2, as the source database to migrate.

#### Tasks to Complete

-   Download the AdventureWorks 2008R2 DW script from: <https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2008r2>

-   Execute instawdwdb.sql against the SQL Server 2008 instance on your VM

-   Rename the database to WorldWideImporters

#### Exit Criteria

-   You have successfully created the WorldWideImporters database.

### Task 8: Update SQL Server service accounts using Configuration Manager

In this task, you will ensure the SQL Server services in running under the holuser account, and that remote TCP/IP connections are allowed.

#### Tasks to Complete

-   Set the services account for the SQL Server service (in both the SQL Server 2008 and 2017 instances) to use the holuser Windows account.

-   Ensure that remote TCP/IP connections are allowed to the SQL Server 2017 instance.

-   Enable the SA login with the password Password.1!!

-   Create a new, empty database called Northwind in the SQL Server 2017 instance

#### Exit Criteria

-   You have successfully created the Northwind target database in SQL Server 2017, and can access this SQL Server using SQL Server credentials.

## Exercise 2: Perform an assessment for a move to Azure SQL Database

Duration: 15 minutes

World Wide Importers would like an assessment to see what potential issues they would have to address in moving their database to Azure SQL Database.

### Task 1: Perform an assessment

#### Tasks to Complete

-   Use the Data Migration Assistant to perform an assessment for migrating the WorldWideImporters database to Azure SQL Database.

#### Exit Criteria

-   You can review the assessment results.

## Exercise 3: Upgrade the SQL Server 2008 database to SQL Server 2017

Duration: 30 minutes

World Wide Importers would like a Proof of Concept (POC) that moves their database to SQL Server 2017 Enterprise. They would like to know about any incompatible features that will block their eventual production move.

### Task 1: Create a shared folder for the database migration

In this task, you are going to create a shared folder that is accessible by both the source and target databases for the migration. This folder will store a backup of the database used for the migration process.

#### Tasks to Complete

-   Create a common shared folder on the VM for use as the "Shared location" used by Data Migration Assistant.

#### Exit Criteria

-   You have a folder accessible by both SQL Servers for Data Migration Assistant to use as a shared location during migration.

### Task 2: Migrate using Data Migration Assistant

#### Tasks to Complete

-   Migrate the WorldWideImporters database from SQL Server 2008 to SQL Server 2017 using the Data Migration Assistant.

    -   Be sure to use Windows Authentication

    -   Uncheck Encrypt connection and Trusted server certificate

Exit Criteria

-   Your results indicate a successful upgrade.
    
    ![WorldWideImporters is highlighted under Databases in the Data Migration Assistant.](images/Hands-onlabunguided-Dataplatformupgradeandmigrationimages/media/image16.png "Migrate the WorldWideImporters database")

## Exercise 4: Post Upgrade Enhancement

Duration: 20 minutes

In this exercise, you will confirm that the POC was successful. To demonstrate value from the upgrade, the new features of SQL Server 2017 (Table Compression and ColumnStore Index) will be enabled to immediately show benefit.

### Task 1: Table Compression

#### Tasks to Complete

-   Take note of the data space and index space used by the FactInternetSales table.

-   Enable Table Compression (page type) on the FactInternetSales table.

#### Exit Criteria

-   You observe a change in the data space and index space.

### Task 2: Clustered ColumnStore Index

In this task, you will create a new table based on the existing FactResellerSales table and apply a ColumnStore index.

#### Tasks to Complete

-   Create a new table called ColumnStore\_FactResellerSales from the FactResellersSales table.

-   Create a clustered ColumnStore index on the new table.

-   Query the tables and compare the execution plan query costs, batch percentage.

-   Query the tables with statistic IO on and compare the logical and lob logical reads.

#### Exit Criteria

-   You observe a performance improvement.
    
    ![Various performance improvements are highlighted on the Messages tab.](images/Hands-onlabunguided-Dataplatformupgradeandmigrationimages/media/image17.png "Observe improvements")

## Exercise 5: Setup Oracle 11g Express Edition

Duration: 45 minutes

In this exercise, you will install Oracle XE on your Lab VM, load a sample database supporting an application, and then migrate the database to SQL Server 2017.

### Task 1: Install Oracle XE

#### Tasks to Complete

-   Install Oracle XE on your lab VM from: <http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html>

-   Be sure to set the SYS and SYSTEM password to Password.1!!

#### Exit Criteria

-   You have successfully installed Oracle.

### Task 2: Install Oracle Data Access Components

#### Tasks to Complete

-   Install Oracle Data Access Components (ODAC) from <http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html>

-   Be sure the install the 64-bit ODAC components

-   Ensure you configure ODP.NET and Oracle Providers for ASP.NET at machine-wide level

#### Exit Criteria

-   You have successfully installed ODAC.

### Task 3: Install SQL Server Migration Assistant (SSMA) 7.x for Oracle

#### Tasks to Complete

-   Install SSMA 7.x from <https://www.microsoft.com/en-us/download/details.aspx?id=54258>

#### Exit Criteria

-   You have successfully installed SSMA on your Lab VM.

### Task 4: Install dbForge Fusion tool (trial version)

In this task, you will install a third-party extension to Visual Studio to enable interaction with, and script execution for, the Oracle database in Visual Studio 2017 Community Edition. This step is necessary because the Oracle Developer Tools extension does not currently work with Visual Studio 2017 Community Edition.

#### Tasks to Complete

-   Install dbForge Fusion for Oracle Professional trial version from <https://www.devart.com/dbforge/oracle/fusion/download.html>

#### Exit Criteria

-   The dbForge Fusion extension is successfully installed in Visual Studio 2017.

### Task 5: Create the Northwind database in Oracle 11g XE

In this task, you will create a connection to the Oracle database on your Lab VM, and create a database called Northwind.

#### Tasks to Complete

-   Download the starter files from <http://bit.ly/2rwxnwW>. (Note the URL is cases sensitive)

-   Using Database Explorer provided under the Fusion menu installed by dbForge Fusion in Visual Studio, create a connection to the Oracle database.

-   Run the SQL scripts in order provided under the Oracle Scripts folder of the starter.

#### Exit Criteria

-   You have created the Northwind database with schema and data in Oracle.

### Task 6: Configure the Starter Application to use Oracle

In this task, you will add the necessary configuration to the NorthwindMVC solution to connect to the Oracle database you created in the previous task.

#### Tasks to Complete

-   Modify the web.config of the NorthwindMVC web app so the OracleConnectionString points to your Oracle instance of Northwind.

#### Exit Criteria

-   You can run the application and see the Dashboard from Northwind Traders load.\
    
    ![This is a screenshot of the Northwind Traders Dashboard.](images/Hands-onlabunguided-Dataplatformupgradeandmigrationimages/media/image18.png "View the dashboard")

## Exercise 6: Migrate the Oracle database to SQL Server 2017

Duration: 30 minutes

In this exercise, you will migrate the Oracle database into SQL Server 2017 using SSMA.

### Task 1: Migrate the Oracle database to SQL Server 2017 using SSMA

#### Tasks to Complete

-   Use SSMA to migrate the schema and data from the NW database on Oracle to the Northwind database in SQL Server 2017.

    -   Fix data type errors reported during schema conversion

    -   In SQL Server 2017, Microsoft now by default requires that all type of assemblies (SAFE, EXTERNAL\_ACCESS, UNSAFE) are authorized for UNSAFE access. Add the SSMA4OracleSQLServerCollections.NET and SSMA4OracleSQLServerExtensions.NET assemblies to the SQLCLR whitelist for the SQL Server 2017 instance.

    -   Execute the script below in SSMS for each assembly. Note: The binary value for each assembly can be obtained by saving the assemblies as a script in SSMA.
    ```
    USE master;

    GO

    DECLARE @clrName nvarchar(4000) = '[INSERT ASSEMBLY NAME]'

    DECLARE @asmBin varbinary(max) = [INSERT BINARY];

    DECLARE @hash varbinary(64);

    SELECT @hash = HASHBYTES('SHA2_512', @asmBin);

    EXEC sys.sp_add_trusted_assembly @hash = @hash, @description = @clrName;
    ```

#### Exit Criteria

-   Your Northwind database in SQL Server has the schema objects and the tables have data.

## Exercise 7: Migrate the application

Duration: 15 minutes

In this exercise, you will modify the NorthwindMVC application, so it targets SQL Server 2017 instead of Oracle.

### Task 1: Create a new Entity Model against SQL Server

#### Tasks to Complete

-   Modify the web.config so that SqlServerConnectionString is correct for your environment.

-   Delete the existing entity model present in the files under the Data folder in Solution explorer.

-   Connect to the SQL Server database through Server Explorer in Visual Studio.

-   Create a new Code First model against the SQL Server database.

#### Exit Criteria

-   You have successfully generated a new model.

### Task 2: Modify Application Code

#### Tasks to Complete

-   Change the DataContext to use your SqlServerConnectionString.

-   Within HomeController.cs uncomment the SQL Server specific code and comment out the Oracle specific code.

-   Modify SALESBYYEAR.cs so that YEAR is of type int and SUBTOTAL is of type decimal.

-   Modify SalesByYearViewModel.cs so that Year is of type int.

-   Run the SALES\_BY\_YEAR\_fix.sql

#### Exit Criteria

-   You can run the application and see the dashboard showing correctly, this time with data coming from SQL Server. 
    
    ![This is a screenshot of the Northwind Traders Dashboard.](images/Hands-onlabunguided-Dataplatformupgradeandmigrationimages/media/image18.png "View the dashboard")

## After the hands-on lab 

Duration: 10 mins

In this exercise, attendees will deprovision any Azure resources that were created in support of the lab. You should follow all steps provided after attending the Hands-on lab.

### Task 1: Delete the resource group

1.  Using the Azure portal, navigate to the Resource group you used throughout this hands-on lab by selecting Resource groups in the left menu.

2.  Search for the name of your research group, and select it from the list.

3.  Select Delete in the command bar, and confirm the deletion by re-typing the Resource group name, and selecting Delete.

You should follow all steps provided *after* attending the Hands-on lab.