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
