# Preparing Active Directory for SCCM

We need to first create the System Management container in our Active Directory, which is used by SCCM to store configuraiton data that clients use to identify management points, software distribution points, and site boundaries.

1. To do this, on your domain controller, open **ADSI Edit**.
2. Right-click ADSI Edit on the left pane and click on **Connect to...**.
3. In the Conenction Settings window, keep everything as is and click the **OK** button.
4. On the left pane of ADSI Edit, expand each subdirectory until you see **CN=System**. Right-Click it, and then click on New > Object.
5. In the Create Object that opens as a result, select **Container** and click **Next**.
6. In the **Value:** box, enter in **System Management**.
7. Click **Finish**.
8. Refresh ADSI Edit and you should now see a the System Management container under **CN=System**.

# Assigning Permissions to the System Management Container

1. In ADSI Edit, right-click on **CN=System Management**, then click on **Properties**.
2. Click on the **Security** tab and click on the **Add...** button.
3. Click on **Object Types...** button, and select **Computers**.
4. Type in the name of the system your SCCM server will be installed on and click **Check Names** button. It should find your device easily.
5. Click on **Advanced** button.
6. Select your server, and click **Edit**.
7. In the **Applies to:** drop-down box, select **This object and all descendant objects**.
8. Check the **Full control** box, and everything else should be selected as well.
9. Click **Apply** and **OK**.
10. Click **Apply** and **OK** again.

# Disk setup for Configuration Manager

1. In my member server VM, named Server2025, I created 7 virtual disks
2. In our member server VM, open Disk Management, right-click on each disk we created and bring them Online
3. After bringing the disks online, you should now see that they say "Not Initialized." Right-click one of the disks, and then click on **Initialize Disk**. This opens an Initialize Disk window, allowing you to select which disks to initialize, and which partition style to use.
4. For each disk, you will now need to create a Volume / format the disk with a file system.
5. After creating a Volume for each disk, I rename them to help easily know what each disk is for. 

# NO_SMS_ON_DRIVE.SMS File

Before continuing, we want to make sure to have a **NO_SMS_ON_DRIVE.SMS** file in any of our drives that we do not want any files from SCCM's Content Library to be copied to. 

1. First, create a a text file on your C:\ drive
2. Name the file as **NO_SMS_ON_DRIVE.SMS**.
3. You will be prompted a warning about changing file extension. Click Yes to close the warning box and you should see the file's type change from Text Document to SMS File.
4. I will be copying that file onto all of the other drives I created except for my **SCCM_ContentLibrary** drive.
5. Create a folder named Database onto the root of every drive that will be used for the different databases that will be used in SCCM. So this would be in my **SCCM_SQL_MDF**, **SCCM_SQL_LDF**, **SCCM_TempDB**, and **SQL_WSUS_Database** drives.

# Installing IIS

According to Microsoft, "Several Configuration Manager site system roles require the use of Internet Information Services (IIS)." This next step will be for us to install IIS, as well as Background Intelligent Transfer Service (BITS), and Remote Differential Compression (RDC).

IIS is the web server for SCCM, which hosts Management Points and Distribution Points.
BITS manages the communication between a client and the server.
RDC handles file synchronization. Rather than having a client to download an entire file or an update, RDC compares the signature of the file/update currently on the client with the signature of the update, and download only the parts that have changed between what is currently on the client and the update.

To install all 3 components, simply run the following command in PowerShell:
```
Install-WindowsFeature Web-Static-Content,Web-Default-Doc,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Redirect,Web-Net-Ext,Web-ISAPI-Ext,Web-Http-Logging,Web-Log-Libraries,Web-Request-Monitor,Web-Http-Tracing,Web-Windows-Auth,Web-Filtering,Web-Stat-Compression,Web-Mgmt-Tools,Web-Mgmt-Compat,Web-Metabase,Web-WMI,BITS,RDC
```

# Installing SQL Server

We will be using the Standard Developer edition of SQL Server 2025 as it's perfet for a non-production environment. Grab SQL Server [here](https://www.microsoft.com/en-us/evalcenter/download-microsoft-endpoint-configuration-manager).

1. Launch your SQL Server installer, and select **Custom**.
2. Specify the location that you want the media to be installed onto. Then wait for the install to finish.
3. Click on **Installation** on the left pane, and then click on **New SQL Server standalone installation or add features to an existing installation**.
4. Specify your free edition (if testing in a lab), or enter in billing information for the subscription option, or a product key if you have one.
5. Read through and accept license agreement to continue.
6. If you want, check the box to **Use Microsoft Update to check for updates**.
7. Here, you will see a warning for Windows Firewall. We will need to ensure the necessary rules are in place to allow traffic through. The required ports are the following: 1433, 1434, 4022, and 135.
8. Let's set up some firewall rules to allow traffic inbound through those ports. Open up PowerShell and run the following commands:
```
New-NetFirewallRule -DisplayName “SQL Server” -Direction Inbound –Protocol TCP –LocalPort 1433 -Action allow
New-NetFirewallRule -DisplayName “SQL Admin Connection” -Direction Inbound –Protocol TCP –LocalPort 1434 -Action allow
New-NetFirewallRule -DisplayName “SQL Database Management” -Direction Inbound –Protocol UDP –LocalPort 1434 -Action allow
New-NetFirewallRule -DisplayName “SQL Service Broker” -Direction Inbound –Protocol TCP –LocalPort 4022 -Action allow
New-NetFirewallRule -DisplayName “SQL Debugger/RPC” -Direction Inbound –Protocol TCP –LocalPort 135 -Action allow
```
9. You can enable Azure Extension. Since we won't be touching Azure, I leave it disabled.
10. In Feature Selection, I am only enabled **Database Engine Services**.
11. I will leave the instance name as default. Change it to your liking.
12. Next, we will designate a service account for SQL Server Agent and SQL Server Database Engine. I have created an admin account for this, named SCCMAdmin. Make sure that your account you are using has been added to the Domain Admin and Administrators group. I also enabled Server Agent to have Automatic startup.
13. In **Database Engine Configuration**, I will be keeping Authentication Mode as **Windows authentication mode**, and adding admin accounts I want to have unrestricted access to SQL Server. 
For Data Directories, we will keep the directories as default.
What we are changing is TempDB. We are changing the Autogrowth for both TempDB data files and log files from 64 MB to 256 MB, and changing the directory to our H:\ drive that we aptly named TempDB. 
15. Install and wait for the process to complete.

# Power BI Report Server

With SQL Server 2025, Power BI Report Server (PBRS) replaces SQL Sever Reporting Services (SSRS). You can Grab PBRS [here](https://www.microsoft.com/en-us/download/details.aspx?id=105943&culture=en-us&country=us).

1. Run the PBRS installer and select **Instal Power BI Report Server**.
2. Choose your edition, or enter in a product key if you have one.
3. Read and accept the license agreement to continue.
4. Since we already installed the Database Engine earlier (when we installed SQL Server), simply click **Next**.
5. Choose your installation path. I will be leaving it default.
6. Install and wait for completion. Then click on **Configure report server**.
7. Specify the Server Name and Report Server Instance.
8. Under **Service Account**, specify an account to run report server service.
9. Under **Database**, click on **Change Database**.
Then, select **Choose an existing report server database**.
Leave Server Name as is (which should already have been prefilled with your server's hostname), and choose an Authentication Type (whether you want the already filled in Current User, or a SQL Server Account
Leave the **Database** page as is, and continue on.
11. adsf
12. asdf

# Installing SQL Server Management Studio

We now need to install SQL Server Management Studio (SSMS), which allows us to view our SQL database(s). You can find SSMS [here](https://learn.microsoft.com/en-us/ssms/install/install).

1. Run the SSMS installer, which will open up Visual Studio Instller. Click **Continue**.
2. Choose your optional components, and click **Install**.
3. Once installation is complete, restart your system.
4. After restarting, launch SQL Server Management Studio. Enter in your Server Name, Authentication information, and connect.
5. Under Object Explorer, right-click on your server, and click **Properties**.
6. Under **Memory**, set the minimum and maximum memory. It is recommended to set at least 8 GB minimum, and 90% of your system's total memory as the maximum. Then reboot once more.
7. After restarting, install the latest cumulative update for SQL Server.
8. Select the features you want installed. I left everything selected.
9. Wait for the check to finish, and continue.
10. Proceed with the update and wait for it to finish installing.

# Installing WSUS

Next, we will install WSUS. WSUS will allow us to centrally manage and deploy software updates.

1. On our SCCM Server, Server2025, we go to Add Roles and Features.
2. Selet **Role-Based or feature-based installation**.
3. Our server should be selected automatically, as it is the only server listed. Click Next.
4. Select Windows Server Update Services.
5. There is nothing that needs selecting in **Select features**. Continue on.
6. In **Select role services** for WSUS, uncheck WID Connectivity and check **SQL Server Connectivity**.
7. Set the location you want to store updates in. I have selected my SCCM_ContentLibrary drive (K:\), directly to a folder I created named WSUS.
8. Specify your database server.
9. Run the install and wait for completion.
10. Finally, run the Post-Installation tasks.
11. According to Microsoft, it is good practice to set the WSUSPool Queue Length to 2000 and increase WsusPool Private Memory limit by 4 times. This is done in IIS. So open IIS Manager.
12. On the left pane, navigate to your server > Application Pool. Then right-click **WsusPool** and click on **Advanced Settings**.
13. Change the **Queue Length** from the 1000 default value to 2000.
14. Change the **Private Memory Limit** to 4x what is set by default.
15. Because I created a separate drive for WSUS Database, I will have to change the default database path that has been set. First, stop WsusPool in IIS Manager and Wsus Service in Services.
16. Open SQL Management Studio. Navigate to [Server] > Databases > SUSDB. Right-Click SUSDB and go to Properties.
17. Under Files, you should see the default Path. Copy that path and navigate to it in File Explorer.
18. Now that we have the path opened in File Explorer, go and right-click on SUSDB, click on Tasks > Detach.
19. Select Drop Connections and then click OK.
20. Back in File Explorer, in the path we navigated to, Move the **SUSDB.mdf** and **SUSDB_log.ldf** files to our WSUS drive, which is my I:\ drive named SQL_WSUS_Database.
21. Back in SQL Manager, right-click Databases and click Attach.
22. Click on the **Add...** button.
23. Navigate to our I:\Database file path, select SUSDB.mdf, and click OK.
24. Click OK to readd SUSDB.
25. Start WSUS Service and WsusPool.

# Installing Windows ADK

Get the Windows ADK and Windows PE add-on installers [here](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install).

1. Run the **adksetup.exe**. We wil leave the default install path.
2. Select Yes or No to allow Microsoft to collect insights.
3. Read through and accept the license agreement to continue.
4. Select the features needed, and Install.
5. Once that is complete, run **adkwinpesetup.exe**.
6. Leave the location specification as it is and click Next.
7. Select Yes or No to allow Microsoft to collect insights.
8. Read through and accept the license agreement to continue.
9. There is only one feature to select to install, which is WinPE.

# Extend Active Directory Schema

For this lab, we will be using the Technical Preview version of Configuration Manager. Download the installer [here](https://www.microsoft.com/en-in/evalcenter/evaluate-microsoft-endpoint-configuration-manager-technical-preview). While this will be used to install Configuration Manager, we also need an .exe in here that will extend our Active Directory Schema.

1. Run **ConfigMgr_TechPreview2411.exe**. Designate where to extract the files to. Then press Extract.
2. Open up the folder that has been extracted. Then navigate to SMSSETUP\BIN\X64. Look for **extadsch.exe**.
3. While holding Shift key, right-click on **extadsch.exe** and click on **Rune as different user**.
4. Enter in the credentials of an account with the necessary rights to extend an AD schema.
5. You will see CMD open up briefly and close. To verify that the .exe worked successfully, go to C:\ and find the **ExtADSch.log**.
6. Open the log file and find the entry that states that the Active Directory schema has been extended successfully.

# Create SCCM Databases

Let's create our SCCM databases before we finally install Configuration Manager.

1. Back in SQL Server Management Studio, right-click Databases and click on **New Database**.
2. Enter in a database name and database owner.
3. From the initial 1 database filea and 1 database log file created automatically, I added 3 more database files. This means I have 1 database log, and 4 database files in total. For each of the database files, I set their initial size to 256 MB (for a small lab like mine, this should be fine), and the log file to 512 MB. For the autogrowth size, the 4 database files are set to 128 MB and the log file to 256 MB.
4. Remember at the beginning I also create separate drives for the SQL Server MDF database files and 1 for the log files. As such, I set the path for the 4 database files to point to my MDF drive (F:\Database in my case) and my log file to my LDF drive (G:\Database in my case).
5. Finally, set the Recovery Model to **Simple**.
