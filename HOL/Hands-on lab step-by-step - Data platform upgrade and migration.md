![](images/HeaderPic.png "Microsoft Cloud Workshops")

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

- [Data platform upgrade and migration hands-on lab step-by-step](#data-platform-upgrade-and-migration-hands-on-lab-step-by-step)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution Architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Exercise 1: Deploy SQL Server instances](#exercise-1-deploy-sql-server-instances)
        - [Task 1: Provision an Azure VM](#task-1-provision-an-azure-vm)
        - [Task 2: Allow remote systems to connect with the SQL Server VM](#task-2-allow-remote-systems-to-connect-with-the-sql-server-vm)
        - [Task 3: Install SQL Server 2017](#task-3-install-sql-server-2017)
        - [Task 4: Configure the VM as an application server](#task-4-configure-the-vm-as-an-application-server)
        - [Task 5: Install SQL Server 2008 R2](#task-5-install-sql-server-2008-r2)
        - [Task 6: Open port 1433 on the Windows Firewall of the SqlServerDw VM](#task-6-open-port-1433-on-the-windows-firewall-of-the-sqlserverdw-vm)
        - [Task 7: Install the AdventureWorks sample database](#task-7-install-the-adventureworks-sample-database)
        - [Task 8: Update SQL Server service accounts using Configuration Manager](#task-8-update-sql-server-service-accounts-using-configuration-manager)
    - [Exercise 2: Perform an assessment for a move to Azure SQL Database](#exercise-2-perform-an-assessment-for-a-move-to-azure-sql-database)
        - [Task 1: Perform an assessment](#task-1-perform-an-assessment)
    - [Exercise 3: Upgrade the SQL Server 2008 database to SQL Server 2017](#exercise-3-upgrade-the-sql-server-2008-database-to-sql-server-2017)
        - [Task 1: Create a shared folder for the database migration](#task-1-create-a-shared-folder-for-the-database-migration)
        - [Task 2: Migrate using Data Migration Assistant](#task-2-migrate-using-data-migration-assistant)
    - [Exercise 4: Post upgrade enhancement](#exercise-4-post-upgrade-enhancement)
        - [Task 1: Table compression](#task-1-table-compression)
        - [Task 2: Clustered ColumnStore index](#task-2-clustered-columnstore-index)
    - [Exercise 5: Setup Oracle 11g Express Edition](#exercise-5-setup-oracle-11g-express-edition)
        - [Task 1: Install Oracle XE](#task-1-install-oracle-xe)
        - [Task 2: Install Oracle Data Access components](#task-2-install-oracle-data-access-components)
        - [Task 3: Install SQL Server Migration Assistant (SSMA) 7.x for Oracle](#task-3-install-sql-server-migration-assistant-ssma-7x-for-oracle)
        - [Task 4: Install dbForge Fusion tool (trial version)](#task-4-install-dbforge-fusion-tool-trial-version)
        - [Task 5: Create the Northwind database in Oracle 11g XE](#task-5-create-the-northwind-database-in-oracle-11g-xe)
        - [Task 6: Configure the Starter Application to use Oracle](#task-6-configure-the-starter-application-to-use-oracle)
    - [Exercise 6: Migrate the Oracle database to SQL Server 2017](#exercise-6-migrate-the-oracle-database-to-sql-server-2017)
        - [Task 1: Migrate the Oracle database to SQL Server 2017 using SSMA](#task-1-migrate-the-oracle-database-to-sql-server-2017-using-ssma)
    - [Exercise 7: Migrate the Application](#exercise-7-migrate-the-application)
        - [Task 1: Create a new Entity Model against SQL Server](#task-1-create-a-new-entity-model-against-sql-server)
        - [Task 2: Modify Application Code](#task-2-modify-application-code)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Task 1: Delete the resource group](#task-1-delete-the-resource-group)

<!-- /TOC -->


# Data platform upgrade and migration hands-on lab step-by-step

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

![This solution diagram is divided in to Microsoft Azure, and On Premises. Microsoft Azure includes SQL Server 2017 in a VM as an Always On Secondary, and Azure SQL Data Warehouse for a stretch table. On Premise includes the following elements: API App for vendor connections; Web App for Internet Sales Transactions; ASP.NET Core App for inventory management; SQL Server 2017 OLTP for Always On and JSON store; SSRS 2017 for Reporting of OLTP, Data Warehouse, and Cubes; SSIS 2017 for a Data Warehouse Load; Excel for reporting; SQL Server 2017 Enterprise for a Data Warehouse; and SSAS 2017 for a Data Warehouse.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image2.png "Preferred Solution diagram")

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

1.  In a web browser, navigate to the Azure portal (<https://portal.azure.com>).

2.  Select +Create a resource, and in the Search the marketplace box type "windows server 2012." Select the Windows Server 2012 R2 Datacenter VM image from the list. 
    
    ![+ Create a resource is highlighted on the left side of the Azure portal, and at right, windows server 2012 and Windows Server 2012 R2 Datacenter are highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image16.png "Azure portal")

3.  In the blade that comes up, ensure the deployment model is set to Resource Manager, and select Create. 
    
    ![Resource Manager is listed below Select a deployment model, and the Create button appears below that.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image4.png "Select a deployment model")

4.  On the Create virtual machine blade, enter the following:

    -   Name: Enter SqlServerDw

    -   VM disk type: Select SSD

    -   User name: Enter holuser

    -   Password: Enter Password.1!!, then re-enter it in the confirm password text box

    -   Subscription: Select the subscription you are using for this lab

    -   Resource group: Select Use existing, then choose the hands-on-labs resource group

    -   Location: Select the location you used previously in this lab 
        
        ![On the Basics blade, the values above are entered in the boxes.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image17.png "Basics blade")

    -   Select OK to move on to the Size blade.

5.  On the Choose a size blade, select View all. 
    
    ![On the Choose a size blade, View all is highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image18.png "Choose a size blade")

6.  Choose a VM size. For this lab, it does not need to be very large, so selecting DS1\_V2 Standard is a good option. 

    ![DS1\_V2, the second of three boxes, is highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image19.png "Choose a VM size")

7.  Select Select on the Choose a size blade.

8.  On the Settings blade, select OK to accept the defaults and move on to the Summary.

9.  On the Summary blade, select Create to provision the VM. 

    ![In this screenshot of the Create blade, various information appears below Offer details, Summary, and Terms of use. A Create button appears at the bottom of the blade.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image20.png "Select Create")

10. It can take 10+ minutes to provision the new VM.

### Task 2: Allow remote systems to connect with the SQL Server VM

In this task, you will create a network security rule for the SqlServerDw VM that allows remote systems to connect with SQL Server.

1.  In the Azure portal, navigate to the newly provisioned SqlServerDw VM by selecting Resource groups from the left-hand menu, then selecting the hands-on-lab resource group from the list. 
    
    ![Resource groups is highlighted on the left side of the Azure portal, and at right, hands and hands-on-labs are highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image8.png "Azure portal")

2.  From the list of available resources, select the SqlServerDw virtual machine. 
    
    ![SqlServerDw is highlighted in the Name list.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image21.png "Select the SqlServerDw virtual machine")

3.  On the SqlServerDw blade, select Networking under Settings from the left-hand menu. 
    
    ![Networking is selected and highlighted under Settings.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image22.png "Select Networking")

4.  On the Networking blade, select Add inbound port rule. 
    
    ![Add inbound port rule is highlighted on the Networking blade.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image23.png "Select Add inbound port rule")

5.  In the Add inbound security rule dialog, select MS SQL for the Service, then select OK. 
    
    ![MS SQL is highlighted under Service.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image24.png "Select MS SQL")

6.  Once the new inbound port rule is created, you will see it appear in the Inbound port rules list. 
    
    ![MS\_SQL is highlighted in the Inbound port rules list.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image25.png "View the Inbound port rules list")

### Task 3: Install SQL Server 2017

In this task, you will create a remote desktop protocol (RDP) connection to the SqlServerDw VM, and install SQL Server 2017 and Microsoft SQL Server Management Studio (SSMS) on the SqlServerDw VM.

1.  From the overview blade of your SqlServerDw VM in the Azure portal, select Connect, and follow the prompts to login to the VM, following the same steps used to connect to your Lab VM and disable IE Enhanced Security Configuration in Before the hands-on lab, Task 2

    ![Connect is highlighted on the overview blade.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image26.png "Select Connect")

2.  From a web browser on the SqlServerDw VM, download SQL Server 2017 Developer Edition by navigating to <https://www.microsoft.com/sql-server/sql-server-downloads>, and selecting the Download now link under Developer edition. 

    ![The Download now link is highlighted under Developer edition at https://www.microsoft.com/sql-server/sql-server-downloads.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image27.png "Download SQL Server 2017 Developer Edition")

3.  Run the downloaded installer.

4.  In the install dialog, select Custom as the installation type on the first screen. 

    ![In the installation dialog box, Custom is selected and highlighted under Select an installation type.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image28.png "Select Custom")

5.  On the next screen, leave the Media Location set to the default value, and select Install. 
    
    ![The default value (C:\\SQLServer2017Media) is listed under Media Location, and Install is highlighted at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image29.png "Install with the default value")

6.  Once the necessary components are downloaded, the SQL Server Installation Center will open. Select Installation on the left-hand menu, then select New SQL Server stand-alone installation or add features to an existing installation. 
    
    ![Installation is highlighted on the left side of the SQL Server Installation Center dialog box. At right, New SQL Server stand-alone installation or add features to an existing installation is highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image30.png "Choose your installation")

7.  On the Product Key screen, select Developer under Specify a free edition, and select Next. 
    
    ![Developer displays under Specify a free edition.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image31.png "Specify the edition")

8.  On the License Terms screen, check the box to accept the license terms, and select Next.

9.  Select Next on each the following screens to accept the defaults, until you get to the Feature Selection screen.

10. On the Feature Selection screen, check the box next to Database Engine Services, and select Next. 
    
    ![Database Engine Services is selected and highlighted under Instance Features on the Feature Selection screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image32.png "Select Database Engine Services")

11. On the Instance Configuration screen, leave Default instance selected, and select Next.

12. Accept the default values on the Server Configuration screen, and select Next.

13. On the Database Engine Configuration screen:

    -   Select Mixed Mode

    -   Enter Password.1!! for the sa password.

    -   Click the Add Current User button to make the holuser windows account a SQL Server administrator. 
    
    ![The information above is highlighted on the Database Engine Configuration screen, including the Add Current User button under Specify SQL Server administrators.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image33.png "Specify Database Engine Configuration settings")

    -   Select Next.

14. Select Install on the Ready to Install screen to start the installation.

15. Select Close when the installation is complete.

16. Back in the SQL Server Installation Center dialog, select Install SQL Server Management Tools on the Installation tab. 
    
    ![Installation is highlighted on the left side of the SQL Server Installation Center dialog box. At right, Install SQL Server Management Tools is highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image34.png "Select Install SQL Server Management Tools")

17. In the browser window that opens, scroll down and select the Download SQL Server Management Studio 17.x link, then run the installer. 
    
    ![Download SQL Server Management Studio 17.5 is highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image35.png "Browser window")

18. In the install window that appears, select Install to complete the installation. 
    
    ![Install is highlighted at the bottom of the Microsoft SQL Server Management Studio installation window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image36.png "Microsoft SQL Server Management Studio Welcome page")

19. Close the SQL Server Management Studio (SSMS) installer once setup is completed.

20. Close the SQL Server Installation Center dialog.

### Task 4: Configure the VM as an application server

In this task, you will configure the SqlServerDw VM to be an application server. This is necessary to install the required .NET components on the server prior to installing SQL Server 2008 R2.

1.  On your SqlServerDw VM, launch the Server Manager by selecting its icon on the Windows task bar. 
    
    ![The Server Manager icon is highlighted on the VM taskbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image37.png "Open Server Manager")

2.  From the Server Manager Dashboard, select Add roles and features. 
    
    ![Add roles and features is highlighted on the Server Manager dashboard.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image38.png "Select Add roles and features")

3.  Select Next on the Before You Begin screen.

4.  Select Role-based or feature-based installation for the installation type, and select Next. 
    
    ![Role-based or feature-based installation is selected and highlighted under Select installation type.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image39.png "Select Role-based or feature-based installation ")

5.  Accept the default selection on the Select destination server screen, and select Next. 
    
    ![Select a server from the server pool is selected on the Select a destination server screen, and the default value (SqlServerDW) appears in the Server Pool box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image40.png "Accept the default selection")

6.  On the Select server roles screen, select Application Server, and select Next. 
    
    ![Application Server is selected and highlighted on the Select server roles screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image41.png "Select Application Server")

7.  On the Select features screen, select .NET Framework 3.5 Features, then select Next. 
    
    ![NET Framework 3.5 Features is selected and highlighted on the Select features screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image42.png "Select .NET Framework 3.5 Features")

8.  Select Next on the Application Server screen.

9.  Accept the default values on the Select role services screen, and select Next.

10. On the Confirm installation selections screen, check the Restart the destination server automatically if required check box. Click Yes on the restart confirmation dialog. 
    
    ![The Yes button is highlighted on the Add Roles and Features Wizard confirmation dialog.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image43.png "Add Roles and Features Wizard dialog box")

11. Select Install. Note: You may see a message about specifying an alternate source path. This can be ignored. 
    
    ![Restart the destination server automatically if required is selected and highlighted on the Confirm installation selections screen, and Install is highlighted at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image44.png "Confirm your installation selections")

12. Close the Add Roles and Features Wizard, once the installation is completed.

13. Close the Server Manager.

### Task 5: Install SQL Server 2008 R2

1.  On the SqlServerDw VM, open a web browser, and navigate to <https://www.microsoft.com/download/details.aspx?id=30438>.

2.  Select the Download link on the page. 
    
    ![Download is selected and highlighted on the https://www.microsoft.com/download/details.aspx?id=30438 page under Microsoft SQL Server 2008 R2 SP2 - Express Edition.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image45.png "Download link")

3.  Download SQL Server 2008 R2 Express with Advanced Services by selecting SQLEXPRADV\_x64\_ENU.exe, then selecting Next. 
    
    ![SQLEXPRADV\_x64\_ENU.exe is highlighted in the list under Choose the download you want.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image46.png "Download SQL Server 2008 R2 Express with Advanced Services")

4.  Run the installer.

5.  In the SQL Server Installation Center window, select New installation or add features to an existing installation. 
    
    ![New installation or add features to an existing installation is highlighted in the SQL Server Installation Center window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image47.png "Select New installation or add features to an existing installation")

6.  In the SQL Server 2008 R2 Setup dialog, accept the license terms on the License Terms screen, and select Next.

7.  Accept the default values on each screen by selecting Next, until you get to the Instance Configuration screen.

8.  On the Installation Configuration screen, select Named Instance, and enter SQLServer2008 for the instance name, then select Next.
    
    ![Instance Configuration is highlighted on the left side of the SQL Server 2008 R2 Setup dialog box. At right, Named instance: SQLServer2008 is highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image48.png "Enter the instance name")

9.  On the Server Configuration screen, select Use the same account for all SQL Server services. 
    
    ![Server Configuration is highlighted on the left side of the SQL Server 2008 R2 Setup dialog box. At right, SQL Server Database Engine is selected on the Service Accounts tab, and Use the same account for all SQL Server services is highlighted at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image49.png "Select Use the same account for all SQL Server services")

10. Click Browse, and enter holuser. Select the holuser account you created with you provisioned the VM, then enter the password, Password.1!!, and select OK.
    
    ![The credentials above are entered in the Use the same account for all SQL Server 2008 R2 services dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image50.png "Enter your credentials")

11. Select Next on the Server Configuration screen.

12. On the Database Engine Configuration screen:

    -   Select Mixed Mode.

    -   Enter Password.1!! for the sa password.

    -   Click the Add Current User button to specify the holuser Windows account as a SQL Server Administrator. 
        
        ![Database Engine Configuration is highlighted on the left side of the SQL Server 2008 R2 Setup dialog box. The information above is highlighted at right, including the Add Current User button under Specify SQL Server administrators.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image51.png "Specify Database Engine Configuration settings")

    -   Select Next.

13. Select Next to accept the default values on the remaining screens to complete the installation.

14. Select Close on the SQL Server 2008 R2 Setup dialog.

### Task 6: Open port 1433 on the Windows Firewall of the SqlServerDw VM

In this task, you will add rules to the SqlServerDw VM's Windows firewall to allow access to SQL Server via port 1433 by other machines.

1.  On the SqlServerDw VM, select Start. 
    
    ![The Start icon is highlighted on the VM taskbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image52.png "Select Start")

2.  Then, select the Search icon in the top right-hand corner of the screen. 
    
    ![The Search icon is highlighted on the right side.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image53.png "Select Search")

3.  In the Search text box, enter wf.msc, then select wf from the list of results. 
    
    ![The wf icon is highlighted in the list of search results.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image54.png "Search on wf.msc")

4.  In the Windows Firewall with Advanced Security dialog, select, then right-click Inbound Rules in the left pane, then click New Rule in the action pane. 
    
    ![New Rule is highlighted in the submenu for Inbound Rules, which is selected in the left pane of the Windows Firewall with Advanced Security dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image55.png "Create a new Inbound Rule")

5.  In the New Inbound Rule Wizard, under Rule Type, select Port, then select Next. 
    
    ![Rule Type is selected and highlighted on the left side of the New Inbound Rule Wizard, and Port is selected and highlighted on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image56.png "Select Port")

6.  In the Protocol and Ports dialog, use the default TCP, and enter 1433 in the Specific local ports text box, the select Next.
    
    ![Protocol and Ports is selected on the left side of the New Inbound Rule Wizard, and 1433 is in the Specific local ports box, which is selected on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image57.png "Select a specific local port")

7.  In the Action dialog, select Allow the connection, and select Next. 
    
    ![Action is selected on the left side of the New Inbound Rule Wizard, and Allow the connection is selected on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image58.png "Specify the action")

8.  In the Profile step, check Domain, Private, and Public, then select Next. 
    
    ![Profile is selected on the left side of the New Inbound Rule Wizard, and Domain, Private, and Public are selected on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image59.png "Select Domain, Private, and Public")

9.  In the Name screen, enter sqlserver, then select Finish. 
    
    ![Profile is selected on the left side of the New Inbound Rule Wizard, and sqlserver is in the Name box on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image60.png "Specify the name")

10. Close the Windows Firewall with Advanced Security window.

### Task 7: Install the AdventureWorks sample database

In this task, you will install the AdventureWorks database in SQL 2008 R2, as the source database to migrate.

1.  On the SqlServerDw VM, open a web browser, and navigate to the GitHub site containing the sample AdventureWorks database at <https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2008r2>.

2.  Scroll down under Assets, and select adventure-works-2008r2-dw.script.zip. 
    
    ![The adventure-works-2008r2-dw-script.zip download link is highlighted under Assets for the sample database.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image61.png "Assets list")

3.  Unzip the downloaded file to a folder you create, called C:\\AdventureWorksSample.

4.  Launch SQL Server Management Studio 17 (SSMS), found under Start-\>Apps-\>Microsoft SQL Server Tools 17. 
    
    ![SQL Server Management Studio 17 (SSMS) is highlighted under Microsoft SQL Server Tools 17.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image62.png "Open SQL Server Management Studio 17 (SSMS)")

5.  In the Connect to Server dialog, enter SqlServerDw\\SQLSERVER2008 in the Server name box, leave Authentication set to Windows Authentication, and select Connect. 
    
    ![In the Connect to Server dialog box, SqlServerDw\\SQLSERVER2008 is highlighted in the Server name box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image63.png "Connect to SqlServerDw\SQLSERVER2008")

6.  Select the Open File icon in SSMS menu bar. 
    
    ![The File icon is highlighted on the SSMS menu bar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image64.png "Select Open File")

7.  In the Open File dialog, browse to the C:\\AdventureWorksSampe\\AdventureWorks 2008R2 Data Warehouse\\ folder, and select the file named instawdwdb.sql. 
    
    ![Local Disk (C:) is selected on the left side of the Open File dialog box, and instawdwdb.sql is selected and highlighted on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image65.png "Select instawdwdb.sql")

8.  Select Open.

9.  Next, select the Tools menu in SSMS, then select Options. 
    
    ![Tools is highlighted on the SSMS menu bar, and Options is highlighted at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image66.png "Select Options")

10. In the Options dialog, expand Text Editor in the tree view on the left, then expand Transact-SQL, select General, the check the box next to Line numbers. This will display line numbers in the query editor window, to make finding the lines specified below easier. 
    
    ![On the left side of the Options dialog box, Text Editor is highlighted, Transact-SQL is highlighted below that, and General is selected and highlighted below that. At right, Line numbers is selected and highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image67.png "Display line numbers in the query editor")

11. Select OK to close the Options dialog.

12. In the SSMS query editor for instawdwdb.sql, uncomment the SETVAR lines (lines 36 and 37) by removing the double hyphen "\--" from the beginning of each line.

13. Next, edit the file path for each variable so they point to the following (remember to include a trailing backslash "\\" on each path):

    -   SqlSamplesDatabasePath: C:\\AdventureWorksSample\\

    -   SqlSamplesSourceDataPath: C:\\AdventureWorksSample\\AdventureWorks 2008R2 Data Warehouse\\ 
        
        ![The variables and file paths specified above are highlighted in the SSMS query editor.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image68.png "Edit the variable file paths")

14. Place SSMS into SQLCMD mode by selecting it from the Query menu. 
    
    ![SQLCMD Mode is highlighted in the Query menu.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image69.png "Select SQLCMD Mode")

15. Execute the script by selecting the Execute button on the toolbar in SSMS. 
    
    ![Execute is highlighted on the SSMS toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image70.png "Run the script")

16. This will create the AdventureWorksDW2008R2 database. When the script is done running, you will see output similar to the following in the results pane. 
    
    ![Output is displayed in the results pane. At this time, we are unable to capture all of the information in the window. Future versions of this course should address this.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image71.png "View the script output")

17. Expand Databases in Object Explorer, right-click the AdventureWorksDW2008R2 database, and select Rename. 
    
    ![On the left side of Object Explorer, Databases is highlighted, AdventureWorksDW2008R2 is highlighted below that, and Rename is selected and highlighted in the submenu.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image72.png "Select Rename")

18. Set the name of the database to WorldWideImporters.

    ![WorldWideImporters is highlighted under Databases in Object Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image73.png "Name the database")

### Task 8: Update SQL Server service accounts using Configuration Manager

1.  From the Start Menu on your SqlServerDw VM, search for SQL Server 2017 Configuration Manager, then select it from the search results.

    ![SQL Server 2017 Configuration Manager is selected and highlighted in the Search results.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image74.png "Select SQL Server 2017 Configuration Manager")

2.  From the tree on the left, select SQL Server Services. 
    
    ![SQL Server Services is highlighted on the left side of SQL Server 2017 Configuration Manager.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image75.png "Select SQL Server Services")

3.  In the list of services, double-click SQL Server (MSSQLSERVER) to open its properties dialog. 
    
    ![SQL Server (MSSQLSERVER) is highlighted in the list on the right side of SQL Server 2017 Configuration Manager.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image76.png "Select SQL Server (MSSQLSERVER)")

4.  In the SQL Server (MSSQLSERVER) Properties dialog, change the Log On user to use the holuser account, by entering holuser into the Account Name box, then entering the password, Password.1!!, into the Password and Confirm password boxes. 
    
    ![The above credentials are highlighted in the SQL Server (MSSQLSERVER) Properties dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image77.png "Enter holuser credentials")

5.  Select OK.

6.  Select Yes to restart the service in the Confirm Account Change dialog. 
    
    ![The Yes button is highlighted in the Confirm Account Change dialog.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image78.png "Confirm Account Change dialog box")

7.  Repeat steps 3 -- 6 above to set the server account to holuser for the SQL Server (SQLSERVER2008) instance as well, if it is not already using the holuser account.

8.  While still in the SQL Server 2017 Configuration Manager, expand SQL Server Network Configuration, select Protocols for MSSQLSERVER, and double-click TCP/IP to open the properties dialog. 
    
    ![Protocols for MSSQLSERVER is highlighted on the left side of SQL Server 2017 Configuration Manager, and TCP/IP is highlighted in the Protocol Name list on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image79.png "Select TCP/IP")

9.  On the TCP/IP Properties dialog, set Enabled to Yes, and select OK. 
    
    ![Enabled is selected on the Protocol tab of the TCP/IP Properties dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image80.png "Enable TCP/IP")

10. When prompted that the changes will not take effect until the service is restarted, select OK. You will restart the service later.

11. Return to the SSMS window.

12. Select Connect in the Object Explorer, and select Database Engine...
    
    ![Database Engine is selected and highlighted under Connect, which is highlighted in Object Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image81.png "Select Database Engine")

13. In the Connect to Server dialog, select the SQL Server 2017 instance, SqlServerDw, from the Server name drop down, leave Authentication set to Windows Authentication, and select Connect. 
    
    ![SqlServerDw is selected and highlighted in the Server name drop-down list in the Connect to Server dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image82.png "Select SqlServerDw")

14. In the Object Explorer, you will now see both the 2008R2 and 2017 instances. Under the 2017 instance, SqlServerDw, right-click Databases, and select New Database...

    ![Databases is highlighted under the SqlServerDw 2017 instance, and New Database is selected and highlighted in the submenu.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image83.png "Select New Database")

15. In the New Database dialog, enter Northwind for the Database name, and select OK. 
    
    ![Northwind is highlighted in the Database name box in the New Database dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image84.png "Enter the database name")

16. Now, return to the SQL Server 2017 Configuration Manager window.

17. As done previously, select SQL Server Services in the tree on the left, then right-click SQL Server (MSSQLSERVER) in the services pane, and select Restart. 
    
    ![SQL Server Services is highlighted on the left side of SQL Server 2017 Configuration Manager, SQL Server (MSSQLSERVER) is highlighted on the right, and Restart is highlighted in the submenu.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image85.png "Select Restart")

18. Repeat the previous step for the SQL Server Agent (MSSQLSERVER) service, this time selecting Start from the menu. 
    
    ![SQL Server Services is highlighted on the left side of SQL Server 2017 Configuration Manager, SQL Server Agent (MSSQLSERVER) is highlighted on the right, and Start is highlighted in the submenu.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image86.png "Select Start")

19. Close the SQL Server 2017 Configuration Manager.

## Exercise 2: Perform an assessment for a move to Azure SQL Database

Duration: 15 minutes

World Wide Importers would like an assessment to see what potential issues they would have to address in moving their database to Azure SQL Database.

### Task 1: Perform an assessment

1.  On your SqlServerDw VM, open a web browser, and download the Data Migration Assistant v3.x from <https://www.microsoft.com/download/details.aspx?id=53595> by clicking the Download button on the page.

2.  Run the installer.

3.  Select Next on each of the screens, accepting to the license terms and privacy policy in the process.

4.  Select Install on the Privacy Policy screen to begin the installation.

5.  On the final screen, check the Launch Microsoft Data Migration Assistant check box, and select Finish. 
    
    ![Launch Microsoft Data Migration Assistant is selected and highlighted at the bottom of the Microsoft Data Migration Assistant Setup dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image87.png "Run the Microsoft Data Migration Assistant")

6.  In the Data Migration Assistant window, select +New. 
    
    ![+ New is selected and highlighted in the Data Migration Assistant window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image88.png "Select + New")

7.  In the New project dialog, enter the following:

    -   Project type: Select Assessment

    -   Project name: Enter Assessment

    -   Source server type: SQL Server

    -   Target server type: Azure SQL Database
        
        ![The above information is entered in the New project dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image89.png "Enter information in the New project dialog box")

    -   Select Create

8.  On the Options screen, ensure the Check database compatibility and Check feature parity report types are checked, and select Next. 
    
    ![Check database compatibility and Check feature parity are selected and highlighted on the Options screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image90.png "Select the report types")

9.  In the Connect to a server dialog, enter SqlServerDw\\SQLSERVER2008 into the Server name box, and uncheck Encrypt connection, then select Connect.

    ![In the Connect to a server dialog box, SqlServerDw\\SQLSERVER2008 is highlighted in the Server name box, and Encrypt connection is highlighted below that in the Connect to a server dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image91.png "Enter information in the Connect to a server dialog box")

10. In the Add sources dialog that appears, check the box next to WorldWideImporters, and select Add. 
    
    ![WorldWideImporters is selected and highlighted under SqlServerDw\\SQLSERVER2008 in the Add sources dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image92.png "Select WorldWideImporters")

11. Select Start Assessment.

12. Review the Assessment results. 
    
    ![Various information is selected on the Review results screen. At this time, we are unable to capture all of the information in the window. Future versions of this course should address this.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image93.png "Review the Assessment results")

13. You now have a list of the issues WWI will need to consider in upgrading their database to Azure SQL Database. Notice the assessment includes recommendations on the potential resolutions to issues. You can select Export report to save the report as a JSON file, if desired.

## Exercise 3: Upgrade the SQL Server 2008 database to SQL Server 2017

Duration: 30 minutes

World Wide Importers would like a Proof of Concept (POC) that moves their database to SQL Server 2017 Enterprise. They would like to know about any incompatible features that will block their eventual production move.

### Task 1: Create a shared folder for the database migration

In this task, you are going to create a shared folder that is accessible by both the source and target databases for the migration. This folder will store a backup of the database used for the migration process.

1.  Open Windows Explorer.

2.  Create a new folder in the root of C: named backupshare. 
    
    ![This PC is selected on the left side of the Local Disk (C:) window, and the backupshare folder is selected and highlighted on the right side.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image94.png "Select backupshare")

3.  Right-click the backupshare folder, and select Properties. 
    
    ![Properties is selected and highlighted in the shortcut menu for the backupshare folder.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image95.png "Select Properties")

4.  In the backupshare Properties dialog, select the Sharing tab, and select Share...
    
    ![Share is highlighted on the Sharing tab (highlighted) in the backupshare Properties dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image96.png "Select Share")

5.  Ensure the Permission Level for the holuser account is set to Read/Write, and select Share. 
    
    ![The holuser account is highlighted as is the Read/Write Permission Level in the File Sharing dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image97.png "Check the Permission Level")

6.  If prompted about turning on network discovery, select No. 
    
    ![No, make the network that I am connected to a private network is highlighted in the Network discovery and file sharing dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image98.png "Select No")

7.  Select Done. 
    
    ![Done is highlighted under Your folder is shared in the File Sharing dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image99.png "Select Done")

8.  Select Close on the backupshare Properties dialog.

### Task 2: Migrate using Data Migration Assistant

1.  On your SqlServerDw VM, open the Data Migration Assistant.

2.  Select +New.

    ![+ New is selected and highlighted in the Data Migration Assistant window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image88.png "Select + New")

3.  On the New project pane, set the following:

    -   Project type: Select Migration

    -   Project name: Enter Migration

    -   Source server type: Select SQL Server

    -   Target server type: Select SQL Server, as you will target the "on-premises" SQL Server 2017 instance you installed previously.

    -   Select Create.

        ![The above information is entered in the New project pane.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image100.png "Enter information in the New project pane")

4.  On the Specify source & target tab, enter the following:

    -   Source server name: SqlServerDw\\SQLSERVER2008

    -   Target server name: SqlServerDw

    -   Authentication type: Set to Windows Authentication for both the Source and Target servers.

    -   Connection properties: Ensure the Encrypt connection and Trust server certification check boxes are unchecked for both the Source and Target servers. 
        
        ![The above information is entered on the Specify source & target screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image101.png "Enter information on the Specify source & target screen")

5.  Select Next.

6.  On the Add database screen:

    -   Make sure the ONLY database selected is WorldWideImporters. All other databases should be unchecked.

    -   Enter \\\\SqlServerDw\\backupshare into the Shared location text box, and leave the remaining fields as they are, then select Next. 
        
        ![The above information is entered on the Add database screen, and WorldWideImporters and \\\\SqlServerDw\\backupshare are highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image102.png "Enter information on the Add database screen")

7.  On the Select logins screen, ensure SqlServerDw\\holuser is selected, then select Start Migration. 
    
    ![SqlServerDw\\holuser is highlighted under Selected Logins (1/3) on the Select logins screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image103.png "Select Start Migration")

8.  Review the results. You may also wish to select Export report to save the report as a CSV file for later review. 
    
    ![WorldWideImporters is highlighted under Databases (1) on the View results screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image104.png "Review the results")

## Exercise 4: Post upgrade enhancement

Duration: 20 minutes

In this exercise, you will confirm that the POC was successful. To demonstrate value from the upgrade, the new features of SQL Server 2017 (Table Compression and ColumnStore Index) will be enabled to immediately show benefit.

### Task 1: Table compression

1.  On the SqlServerDw VM, open SSMS 17. Note: There are two version of SSMS installed, so be sure to choose the correct one.

2.  Connect to your SQL Server 2017 instance by selecting Connect in the Object Explorer, entering SqlServerDw into the Server name box, then selecting Connect. 
    
    ![SqlServerDw is selected in the Server name box in the Connect to Server dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image105.png "Connect to your SQL Server 2017 instance")

3.  Expand Databases, then WorldWideImporters, and then Tables. 
    
    ![Databases is highlighted in Object Explorer, WorldWideImporters is highlighted below that, and then Tables is highlighted below that.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image106.png "Select Tables")

4.  Right-click on the FactInternetSales table, and select Properties. 
    
    ![The FactInternetSales table is selected, and the Properties is highlighted in the submenu.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image107.png "Select Properties")

5.  In the Table Properties -- FactInternetSales dialog, select the Storage page, and record the data space and index space being used. 
    
    ![In the Table Properties -- FactInternetSales dialog box, Storage is selected and highlighted under Select a page on the left, and Data space (8.000 MB) and Index space (10.820 MB) are highlighted under General on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image108.png "Record the data space and index space")

6.  Select Cancel to close the properties dialog.

7.  Right-click the FactInternetSales table again, and this time select the Storage context menu, then select Manage Compression... from the flyout menu.
    
    ![The FactInternetSales table is selected on the left, Storage is selected and highlighted in the submenu, and Manage Compression is highlighted on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image109.png "Select Manage Compression")

8.  On the Welcome page of the Data Compression Wizard, select Next.

9.  On the Select Compression Type pages, select Row from the Compression Type drop down, then select Calculate. Observe the simulated Requested compressed space value. 
    
    ![Row and 10.023 MB are highlighted under Compression type and Requested compressed space, and Calculate is highlighted in the Data Compression Wizard - FactInternetSales dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image110.png "Select Row and then select Calculate")

10. Now, change the Compression type to Page, select Calculate, and observe the Requested compressed space. 
    
    ![Page and 6.305 MB are highlighted under Compression type and Requested compressed space in the Data Compression Wizard - FactInternetSales dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image111.png "Select Page and then select Calculate")

11. Leave the Compression type set to Page, and select Next.

12. On the Select an Output Option screen, select Run Immediately, and select Finish \>\>\|. 
    
    ![Run immediately is selected and highlighted under Select an Output Option on the Select an Output Option screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image112.png "Select Run Immediately")

13. Select Finish on the summary page. 
    
    ![Data Compression Wizard summary is selected under Review your selections on the Data Compression Wizard Summary screen, and Finish is highlighted at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image113.png "Select Finish on the summary page")

14. When the compression is completed, you will see a success message. Select Close. 
    
    ![A Success message appears on the Compression Wizard Progress screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image114.png "View the Success message")

15. Return to the FactInternetSales Properties dialog (right-click the table and select Properties), and select the Storage page. Once again, note the Data space and Index space, and compare those to the values recorded in Step 5, above. Notice the improvement with compression. 
    
    ![In the Table Properties -- FactInternetSales dialog box, Storage is selected and highlighted under Select a page on the left, and Data space (2.086 MB) and Index space (10.789 MB) are highlighted under General on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image115.png "Record the data space and index space")

16. Compression decreases the load on the Disk I/O subsystem, while increasing the load on the CPU. Since most data warehouse workloads are heavily disk bound, and often have low CPU usage, compression can be a great way to improve performance.

17. Select Cancel to close the table properties window.

### Task 2: Clustered ColumnStore index

In this task, you will create a new table based on the existing FactResellerSales table and apply a ColumnStore index.

1.  In SSMS 17, ensure you are connected to the SQL Server 2017 instance (SqlServerDw).

2.  Open a new query window by selecting New Query from the toolbar. 
    
    ![The New Query icon is highlighted on the SSMS toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image116.png "Select New Query")

3.  Copy the script below, and paste it into the query window.
    ```
    use WorldWideImporters

    select *

    into ColumnStore_FactResellerSales

    from FactResellerSales

    go
    ```

4.  Select Execute on the toolbar to run the query.

    ![The Execute icon is highlighted on the SSMS toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image117.png "Select Execute")

5.  In the Object Explorer, expand Tables under the WorldWideImporters database, right-click the new ColumnStore\_FactResellerSales table, and select Properties. You may need to select the Refresh button in the Object Explorer, if you don't see the new table.

6.  In the ColumnStore\_FactResellerSales properties dialog, select the Storage page and note the Data and Index space. 
    
    ![In the Table Properties -- columnstore\_FactResellerSales dialog box, Storage is selected and highlighted under Select a page on the left, and Data space (12.359 MB) and Index space (0.008 MB) are highlighted under General on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image118.png "Record the data space and index space")

7.  Select Cancel to close the properties dialog.

8.  Select New Query again from the toolbar, and copy and paste the below query into the new query window.
    ```
    USE [WorldWideImporters]

    GO

    CREATE CLUSTERED COLUMNSTORE INDEX [cci_FactResllerSales]

    ON [dbo].[ColumnStore_FactResellerSales]
    ```

9.  This query will create a clustered ColumnStore index on the ColumnStore\_FactResellerSales table. Run the query by selecting Execute on the toolbar.

10. Return to the properties window of the ColumnStore\_FactResellerSales table, select the Storage page, and observe the Data and Index space values. Select Cancel to close the properties window.
    
    ![In the Table Properties -- columnstore\_FactResellerSales dialog box, Storage is selected and highlighted under Select a page on the left, and Data space (1.664 MB) and Index space (0.000 MB) are highlighted under General on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image119.png "Record the data space and index space")

11. Create a new query window by selecting New Query from the toolbar, and select the Include Actual Execution Plan by selecting its button in the toolbar.

    ![The Include Actual Execution Plan icon is highlighted on the New Query the toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image120.png "Select the Include Actual Execution Plan")

12. Paste the queries below into the new query window, and select Execute on the toolbar.
    ```
    select productkey,

    salesamount

    from ColumnStore_FactResellerSales

    select productkey,

    salesamount

    from FactResellerSales
    ```

13. In the Results pane, select the Execution Plan tab. Check the (relative to the batch) percentage value of the two queries and compare them. 
    
    ![The Execution Plan tab is highlighted in the Results pane, 7% is highlighted for Query 1, and 93% is highlighted for Query 2.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image121.png "Compare the two queries")

14. Run the same queries again, but this time set statistics IO on in the query by adding the following to the top of the query window.
    ```
    set statistics io on

    GO
    ```

15. Your query should look like:

    ![The query includes the above information at the top.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image122.png "Set stastics IO")

16. Select Execute from the toolbar to run the query.

17. Statistics IO reports on the amount of logical pages that are read in order to return the query results. Select the Messages tab of the Results pane, and compare two numbers, logical reads and lob logical reads. You should see a significant drop in total number of logical reads on the columns store table. ;
    
    ![Various information is highlighted on the Messages tab of the Results pane.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image123.png "Compare the information")

## Exercise 5: Setup Oracle 11g Express Edition

Duration: 45 minutes

In this exercise, you will install Oracle XE on your Lab VM, load a sample database supporting an application, and then migrate the database to SQL Server 2017.

### Task 1: Install Oracle XE

1.  Connect to your Lab VM using RDP, as you did in [Before the Hands-on Lab, Task 2](file://Mac/Dropbox/Microsoft%20Cloud%20Workshops/Database%20Upgrade%20and%20Migration/Hands-on%20Lab%20Step%20By%20Step%20-%20Data%20platform%20upgrade%20and%20migration.docx#_Task_2:_Connect).

    -   User name: holuser

    -   Password: Password.1!!

2.  Using a web browser, navigate to <http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html>

3.  Accept the license agreement, and select Oracle Database 11g Express Edition Release 2 for Windows x64.

    ![Accept the license agreement and Oracle Database 11g Express Edition Release 2 for Windows x64 are highlighted under Oracle Database Express Edition 11g Release 2.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image124.png "Accept the license agreement")

4.  Sign in with your Oracle account to complete the download. If you don't already have a free Oracle account, you will need to create one.

    ![This is a screenshot of the Sign in screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image125.png "Sign in to complete the download")

5.  After signing in, the file will download.

6.  Unzip the file, and navigate to the DISK1 folder.

7.  Right-click setup.exe, and select Run as administrator. 
    
    ![In File Explorer, setup.exe is selected, and Run as administrator is highlighted in the shortcut menu.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image126.png "Run setup.exe as an administrator")

8.  Select Next to step through each screen of the installer, accepting the license agreement and default values, until you get to the Specify Database Passwords screen.

9.  Set the password to Password.1!!, and select Next.
    
    ![The above credentials are entered on the Specify Database Passwords screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image127.png "Set the password")

10. On the Summary screen, take note of the ports being assigned, and select Install. 
    
    ![Several of the ports being assigned are highlighted on the Summary screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image128.png "Note the ports being assigned")

11. Select Finish on the final dialog to compete the installation.

### Task 2: Install Oracle Data Access components

1.  On your Lab VM, open a browser and navigate to <http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html>

2.  Accept the license agreement, and select the ODAC122010\_x64.zip download link under 64-bit ODAC 12.2c Release 1 (12.2.0.1.0) for Windows x64. 
    
    ![Accept the license agreement and ODAC122010\_x64.zip are highlighted on the 64-bit Oracle Data Access Components (ODAC) Downloads screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image129.png "64-bit Oracle Data Access Components (ODAC) Downloads screen")

3.  When the download completes, extract the contents of the ZIP file to a local drive.

4.  Navigate to the folder containing the extracted ZIP file, and right-click setup.exe, then select Run as administrator to begin the installation.

5.  Select Next to accept the default language, English, on the first screen.

6.  On the Specify Oracle Home User screen, accept the default, Use Windows Built-in Account, and select Next.

7.  Accept the default installation locations, and select Next.

8.  On the Available Product Components, uncheck "Oracle Data Access Components Documentation for Visual Studio", and select Next. 
    
    ![Oracle Data Access Components Documentation for Visual Studio is cleared on the Available Product Components screen, and Next is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image130.png "Clear Oracle Data Access Components Documentation for Visual Studio")

9.  On the ODP.NET screen, check the box for Configure ODP.NET and/or Oracle Providers for ASP.NET at machine-wide level, and select Next.
    
    ![Configure ODP.NET and/or Oracle Providers for ASP.NET at machine-wide level is selected on the ODP.NET screen, and Next is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image131.png "Select Configure ODP.NET and/or Oracle Providers for ASP.NET at machine-wide level")

10. On the DB Connection Configuration screen, enter the following:

    -   Connection Alias: Northwind

    -   Port Number: 1521

    -   Database Host Name: localhost

    -   Database Service Name: XE 
        
        ![The information above is entered on the DB Connection Configuration screen, and Next is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image132.png "Enter the information")

    -   Select Next.

11. If the Next button is disabled on the Perform Prerequisite Checks screen, check the Ignore All box, and then select Next. This screen will be skipped by the installer if no missing requisites are found.
    
    ![The Ignore All box is cleared on highlighted on the Perform Prerequisite Checks screen, and Next is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image133.png "Clear Ignore All if necessary")

12. On the Summary screen, select Install.

13. On the Finish screen, select Close.

### Task 3: Install SQL Server Migration Assistant (SSMA) 7.x for Oracle

1.  In a web browser on your Lab VM, navigate to <https://www.microsoft.com/en-us/download/details.aspx?id=54258>

2.  Select the Download button to download SSMA. 
    
    ![Download is selected and highlighted under Microsoft SQL Server Migration Assistant v7.7 for Oracle.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image134.png "Download link")

3.  Check the box next to SSMA for Oracle.7.7.0.msi, and select Next to begin the download. 
    
    ![SSMAforOracle\_7.7.0.msi and Next are selected and highlighted under Choose the download you want.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image135.png "Choose the download you want section")

4.  Run SSMA for Oracle.7.7.0.msi to start the installation of SSMA. Select Next on the Welcome screen. 
    
    ![Next is selected on the SSMA for Oracle Welcome screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image136.png " SSMA for Oracle Welcome screen")

5.  Accept the License Agreement, and select Next.

6.  On the Choose Setup Type screen, select Typical, and select Next. 
    
    ![Typical is selected and highlighted on the Choose Setup Type screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image137.png "Select Typical")

7.  Select Install on the Ready to Install screen. 
    
    ![Install is selected on the Ready to Install screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image138.png "Select Install")

8.  Select Finish when the installation is complete.

### Task 4: Install dbForge Fusion tool (trial version)

In this task, you will install a third-party extension to Visual Studio to enable interaction with, and script execution for, the Oracle database in Visual Studio 2017 Community Edition. This step is necessary because the Oracle Developer Tools extension does not currently work with Visual Studio 2017 Community Edition.

1.  On your Lab VM, open a web browser and navigate to <https://www.devart.com/dbforge/oracle/fusion/download.html>

2.  Scroll down on the page, and download a Trial of the current version by selecting the green download link. 
    
    ![Trial and Download are highlighted under dbForge Fusion, Current Version.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image139.png "dbForge Fusion, Current Version section")

3.  Run the installer. If Visual Studio is open, you will need to exit the application prior to running the installer.

4.  Select Next on the Welcome screen. 
    
    ![Next is selected on the Devart dbForge Fusion for Oracle Welcome screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image140.png "Select Next on the Welcome screen")

5.  Accept the license agreement, and select Next.

6.  Accept the default installation location, and select Next.

7.  On the Select Start Menu Folder, select Next, accepting the default value.

8.  On the Install Products for Add-in screen, ensure Visual Studio 2017 is checked, and select Next. 
    
    ![Visual Studio 2017 and Next are selected on the Install Products for Add-in screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image141.png "Select Visual Studio 2017")

9.  On the Merge Help Namespaces screen, leave the check box unchecked, and select Next. 
    
    ![Perform pre-merging of namespaces during the installation is cleared and Next is selected on the Merge Help Namespaces screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image142.png "Clear Perform pre-merging of namespaces during the installation")

10. Select Install on the Ready to Install screen. 
    
    ![Install is selected on the Ready to Install screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image143.png "Select Install ")

11. Select Finish when the installation is complete.

### Task 5: Create the Northwind database in Oracle 11g XE

In this task, you will create a connection to the Oracle database on your Lab VM, and create a database called Northwind.

1.  On your Lab VM, download the starter files from <http://bit.ly/2rwxnwW>. (Note the URL is cases sensitive, so you may need copy and paste it into your browser.)

2.  When the download completes, unzip the contents to C:\\handsonlab.

3.  Within the handsonlab folder, open the NorthwindMVC folder, and double-click NorthwindMVC.sln to open the project in Visual Studio 2017.

4.  If prompted for how you want to open the file, select Visual Studio 2017, and select OK. 
    
    ![Visual Studio 2017 is selected and highlighted under How do you want to open this file?](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image144.png "Open Visual Studio 2017")

5.  Sign into Visual Studio (or create an account if you don't have one), when prompted.

6.  At the Security Warning screen, uncheck Ask me for every project in this solution, and select OK. ![Ask me for every project in this solution is cleared and OK is selected on the Security Warning screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image145.png "Clear Ask me for every project in this solution")

7.  Once then solution is open in Visual Studio, select the Fusion menu, and select New Connection. 
    
    ![New Connection is highlighted in the Fusion menu in Visual Studio.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image146.png "Select New Connection")

8.  In the Database Connection properties dialog, set the following values:

    -   Host: localhost

    -   Port: Leave 1521 selected

    -   Select SID, and enter XE

    -   User: system

    -   Password: Password.1!!

    -   Check Allow saving password

    -   Connect as: Normal

    -   Connection Name: Northwind

        ![The information above is entered in the Database Connection Properties - Oracle dialog box, and OK is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image147.png "Specify the settings")

9.  Select Test Connection to verify the settings are correct, and select OK to close the popup.

10. Select OK to create the Database Connection.

11. You will now see the Northwind connection in the Database Explorer window. 

    ![The Northwind connection is selected in the Database Explorer window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image148.png "View the Northwind connection")

12. Select Fusion from the Visual Studio menu, and select New Query. You may receive a notification that your trial has expired when you do this. This can be ignored for this hands-on lab. Close that dialog, and continue to the query window that opens in Visual Studio.

    ![New Query is highlighted in the Fusion menu in Visual Studio.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image149.png "Select New Query")

13. You will be working in the Text area of the query window, so at the bottom of the query window, select the Swap views icon to move the Text area to the top of the window, and the Query Builder down. You can then use the mouse to expand the Text area.

    ![The Swap views icon is highlighted at the bottom of the query window. ](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image150.png "Select the Swap views icon")

14. In Visual Studio, select File, Open File, and navigate to C:\\handsonlab\\NorthwindMVC\\Oracle Scripts, and select the file 1.northwind.oracle.schema.sql. Select Open. 
    
    ![The 1.northwind.oracle.schema.sql file is selected and highlighted in the Open File window, and Open is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image151.png "Open 1.northwind.oracle.schema.sql")

15. Select and copy all the file contents (use CTRL+A, CTRL+C).

16. Select the tab for your Fusion query. 
    
    ![Query1.sql\* is selected and highlighted on the tab for your Fusion query.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image152.png "Select Query1.sql*")

17. Paste (CTRL+V) the text copied in the previous step into the Text area at the bottom of the query window. 
    
    ![The text from the previous step is pasted and highlighted in the text area at the bottom of the query window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image153.png "Paste the text")

18. Select the Execute script button on the Visual Studio toolbar. 
    
    ![The Execute script icon is highlighted on the Visual Studio toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image154.png "Run the script")

19. The results of execution can be viewed in the Output window, found at the bottom left of the Visual Studio window.

    ![Output is highlighted in the Output window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image155.png "View the results")

20. In the Database Explorer window, right-click on the Northwind connection, and select Modify Connection... (If the Database Explorer is not already open, you can open it by selecting Fusion in the menu, then selecting Database Explorer).

    ![Modify Connection is highlighted in the submenu for the Northwind connection in the Database Explorer window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image156.png "Select Modify Connection")

21. In the Modify Connection dialog, change the username and password as follows:

    -   User name: NW

    -   Password: oracledemo123

22. Select Test Connection to verify the new credentials work. 
    
    ![The information above is entered and highlighted in the Database Connection Properties - Oracle dialog box, and Test Connection is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image157.png "Specify the settings")

23. Select OK to close the Database Connection properties dialog.

24. Return to your Fusion query window in Visual Studio.

25. Delete all the query text pasted into the Text area previously. (Click in the Text area, press CTRL+A, then press Delete).

26. Select the Open File icon on the Visual Studio toolbar. 
    
    ![The Open File icon is highlighted on the Visual Studio toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image158.png "Select Open File")

27. In the Open File dialog, navigate to C:\\handsonlab\\NorthwindMVC\\Oracle Scripts. Select the file 2.northwind.oracle.tables.views.sql, and select Open.

28. As you did previously, copy all the text from the query, and paste it into the Text area in the Fusion query window.

29. Select the Execute script button on the toolbar, and view the results of execute in the Output pane. 
    
    ![The Execute script icon is highlighted on the Visual Studio toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image154.png "Run the script")

30. Repeat steps 26-29, replacing the file name in step 27 with each of the following:

    -   3.northwind.oracle.packages.sql

    -   4.northwind.oracle.sps.sql

        -   During the Execute script step for this file, you will need to execute each CREATE OR REPLACE statement independently.

        -   Using your mouse, select the first statement, starting with CREATE and going to END; 
        
        ![The first statement between CREATE and END is highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image159.png "Select the first statement")

        -   Next, select Execute Selection in the Visual Studio toolbar. 
        
        ![Execute Selection is highlighted on the Visual Studio toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image160.png "Select Execute Selection")

        -   Repeat this for each of the remaining CREATE OR REPLACE... END; blocks in the script file (there are 7 more to execute, for 8 total)

    -   5.northwind.oracle.seed.sql

        -   This query can take a few minutes to run, so make sure you wait until you see Execute succeeded in the output window before executing the next file, like the following.
        
        ![This is a screenshot of the Execute succeeded message in the output window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image161.png "Execute succeeded message")

    -   6.northwind.oracle.constraints.sql

### Task 6: Configure the Starter Application to use Oracle

In this task, you will add the necessary configuration to the NorthwindMVC solution to connect to the Oracle database you created in the previous task.

1.  In Visual Studio, select Build from the menu, then select Build Solution. 
    
    ![Build Solution is highlighted in the Build menu in Visual Studio.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image162.png "Select Build Solution")

2.  Open Web.config by double-clicking the file in the Solution Explorer, on the right-hand side in Visual Studio. 
    
    ![Web.config is selected under Solution 'NorthwindMVC' (1 project) in Solution Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image163.png "Open Web.config")

3.  In the Web.config file, locate the connectionString section, and verify the connection string named "OracleConnectionString" matches the values you have used in this hands-on lab:
    ```
    DATA SOURCE=localhost:1521/XE;PASSWORD=oracledemo123;USER ID=NW
    ```

    ![The information above is highlighted in the Web.config file.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image164.png "Verify the connection string")

4.  Run the solution by selecting the green Start button on the toolbar. 

    ![Start is selected on the toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image165.png "Run the solution")

5.  You should see the Northwind Traders Dashboard load in your browser. 
    
    ![The Northwind Traders Dashboard is visible in a browser.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image166.png "View the dashboard")

6.  Close the browser to stop debugging the application, and return to Visual Studio.

## Exercise 6: Migrate the Oracle database to SQL Server 2017

Duration: 30 minutes

In this exercise, you will migrate the Oracle database into SQL Server 2017 using SSMA.

### Task 1: Migrate the Oracle database to SQL Server 2017 using SSMA

1.  Launch Microsoft SQL Server Migration Assistant for Oracle (32-bit) from the Start Menu.

2.  Select File, then New Project...

    ![File and New Project are highlighted in the SQL Server Migration Assistant for Oracle.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image167.png "Select New Project")

3.  In the New Project dialog, accept the default name and location, select SQL Server 2017 for the Migration To value, and select OK. 
    
    ![In the New Project dialog box, SQL Server 2017 is selected and highlighted in the Migration To box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image168.png "New Project dialog box")

4.  Select Connect to Oracle in the SSMA toolbar. 
    
    ![Connect to Oracle is highlighted on the SSMA toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image169.png "Select Connect to Oracle")

5.  In the Connect to Oracle dialog, enter the following:

    -   Provider: Leave set to the default value, Oracle Client Provider

    -   Mode: Leave set to Standard mode

    -   Server name: Enter localhost

    -   Server port: Set to 1521

    -   Oracle SID: Enter XE

    -   User name: Enter NW

    -   Password: Enter oracledemo123

        ![The information above is entered in the Connect to Oracle dialog box, and Connect is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image170.png "Specify the settings")

6.  Select Connect.

7.  In the Output window, you will see a message that the connection was established successfully, like the following: 
    
    ![The successful connection message is highlighted in the Output window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image171.png "View the successful connection message")

8.  Under Oracle Metadata Explorer, expand the localhost node, Schemas, and confirm you can see the NW schema, which will be the source for the migration.

    ![The NW schema is highlighted in Oracle Metadata Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image172.png "Confirm the NW schema")

9.  Next, select Connect to SQL Server from the toolbar, to add your SQL 2017 connection. 
    
    ![Connect to SQL Server is highlighted on the toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image173.png "Select Connect to SQL Server")

10. In the connect to SQL Server dialog, provide the following:

    -   Server name: Enter the IP address of your SqlServerDw VM. You can get this from the Azure portal by navigating to your VM's blade, and looking at the Essentials area. 
        
        ![The IP address of your SqlServerDw VM is highlighted in the Essentials area of your VM's blade in the Azure portal.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image174.png "Enter the IP address ")

    -   Server port: Leave set to \[default\]

    -   Database: Enter Northwind

    -   Authentication: Set to SQL Server Authentication

    -   User name: Enter sa

    -   Password: Enter Password.1!!

        ![The information above is entered in the Connect to SQL Server dialog box, and Connect is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image175.png "Specify the settings")

11. Select Connect.

12. You will see a success message in the output window. 
    
    ![The successful connection message is highlighted in the Output window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image176.png "View the successful connection message")

13. In the SQL Server Metadata Explorer, expand your servers node, then Databases. You should see Northwind listed. 
    
    ![Northwind is highlighted under Databases in SQL Server Metadata Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image177.png "Verify the Northwind listing")

14. In the Oracle Metadata Explorer, check the box next to NW, and make sure it is selected in the tree. 
    
    ![The NW schema is selected and highlighted in Oracle Metadata Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image178.png "Confirm the NW schema")

15. In the SQL Server Metadata explorer, check the box next to Northwind. 
    
    ![Northwind is selected and highlighted under Databases in SQL Server Metadata Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image179.png "Select Northwind")

16. In the SSMA toolbar, select Convert Schema. There is a bug in SSMA which prevents this button to being properly enabled, so if the button is disabled, you can expand the NW node in the Oracle Metadata Explorer, which should cause the Convert Schema button to become enabled. You can also right-click on the NW database in the Oracle Metadata Explorer, and select Convert Schema if that does not work.
    
    ![Convert Schema is highlighted on the SSMA toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image180.png "Select Convert Schema")

17. After about a minute the conversion should have completed.

18. In the SQL Server Metadata Explorer, observe that new schema objects have been added. For example, under Northwind, Schemas, NW, Tables you should see the tables from the Oracle database. 
    
    ![NW is selected in the SQL Server Metadata Explorer, and tables from the Oracle database are visible below that.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image181.png "Observe new schema objects")

19. In the output pane, you will notice a message that the conversion finished with 1 error, and 22 warnings. 
    
    ![The conversion message is highlighted in the Output window.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image182.png "View the conversion message")

20. To view the errors, select the Errors List at the bottom of the SSMA screen. 
    
    ![Errors List is highlighted at the bottom of the SSMA screen.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image183.png "Select Errors List")

21. Select Warnings to hide the warnings, and leave only Errors displayed. 
    
    ![The Warnings notification is highlighted in the Errors List.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image184.png "Errors list")

22. Double-click on the error listed. This will display the Table in both Oracle and SQL Server that is causing the error, EMPLOYEETERRITORIES. Notice the Oracle table lists EMPLOYEEID with a data type of NUMBER, while SQL Server is expecting a data type of float(53). 
    
    ![NUMBER and float(53) are highlighted under Data Type, and EMPLOYEEID is selected in the split-screen views of the Table tab for Oracle and SQL Server.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image185.png "View the information")

23. Look at the table definition for the table on the Oracle side.

24. To change the data type, select the Type Mapping tab, select the row with source type of number, and select Edit.
    
    ![The Type Mapping tab is highlighted, and below that, the row with source type of number is highlighted, and Edit is highlighted on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image186.png "Change the data type")

25. In the Edit Type Mapping dialog, set the Target type to numeric(precision, scale), set the Precision to 10, and select OK.

    ![The information above is entered in the Edit Type Mapping dialog box, and OK is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image187.png "Specify the settings")

26. Select Apply on the Type mapping tab. 
    
    ![The row with source type of number is highlighted on the Type Mapping tab, and Apply is highlighted at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image188.png "Select Apply")

27. In the Oracle Metadata Explorer, right-click the EMPLOYEETERRITORIES table, and select Convert Schema. 
    
    ![Convert Schema is highlighted in the submenu of the EMPLOYEETERRITORIES table in Oracle Metadata Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image189.png "Select Convert Schema")

28. When prompted that the target exists, select Overwrite. 
    
    ![Overwrite is selected and highlighted in the Target Exists prompt.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image190.png "Select Overwrite")

29. Notice the error is now gone from the Error List.

30. We are going to fix another data type conversion issue now, which will otherwise appear when we attempt to migrate the data.

31. Select the ORDER\_DETAILS table in the Oracle Metadata Explorer. 
    
    ![The ORDER\_DETAILS table is selected and highlighted in Oracle Metadata Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image191.png "Select the ORDER_DETAILS table ")

32. Next, you are going to convert the type associated with the DISCOUNT column, FLOAT(23) to a numeric(10, 0), similar to what you did for the EMPLOYEETERRITORIES table.

33. Select the Type Mapping tab, then select float\[\*..53\] from the Source Type list, and select Edit. 
    
    ![The row with source type of float\[\*..53\] is selected and highlighted on the Type Mapping tab, and Edit is highlighted on the right.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image192.png "Edit float[*..53]")

34. In the Edit Type Mapping dialog, change the Target type to numeric(precision, scale), and set the Precision to 10, then select OK.

    ![In the Edit Type Mapping dialog box, numeric(precision, scale) is highlighted under Target type, 10 is highlighted next to Replace with under Precision, and OK is highlighted at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image193.png "Change the Target type")

35. Select Apply to save the changes to the ORDER\_DETAILS table. 
    
    ![The row with source type of float\[\*..53\] is selected and highlighted on the Type Mapping tab, and Apply is highlighted at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image194.png "Apply float[*..53]")

36. Now, right-click on the ORDER\_DETAILS table in the Oracle Metadata Explorer, and select Convert Schema. 
    
    ![Convert Schema is highlighted in the submenu of the ORDER\_DETAILS table in Oracle Metadata Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image195.png "Select Convert Schema")

37. When prompted that the target exists, select Overwrite. 
    
    ![Overwrite is selected and highlighted in the Target Exists prompt.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image196.png "Target Exists prompt")

38. \[Optional\] Save the project. This can take a while, and is not necessary to complete the hands-on lab.

39. To apply the resultant schema to the Northwind database in SQL Server, use the SQL Server Metadata Explorer to view the Northwind database. Right-click Northwind, and select Synchronize with Database. 
    
    ![Synchronize with Database is highlighted in the submenu of the Northwind database in SQL Server Metadata Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image197.png "Select Synchronize with Database")

40. Select OK in the Synchronize with the Database dialog.

41. The Synchronize action will result in multiple errors in the Error List, resulting from attempting to add the SSMA assemblies to the Northwind database. 
    
    ![The Errors notification is highlighted at the top of the Errors List window, and the first of multiple errors is selected and highlighted in the Message list.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image198.png "View the errors")

42. These errors are the result of improvements implemented in SQL Server 2017 SQLCLR security model. Specifically, in SQL Server 2017, Microsoft now by default requires that all type of assemblies (SAFE, EXTERNAL\_ACCESS, UNSAFE) are authorized for UNSAFE access.

43. For this hands-on lab, you will be adding the assemblies causing the errors to the trusted assembly list, which is synonymous with whitelisting the assemblies. To fix these errors, complete the following:

    -   Under the Northwind database in the SQL Server Metadata Explorer in SSMA, expand Assemblies. 
        
        ![Three items are listed below Assemblies, which is highlighted below the Northwind database in SQL Server Metadata Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image199.png "Expand Assemblies")

    -   Right-click SSMA4OracleSQLServerCollections.NET, and select Save as Script. 
        
        ![Save as Script is highlighted in the submenu for SSMA4OracleSQLServerCollections.NET.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image200.png "Select Save as Script")

    -   Save the script to the local machine.

    -   Now, you will need to use SSMS on your SqlServerDw VM.

        -   Open an RDP connection to your SqlServerDw VM, if one is not already open.

        -   Open SSMS 17.

        -   Connect to the SQL Server 2017 instance (SqlServerDw), by selecting Connect, and entering SqlServerDw into the Server name field.

        -   Expand Databases.

        -   Right-click on Northwind, and select New Query.

        -   Paste the following query into the new query window.
    ```
    USE master;

    GO

    DECLARE @clrName nvarchar(4000) = 'SSMA4OracleSQLServerCollections.NET'

    DECLARE @asmBin varbinary(max) = [INSERT BINARY];

    DECLARE @hash varbinary(64);

    SELECT @hash = HASHBYTES('SHA2_512', @asmBin);

    EXEC sys.sp_add_trusted_assembly @hash = @hash, @description = @clrName;
    ```

    -   Now, return to your Lab VM, and open the saved SSMA4OracleSQLServerCollections.NET.sql file from the desktop with Notepad.exe.

    -   Within the SQL file, locate the line that begins with CREATE ASSEMBLY, then locate the word FROM. Copy the binary string that appears after FROM. This value will span all the way down to the line containing the text "WITH PERMISSION\_SET = SAFE". Be sure not to include any whitespace at the end of the binary value.
    
    ![The binary string that appears after FROM is highlighted within the SQL file.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image201.png "Copy the binary string")

    -   Now, return to SSMS on your SqlServerDw VM, and replace \[INSERT BINARY\] with the copied binary value. The line should end with ";" and there should be no whitespace before the ";".

    -   Execute the query in SSMS.

44. Repeat step 34, this time for the assembly SSMA4OracleSQLServerExtensions.NET. Make sure to replace the \@clrName variable in the script with the value "SSMA4OracleSQLServerExtensions.NET".

45. The SSMA assemblies have now been whitelisted in SQL Server 2017.

46. Return to SSMA on your Lab VM, and rerun the Synchronize with Database action on the Northwind database. This will create all the schema objects in the SQL Server Northwind database. There should now be no errors, and the Output pane should show Synchronization operation is complete.

47. Now you need to migrate the data.

48. In the Oracle Metadata Explorer, select NW and from the command bar, select Migrate Data. 
    
    ![Migrate Data is highlighted in the command bar of Oracle Metadata Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image202.png "Select Migrate Data")

49. You will be prompted to re-enter your Oracle credentials for use by the migration connection.

    -   Recall the Oracle credentials are:

        -   Server name: localhost

        -   Server port: 1521

        -   Oracle SID: XE

        -   User name: NW

        -   Password: oracledemo123

    -   The SQL Server credentials are:

        -   Server name: IP address of your SqlServerDw VM (obtained in the essentials area of your VM's blade in Azure portal)

        -   Server port: \[default\]

        -   Authentication: SQL Server Authentication

        -   User name: sa

        -   Password: Password.1!!

50. Select Connect.

51. After the migration completes, you will be presented with a Data Migration Report, similar to the following: 

    ![This is screenshot of an example Data Migration Report.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image203.png "View the Data Migration Report")

## Exercise 7: Migrate the Application

Duration: 15 minutes

In this exercise, you will modify the NorthwindMVC application, so it targets SQL Server 2017 instead of Oracle.

### Task 1: Create a new Entity Model against SQL Server

1.  On your Lab VM, return to Visual Studio, and open Web.config from the Solution Explorer.

2.  Modify the connection string named SqlServerConnectionString to match your remote SQL Server credentials.

    -   Replace the value of "data source" with your SqlServerDw VM's public IP address.

    -   Replace the value of "password" with Password.1!!.
    
    ![The information above is highlighted in Web.config.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image204.png "Replace the password value")

3.  Build the solution, by selecting Build in the Visual Studio menu, then selecting Build Solution. 
    
    ![Build and Build Solution are highlighted in Visual Studio.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image162.png "Select Build Solution")

4.  In the Solution Explorer, expand the Data folder, and select all the files within the folder. 
    
    ![In Solution Explorer, all the files under Data (highlighted) are selected.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image205.png "Expand the Data folder")

5.  Right-click, and choose Delete.

    ![Delete is selected in the shortcut menu for all the files listed under Data.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image206.png "Delete the files")

6.  Select OK at the confirmation prompt.

7.  Right-click on the Data folder, and select Add \> New Item...
    
    ![In the shortcut menu for the Data folder, New Item and Add are highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image207.png "Select Add")

8.  In the Add New Item dialog, expand Visual C\#, select Data, and select ADO.NET Entity Data Model. Enter DataContext for the name, and select Add.
    
    ![In the Add New Item dialog box, Visual C\#, Data, ADO.NET Entity Data Model, and DataContext are highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image208.png "Add DataContext")

9.  In the wizard's Choose Model Contents dialog, select Code First from database, and select Next. 
    
    ![Code First from database is highlighted under What should the model contain? in the Entity Data Model Wizard.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image209.png "Select Code First from database")

10. In the Choose Your Data Connection dialog, select SqlServerConnectionString (Settings) from the drop down, and select Yes, include the sensitive data in the connection string. Uncheck Save connection settings in Web.Config, and select Next. 
    
    ![SqlServerConnectionString (Settings) and Yes, include the sensitive data in the connection string are selected and highlighted in the Entity Data Model Wizard, and Save connection settings in Web.Config is cleared.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image210.png "Choose Your Data Connection settings")

11. In the Connect to SQL Server dialog, enter the Password, Password.1!!. 
    
    ![The password above is entered in the Connect to SQL Server dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image211.png "Enter the Password")

12. On the Choose Your Database Objects and Settings screen, expand the Tables node, and check NW only. Ensure Pluralize or singularize generated column names is checked.
    
    ![The Tables node is selected, and NW is selected and highlighted in the Entity Data Model Wizard. Pluralize or singularize generated column names is also selected.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image212.png "Choose Your Database Objects and Settings")

13. Select Finish, and the model will be generated. This may take a few minutes.

### Task 2: Modify Application Code

1.  In Visual Studio, open the file DataContext.cs from the Solution Explorer. You may need to collapse the Data folder, and re-expand it after refreshing if you don't see the file listed.
    
    ![DataContext.cs is highlighted under the Data folder in Solution Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image213.png "Open DataContext.cs")

2.  The call to base in the DataContext constructor, at the top of the file, needs to be updated to reflect the correct connection string.

    ![In the DataContext constructor, : base ("name=DataContext") is highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image214.png "Update the call to base")

3.  Change the line from:

    : base ("name=DataContext")

4.  To:

    : base ("name=SqlServerConnectionString")

5.  Save the file.

    ![In the DataContext constructor, : base ("name=SqlServerConnectionString") is highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image215.png "Update the call to base")

6.  Next, open the file HomeController.cs, in the Controllers folder in the Solution Explorer. 
    
    ![The HomeController.cs file is selected and highlighted under the Controllers folder in Solution Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image216.png "Open HomeController.cs")

7.  Comment out the code under the Oracle comment. First, select the lines for the Oracle code, then select the Comment button in the toolbar. 
    
    ![The code under the Oracle comment is highlighted and labeled 1, and the Comment button in the toolbar is highlighted and labeled 2.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image217.png "Comment out code")

8.  Next, uncomment the code under the SQL Server comment. Select the commented out code, then select the Uncomment button on the toolbar. You may need to click the Uncomment button twice to uncomment the code.

    The lines will change from green to colored text when the comment characters have been removed from the front of each line. This code change is done because of differences in how stored procedures are accessed in Oracle versus Sql Server.
        
    ![The code under the SQL Server comment is highlighted and labeled 1, and the Uncomment button in the toolbar is highlighted and labeled 2.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image218.png "Uncomment code")

9.  Save the changes to HomeController.cs.

10. Open the file, SALESBYYEAR.cs, in the Models folder in the Solution Explorer. 
    
    ![SALESBYYEAR.cs is highlighted under the Models folder in the Solution Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image219.png "Open SALESBYYEAR.cs")

11. Change the types of the following properties:

    -   Change the SUBTOTAL property from double to decimal.

    -   Change the YEAR property from string to int.
    
    ![The decimal and int property values are highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image220.png "Change the SUBTOTAL and YEAR properties")

12. Save the file.

13. Open the SalesByYearViewModel.cs file from the Models folder in the Solution Explorer. 

    ![SalesByYearViewModel.cs is highlighted under the Models folder in the Solution Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image221.png "Open SalesByYearViewModel.cs")

14. Change the type of the YEAR property from string to int, then save the file. 
    
    ![The int property value is highlighted.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image222.png "Change the YEAR property")

15. Run the solution by selecting the green Start button on the toolbar. 

    ![Start is highlighted on the toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image165.png "Select Start")

16. You will get an exception that the stored procedure call has failed. This is because of an error in migrating the stored procedure. ![An exception appears indicating that the stored procedure call has failed.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image223.png "View the error")

17. Select the red Stop button to end execution of the application. 
    
    ![The Stop button is highlighted on the toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image224.png "Select Stop")

18. To resolve the error, open the SALES\_BY\_YEAR\_fix.sql file, located under Solution Items in the Solution Explorer. 
    
    ![The SALES\_BY\_YEAR\_fix.sql file is highlighted under the Solution Items folder in Solution Explorer.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image225.png "Open SALES_BY_YEAR_fix.sql")

19. Note the database name at the top of the file (within the brackets after the USE keyword). 
    
    ![NorthwindMigration is displayed as the database name at the top of the file.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image226.png "Note the database name")

20. Enter the correct database name, Northwind, at the top of the file, and save the file.

    ![Northwind is listed as the correct database name at the top of the file.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image227.png "Enter the correct database name")

21. From the Visual Studio menu, select View, and then Server Explorer. 
    
    ![View and Server Explorer are highlighted in the Visual Studio menu.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image228.png "Select Server Explorer")

22. In the Server Explorer, right-click on Data Connections, and select Add Connections...
    
    ![Data Connections is selected in Server Explorer, and Add Connection is highlighted in the shortcut menu.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image229.png "Select Add Connection")

23. On the Choose Data Source dialog, select Microsoft SQL Server, and select Next. 
    
    ![Microsoft SQL Server is selected and highlighted under Data source in the Choose Data Source dialog box.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image230.png "Select Microsoft SQL Server")

24. On the Add Connection dialog, enter the following:

    -   Data source: Leave Microsoft SQL Server (SqlClient)

    -   Server name: Enter the IP address of your SqlServerDw VM.

    -   Authentication: Select SQL Server Authentication

    -   User name: Enter sa

    -   Password: Enter Password.1!!

    -   Connect to a database: Choose Select or enter database name, and enter Northwind

    -   Select Test Connection to verify your settings are correct, and select OK to close the successful connection dialog.

        ![The information above is entered in the Add Connection dialog box, and Test Connection is selected at the bottom.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image231.png "Specify the settings")

25. Select OK.

26. Right-click the newly added SQL Server connection in the Server Explorer, and select New Query. 
    
    ![The newly added SQL Server connection is selected in Server Explorer, and New Query is highlighted in the shortcut menu.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image232.png "Select New Query")

27. Select and copy all of the text from the SALES\_BY\_YEAR\_fix.sql file (click CTRL+A, CTRL+C in the SALES\_BY\_YEAR\_fix.sql file).

28. Paste (CTRL+V) the copied text into the new Query window.

29. Verify the Use \[Northwind\] statement is the first line of the file, and that it matches the database listed in the query bar. Select the green Execute button. 
    
    ![The Use \[Northwind\] statement is highlighted, as is the Northwind database and the Execute button in the query bar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image233.png "Verify the Use [Northwind] statement")

30. You should see a message that the command completed successfully. 
    
    ![This is a screenshot of a message that the command completed successfully.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image234.png "View the message")

31. Run the application again by clicking the green Start button in the Visual Studio toolbar. 
    
    ![The Start button is highlighted on the Visual Studio toolbar.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image165.png "Select Start")

32. Verify the graph is showing correctly on the Northwind traders dashboard. 
    
    ![The Northwind Traders Dashboard is visible in a browser.](images/Hands-onlabstep-by-step-Dataplatformupgradeandmigrationimages/media/image166.png "View the dashboard")

33. Congratulations! You have successfully migrated the data and application from Oracle to SQL Server.

## After the hands-on lab 

Duration: 10 mins

In this exercise, attendees will deprovision any Azure resources that were created in support of the lab. You should follow all steps provided after attending the Hands-on lab.

### Task 1: Delete the resource group

1.  Using the Azure portal, navigate to the Resource group you used throughout this hands-on lab by selecting Resource groups in the left menu.

2.  Search for the name of your research group, and select it from the list.

3.  Select Delete in the command bar, and confirm the deletion by re-typing the Resource group name, and selecting Delete.

You should follow all steps provided *after* attending the Hands-on lab.