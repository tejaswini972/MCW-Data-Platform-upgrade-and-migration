![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Data Platform upgrade and migration
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
September 2018
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

## Contents

- [Data Platform upgrade and migration before the hands-on lab setup guide](#data-platform-upgrade-and-migration-before-the-hands-on-lab-setup-guide)
    - [Requirements](#requirements)
    - [Before the hands-on lab](#before-the-hands-on-lab)
        - [Task 1: Provision a resource group](#task-1-provision-a-resource-group)
        - [Task 2: Create lab virtual machine](#task-2-create-lab-virtual-machine)
        - [Task 3: Create SQL Server virtual machine](#task-3-create-sql-server-virtual-machine)
        - [Task 4: Connect to the Lab VM](#task-4-connect-to-the-lab-vm)
        - [Task 5: Add inbound port 1433 rule on the SqlServerDw VM network security group](#task-5-add-inbound-port-1433-rule-on-the-sqlserverdw-vm-network-security-group)
        - [Task 6: Connect to the SqlServerDw VM](#task-6-connect-to-the-sqlserverdw-vm)
        - [Task 7: Open port 1433 on the Windows Firewall of the SqlServerDw VM](#task-7-open-port-1433-on-the-windows-firewall-of-the-sqlserverdw-vm)
        - [Task 8: Provision Azure SQL Database](#task-8-provision-azure-sql-database)

# Data Platform upgrade and migration before the hands-on lab setup guide

## Requirements

- Microsoft Azure subscription must be pay-as-you-go or MSDN

  - Trial subscriptions will not work.
  
- A virtual machine configured with:

  - Visual Studio Community 2017 or later
  
  - Azure SDK 2.9 or later (Included with Visual Studio 2017)

## Before the hands-on lab

Duration: 45 minutes

In the Before the hands-on lab exercise, you will set up your environment for use in the rest of the hands-on lab. You should follow all the steps provided in the Before the hands-on lab section to prepare your environment **before attending** the hands-on lab. Failure to do so will significantly impact your ability to complete the lab within the time allowed.

>**Important**: Most Azure resources require unique names. Throughout this lab you will see the word “SUFFIX” as part of resource names. You should replace this with your Microsoft alias, initials, or another value to ensure the resource is uniquely named.

### Task 1: Provision a resource group

In this task, you will create an Azure resource group for the resources used throughout this lab.

1. In the [Azure portal](https://portal.azure.com), select **Resource groups**, select **+Add**, then enter the following in the Create an empty resource group blade:

    - **Resource group name**: Enter hands-on-lab-SUFFIX.

    - **Subscription**: Select the subscription you are using for this hands-on lab.

    - **Resource group location**: Select the region you would like to use for resources in this hands-on lab. Remember this location so you can use it for the other resources you'll provision throughout this lab.

        ![Add Resource group Resource groups is highlighted in the navigation pane of the Azure portal, +Add is highlighted in the Resource groups blade, and "hands-on-labs" is entered into the Resource group name box on the Create an empty resource group blade.](./media/create-resource-group.png "Create resource group")

2. Select **Create**.

### Task 2: Create lab virtual machine

In this task, you will provision a virtual machine (VM) in Azure. The VM image used will have Visual Studio Community 2017 installed.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "visual studio community" into the Search the Marketplace box, select **Visual Studio Community 2017 (latest release) on Windows Server 2016 (x64)** from the results, and select **Create**.

    ![+Create a resource is selected in the Azure navigation pane, and "visual studio community" is entered into the Search the Marketplace box. Visual Studio Community 2017 (latest release) on Windows Server 2016 (x64) is selected in the results.](./media/create-resource-visual-studio-on-windows-server-2016.png "Create Windows Server 2016 with Visual Studio Community 2017")

2. On the Create a virtual machine Basics tab, set the following configuration:

    - Project Details:

        - **Subscription**: Select the same subscription you are using for this hands-on lab.
        - **Resource Group**: Choose Use existing, and select the hands-on-lab-SUFFIX resource group.

    - Instance Details:

        - **Virtual machine name**: Enter LabVM.
        - **Region**: Select the region you are using for resources in this hands-on lab.
        - **Availability options**: Select no infrastructure redundancy required.
        - **Image**: Leave Visual Studio Community 2017 (latest release) on Windows Server 2016 (x64) selected.
        - **Size**: Accept the default size, Standard D2 v3.

    - Administrator Account:

        - **Username**: Enter demouser.
        - **Password**: Enter Password.1!!

    - Inbound Port Rules

        - **Public inbound ports**: Choose Allow selected ports.
        - **Select inbound ports**: Select RDP (3389) in the list.

        ![Screenshot of the Basics tab, with fields set to the previously mentioned settings.](media/lab-virtual-machine-basics-tab.png "Create a virtual machine Basics tab")

    - Select **Next: Disks** to move to the next step.

3. On the Disks tab, set OS disk type to **Standard SSD**, and then select **Review + create**. Note, the remaining tabs can be skipped, and default values will be used.

    ![On the Create a virtual machine Disks tab, the OS disk type is set to Standard SSD.](media/lab-virtual-machine-disks-tab.png "Create a virtual machine Disks tab")

4. On the **Review + create** tab, ensure the Validation passed message is displayed, and then select **Create** to provision the virtual machine.

    ![The Review + create tab is displayed, with a Validation passed message.](media/lab-virtual-machine-review-create-tab.png "Create a virtual machine Review + create tab")

5. It may take 10+ minutes for the virtual machine to complete provisioning.

6. You can move on to the next task while waiting for the lab VM to provision.

### Task 3: Create SQL Server virtual machine

In this task, you will provision another virtual machine (VM) in Azure which will host your "on-premises" instances of both SQL Server 2008 R2 and SQL Server 2017. The VM will use the Windows Server 2012 R2 Datacenter image.

>**Note**:  An older version of Windows Server is being used because SQL Server 2008 R2 is not supported on Windows Server 2016.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "windows server 2012" into the Search the Marketplace box, select **Windows Server 2012 R2 Datacenter** from the results, and select **Create**

    ![+ Create a resource is highlighted on the left side of the Azure portal, and at right, windows server 2012 and Windows Server 2012 R2 Datacenter are highlighted.](./media/create-resource-windows-server-2012-r2-datacenter.png "Azure portal")

2. On the Create a virtual machine Basics tab, set the following configuration:

    - Project Details:

        - **Subscription**: Select the same subscription you are using for this hands-on lab.
        - **Resource Group**: Choose Use existing, and select the hands-on-lab-SUFFIX resource group.

    - Instance Details:

        - **Virtual machine name**: Enter SqlServerDw.
        - **Region**: Select the region you are using for resources in this hands-on lab.
        - **Availability options**: Select no infrastructure redundancy required.
        - **Image**: Leave Windows Server 2012 R2 Datacenter selected.
        - **Size**: Accept the default size, Standard DS1 v2.

    - Administrator Account:

        - **Username**: Enter demouser.
        - **Password**: Enter Password.1!!

    - Inbound Port Rules

        - **Public inbound ports**: Choose Allow selected ports.
        - **Select inbound ports**: Select RDP (3389) in the list.

        ![Screenshot of the Basics tab, with fields set to the previously mentioned settings.](media/sql-virtual-machine-basics-tab.png "Create a virtual machine Basics tab")

    - Select **Review + create** to move to the next step. Note, the remaining tabs can be skipped, and default values will be used.

3. On the **Review + create** tab, ensure the Validation passed message is displayed, and then select **Create** to provision the virtual machine.

    ![The Review + create tab is displayed, with a Validation passed message.](media/sql-virtual-machine-review-create-tab.png "Create a virtual machine Review + create tab")

4. It may take 10+ minutes for the virtual machine to complete provisioning.

5. You can move on to the next task while waiting for the SqlServerDw VM to provision.

### Task 4: Connect to the Lab VM

In this task, you will create an RDP connection to your Lab virtual machine (VM), and disable Internet Explorer Enhanced Security Configuration.

1. In the [Azure portal](https://portal.azure.com), select **Resource groups** in the Azure navigation pane, enter your resource group name (hands-on-lab-SUFFIX) into the filter box, and select it from the list.

    ![Resource groups is selected in the Azure navigation pane, "hands" is entered into the filter box, and the "hands-on-lab-SUFFIX" resource group is highlighted.](./media/resource-groups.png "Resource groups list")

2. In the list of resources for your resource group, select the LabVM Virtual Machine.

    ![The list of resources in the hands-on-lab-SUFFIX resource group are displayed, and LabVM is highlighted.](./media/resource-group-resources-labvm.png "LabVM in resource group list")

3. On your Lab VM blade, select Connect from the top menu.

    ![The LabVM blade is displayed, with the Connect button highlighted in the top menu.](./media/connect-labvm.png "Connect to LabVM")

4. Select **Download RDP file**, then open the downloaded RDP file.

    ![The Connect to virtual machine blade is displayed, and the Download RDP file button is highlighted.](./media/connect-to-virtual-machine.png "Connect to virtual machine")

5. Select **Connect** on the Remote Desktop Connection dialog.

    ![In the Remote Desktop Connection Dialog Box, the Connect button is highlighted.](./media/remote-desktop-connection.png "Remote Desktop Connection dialog")

6. Enter the following credentials when prompted:

    - **User name**: demouser
    - **Password**: Password.1!!

7. Select **Yes** to connect, if prompted that the identity of the remote computer cannot be verified.

    ![In the Remote Desktop Connection dialog box, a warning states that the identity of the remote computer cannot be verified, and asks if you want to continue anyway. At the bottom, the Yes button is circled.](./media/remote-desktop-connection-identity-verification-labvm.png "Remote Desktop Connection dialog")

8. Once logged in, launch the **Server Manager**. This should start automatically, but you can access it via the Start menu if it does not start.

    ![The Server Manager tile is circled in the Start Menu.](./media/start-menu-server-manager.png "Server Manager tile in the Start menu")

9. Select **Local Server**, then select **On** next to **IE Enhanced Security Configuration**.

    ![Screenshot of the Server Manager. In the left pane, Local Server is selected. In the right, Properties (For LabVM) pane, the IE Enhanced Security Configuration, which is set to On, is highlighted.](./media/windows-server-manager-ie-enhanced-security-configuration.png "Server Manager")

10. In the Internet Explorer Enhanced Security Configuration dialog, select **Off under Administrators**, then select **OK**.

    ![Screenshot of the Internet Explorer Enhanced Security Configuration dialog box, with Administrators set to Off.](./media/internet-explorer-enhanced-security-configuration-dialog.png "Internet Explorer Enhanced Security Configuration dialog box")

11. Close the Server Manager.

### Task 5: Add inbound port 1433 rule on the SqlServerDw VM network security group

In this task, you will open port 1433 on the network security group associated with the SqlServerDw VM to allow communication with SQL Server.

1. In the [Azure portal](https://portal.azure.com), select **Resource groups** in the Azure navigation pane, enter your resource group name (hands-on-lab-SUFFIX) into the filter box, and select it from the list.

    ![Resource groups is selected in the Azure navigation pane, "hands" is entered into the filter box, and the "hands-on-lab-SUFFIX" resource group is highlighted.](./media/resource-groups.png "Resource groups list")

2. In the list of resources for your resource group, select the SqlServerDw VM.

    ![The list of resources in the hands-on-lab-SUFFIX resource group are displayed, and SqlServerDw is highlighted.](media/resource-group-resources-sqlserverdw.png "SqlServerDw VM in resource group list")

3. On the SqlServerDw blade, select **Networking** under Settings in the left-hand menu, and then select **Add inbound port rule**.

    ![Add inbound port rule is highlighted on the SqlServerDw - Networking blade.](media/sql-virtual-machine-add-inbound-port-rule.png "SqlServerDw - Networking blade")

4. On the **Add inbound security rule blade**, select **Basic** and then enter the following:

    - **Service**: Select MS SQL.
    - **Port ranges**: Value will be set to 1433.
    - **Priority**: Accept the default priority value.
    - **Name**: Enter SqlServer.

        ![On the Add inbound security rule dialog, MS SQL is selected for Service, port 1433 is selected, and the SqlServer is entered as the name.](media/sql-virtual-machine-add-inbound-security-rule-1433.png "Add MS SQL inbound security rule")

5. Select **Add**.

### Task 6: Connect to the SqlServerDw VM

In this task, you will create an RDP connection to the SqlServerDw VM, and configure the server to be an application server. This is necessary to install the required .NET components on the server prior to installing SQL Server 2008 R2. You will also disable IE Enhanced Security Configuration, as you did on the Lab VM.

1. In the [Azure portal](https://portal.azure.com), select **Resource groups** in the Azure navigation pane, enter your resource group name (hands-on-lab-SUFFIX) into the filter box, and select it from the list.

    ![Resource groups is selected in the Azure navigation pane, "hands" is entered into the filter box, and the "hands-on-lab-SUFFIX" resource group is highlighted.](./media/resource-groups.png "Resource groups list")

2. In the list of resources for your resource group, select the SqlServerDw VM.

    ![The list of resources in the hands-on-lab-SUFFIX resource group are displayed, and SqlServerDw is highlighted.](media/resource-group-resources-sqlserverdw.png "SqlServerDw VM in resource group list")

3. On the SqlServerDw blade, select Connect from the top menu.

    ![The SqlServerDw blade is displayed, with the Connect button highlighted in the top menu.](media/connect-sqlserverdw.png "Connect to SqlServerDw")

4. Select **Download RDP file**, then open the downloaded RDP file.

    ![The Connect to virtual machine blade is displayed, and the Download RDP file button is highlighted.](./media/connect-to-virtual-machine.png "Connect to virtual machine")

5. Select **Connect** on the Remote Desktop Connection dialog.

    ![In the Remote Desktop Connection Dialog Box, the Connect button is highlighted.](./media/remote-desktop-connection.png "Remote Desktop Connection dialog")

6. Enter the following credentials when prompted:

    - **User name**: demouser
    - **Password**: Password.1!!

7. Select **Yes** to connect, if prompted that the identity of the remote computer cannot be verified.

    ![In the Remote Desktop Connection dialog box, a warning states that the identity of the remote computer cannot be verified, and asks if you want to continue anyway. At the bottom, the Yes button is circled.](./media/remote-desktop-connection-identity-verification-sqlserverdw.png "Remote Desktop Connection dialog")

8. Once logged in, launch the **Server Manager**. This should start automatically, but you can access it via the Start menu if it does not start.

    ![The Server Manager tile is circled in the Start Menu.](./media/start-menu-server-manager.png "Server Manager tile in the Start menu")

9. Select **Local Server**, then select **On** next to **IE Enhanced Security Configuration**.

    ![Screenshot of the Server Manager. In the left pane, Local Server is selected. In the right, Properties (For LabVM) pane, the IE Enhanced Security Configuration, which is set to On, is highlighted.](./media/windows-server-manager-ie-enhanced-security-configuration.png "Server Manager")

10. In the Internet Explorer Enhanced Security Configuration dialog, select **Off under Administrators**, then select **OK**.

    ![Screenshot of the Internet Explorer Enhanced Security Configuration dialog box, with Administrators set to Off.](./media/internet-explorer-enhanced-security-configuration-dialog.png "Internet Explorer Enhanced Security Configuration dialog box")

11. Back in the Server Manager, select **Dashboard** on the left-hand menu, then select **Add roles and features**.

    ![Add roles and features is highlighted on the Server Manager dashboard.](media/server-manager-dashboard-add-roles-and-features.png "Add roles and features")

12. Select **Next** on the Before You Begin screen.

13. Select **Role-based or feature-based installation** for the installation type, and select **Next**.

    ![Role-based or feature-based installation is selected and highlighted under Select installation type.](./media/add-roles-and-features-wizard-select-installation-type.png "Select Role-based or feature-based installation ")

14. Accept the default selection on the Select destination server screen, and select **Next**.

    ![Select a server from the server pool is selected on the Select a destination server screen, and the default value (SqlServerDW) appears in the Server Pool box.](./media/add-roles-and-features-wizard-select-destination-server.png "Accept the default selection")

15. On the Select server roles screen, select **Application Server**, and select **Next**.

    ![Application Server is selected and highlighted on the Select server roles screen.](./media/add-roles-and-features-wizard-select-server-roles.png "Select Application Server")

16. On the Select features screen, select **.NET Framework 3.5 Features**, then select **Next**.

    ![NET Framework 3.5 Features is selected and highlighted on the Select features screen.](./media/add-roles-and-features-wizard-select-features.png "Select .NET Framework 3.5 Features")

17. Select **Next** on the Application Server screen.

18. Accept the default values on the Select role services screen, and select **Next**.

19. On the Confirm installation selections screen, check the **Restart the destination server automatically if required** check box, and select **Yes** on the restart confirmation dialog.

20. Select **Install**. You may see a message about specifying an alternate source path. This can be ignored.

    ![Restart the destination server automatically if required is selected and highlighted on the Confirm installation selections screen, and Install is highlighted at the bottom.](./media/add-roles-and-features-wizard-confirm-installation-selections.png "Confirm your installation selections")

21. Close the Add Roles and Features Wizard, once the installation is completed.

22. Close the Server Manager.

### Task 7: Open port 1433 on the Windows Firewall of the SqlServerDw VM

In this task, you will add rules to the SqlServerDw VM's Windows firewall to allow access to SQL Server via port 1433 by other machines.

1. On the SqlServerDw VM, select **Start**.

    ![The Start icon is highlighted on the VM taskbar.](media/windows-2012-r2-datacenter-startbar.png "Select Start")

2. Then, select the **Search** icon in the top right-hand corner of the screen.

    ![The Search icon is highlighted on the right side.](media/windows-2012-r2-datacenter-search-icon.png "Select Search")

3. In the Search text box, enter `wf.msc`, then select **wf** from the list of results.

    ![The wf icon is highlighted in the list of search results.](./media/windows-2012-r2-datacenter-search-wf-msc.png "Search on wf.msc")

4. In the Windows Firewall with Advanced Security dialog, select, then right-click **Inbound Rules** in the left pane, then select **New Rule** in the action pane.

    ![New Rule is highlighted in the submenu for Inbound Rules, which is selected in the left pane of the Windows Firewall with Advanced Security dialog box.](./media/windows-firewall-new-inbound-rule.png "Create a new Inbound Rule")

5. In the New Inbound Rule Wizard, under Rule Type, select **Port**, then select **Next**.

    ![Rule Type is selected and highlighted on the left side of the New Inbound Rule Wizard, and Port is selected and highlighted on the right.](./media/new-inbound-rule-wizard-rule-type.png "Select Port")

6. In the Protocol and Ports dialog, use the default **TCP**, and enter **1433** in the Specific local ports text box, the select **Next**.

    ![Protocol and Ports is selected on the left side of the New Inbound Rule Wizard, and 1433 is in the Specific local ports box, which is selected on the right.](./media/new-inbound-rule-wizard-protocol-and-ports.png "Select a specific local port")

7. In the Action dialog, select **Allow the connection**, and select **Next**.

    ![Action is selected on the left side of the New Inbound Rule Wizard, and Allow the connection is selected on the right.](./media/new-inbound-rule-wizard-action.png "Specify the action")

8. In the Profile step, check **Domain**, **Private**, and **Public**, then select **Next**.

    ![Profile is selected on the left side of the New Inbound Rule Wizard, and Domain, Private, and Public are selected on the right.](./media/new-inbound-rule-wizard-profile.png "Select Domain, Private, and Public")

9. In the Name screen, enter **sqlserver**, and select **Finish**.

    ![Profile is selected on the left side of the New Inbound Rule Wizard, and sqlserver is in the Name box on the right.](./media/new-inbound-rule-wizard-name.png "Specify the name")

10. Close the Windows Firewall with Advanced Security window.

### Task 8: Provision Azure SQL Database

In this task, you will create an Azure SQL Database, which will server as the target database for migration of the on-premises WorldWideImporters database into the cloud. The Premium tier is required to support ColumnStore index creation.

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource**, enter "sql database" into the Search the Marketplace box, select **SQL Database** from the results, and select **Create**.

    ![+Create a resource is selected in the Azure navigation pane, and "sql database" is entered into the Search the Marketplace box. SQL Database is selected in the results.](media/create-resource-azure-sql-database.png "Create SQL Server")

2. On the SQL Database blade, enter the following:

    - **Database name**: Enter WorldWideImporters.

    - **Subscription**: Select the same subscription you are using for this hands-on lab.

    - **Resource Group**: Choose Use existing, and select the hands-on-lab-SUFFIX resource group.

    - **Select source**: Select Blank database.

    - **Server**: Select this, and select Create a new server, then on the New server blade, enter the following:
        - **Server name**: Enter a unique name, such as wwiSUFFIX.
        - **Server admin login**: Enter demouser.
        - **Password**: Enter Password.1!!
        - **Location**: Select the location you are using for resources in this hands-on lab.
        - **Allow Azure services to access server**: Ensure this is checked.
        - Select **OK**.

    - Under Want to use SQL elastic pool, select **Not now**.

    - **Pricing tier**: Select Premium P1: 125 DTUs, 500 GB, and select **Apply**.

        ![The Configure pricing tier for SQL Server is displayed, with Premium selected and highlighted.](media/azure-sql-database-pricing-tier-premium.png "SQL Pricing tier configuration")

    - **Collation**: Leave set to SQL_Latin1_General_CP1_CI_AS.

    ![The SQL Database blade is displayed, with the values specified above entered into the appropriate fields.](media/azure-sql-database-create.png "Create Azure SQL Database")

3. Select **Create**.

    > **Note**: The [Azure SQL Database firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) prevents external applications and tools from connecting to the server or any database on the server unless a firewall rule is created to open the firewall for the specific IP address. When creating the new server above, the **Allow azure services to access server** box was checked, which allows any services using an Azure IP address to access this server and databases, so there is no need to create a specific firewall rule for this hands-on lab. To access the SQL server from an on-premises computer or application, you need to [create a server level firewall rule](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal#create-a-server-level-firewall-rule) to allow the specific IP addresses to access the server.
    
You should follow all steps provided *before* performing the Hands-on lab.
