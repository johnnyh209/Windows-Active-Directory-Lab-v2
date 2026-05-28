# Preparing Active Directory for SCCM

We need to first create the System Management container in our Active Directory, which is used by SCCM to store configuraiton data that clients use to identify management points, software distribution points, and site boundaries.

1. To do this, on your domain controller, open **ADSI Edit**. </br>
<img width="722" height="687" alt="1  Open ADSI Edit" src="https://github.com/user-attachments/assets/4c8ad5c0-d206-4ed1-b6ef-43442a578069" /> </br>

2. Right-click ADSI Edit on the left pane and click on **Connect to...**. </br>
<img width="722" height="687" alt="2  Connect To" src="https://github.com/user-attachments/assets/9c2fab53-030e-49e9-8b57-50d575c5a07c" /> </br>

3. In the Conenction Settings window, keep everything as is and click the **OK** button. </br>
<img width="722" height="687" alt="3  Connection Settings" src="https://github.com/user-attachments/assets/027fad2e-3aec-48a2-950e-32e19d02ae63" /> </br>

4. On the left pane of ADSI Edit, expand each subdirectory until you see **CN=System**. Right-Click it, and then click on New > Object. </br>
<img width="722" height="687" alt="4  New Object" src="https://github.com/user-attachments/assets/ee2f8eeb-2b3d-4126-a66a-1da1459d21f6" /> </br>

5. In the Create Object that opens as a result, select **Container** and click **Next**. </br>
<img width="722" height="687" alt="5  Select Container" src="https://github.com/user-attachments/assets/39339325-d622-4602-8aeb-6e295a7d8069" /> </br>

6. In the **Value:** box, enter in **System Management**. </br>
<img width="722" height="687" alt="6  System Management" src="https://github.com/user-attachments/assets/8d638876-7fe9-4a62-9c37-188e227c432d" /> </br>

7. Click **Finish**. </br>
<img width="722" height="687" alt="7  Click Finish" src="https://github.com/user-attachments/assets/b454bf3b-85bc-45f8-a74b-95306fb41170" /> </br>

8. Refresh ADSI Edit and you should now see a the System Management container under **CN=System**. </br>
<img width="722" height="687" alt="8  Container created" src="https://github.com/user-attachments/assets/8f95662e-7622-4b0e-ae58-1c44bb1145cd" /> </br>

# Assigning Permissions to the System Management Container

1. In ADSI Edit, right-click on **CN=System Management**, then click on **Properties**. </br>
<img width="722" height="687" alt="1  System Management Properties" src="https://github.com/user-attachments/assets/cde4ce80-172e-4324-9bcf-ab6b42de05d0" /> </br>

2. Click on the **Security** tab and click on the **Add...** button. </br>
<img width="722" height="687" alt="2  Click on add" src="https://github.com/user-attachments/assets/69c52f9e-5b6f-413e-a54a-c53b9162fa4a" /> </br>

3. Click on **Object Types...** button, and select **Computers**. </br>
<img width="722" height="687" alt="3  Object Types button" src="https://github.com/user-attachments/assets/feee1dbf-3f63-45a4-a8ba-26d294dc0cd1" /> </br>
<img width="722" height="687" alt="3 1 Select computers" src="https://github.com/user-attachments/assets/3796a860-77bd-4614-b772-ee8628340c9b" /> </br>

4. Type in the name of the system your SCCM server will be installed on and click **Check Names** button. It should find your device easily. </br>
<img width="722" height="687" alt="4  Type in server name" src="https://github.com/user-attachments/assets/d93a91c5-2f9b-45c6-8990-9cbaa532680f" /> </br>

5. Click on **Advanced** button. </br>
<img width="722" height="687" alt="5  Click Advanced" src="https://github.com/user-attachments/assets/0a15c33c-564f-4db6-b8fa-843aae79f0b5" /> </br>

6. Select your server, and click **Edit**. </br>
<img width="722" height="687" alt="6  Select server and edit" src="https://github.com/user-attachments/assets/d8c808d9-b09b-43b4-a2c9-e38ec4016203" /> </br>

7. In the **Applies to:** drop-down box, select **This object and all descendant objects**. </br>
<img width="722" height="687" alt="7  Applies to box" src="https://github.com/user-attachments/assets/16968405-2541-4039-89b8-86def44b4a80" /> </br>

8. Check the **Full control** box, and everything else should be selected as well. </br>
<img width="722" height="687" alt="8  Full Control" src="https://github.com/user-attachments/assets/7ef8f539-304c-4f1b-b500-fec8515e02bf" /> </br>

9. Click **Apply** and **OK**. </br>
<img width="722" height="687" alt="9  Apply" src="https://github.com/user-attachments/assets/1351f623-8c4e-41ab-b1bd-c2acb73587e3" /> </br>

10. Click **Apply** and **OK** again. </br>
<img width="722" height="687" alt="10  Apply again" src="https://github.com/user-attachments/assets/1bcf7b30-0c28-4fb9-a498-d304967c13ef" /> </br>

# Disk setup for Configuration Manager

1. In my member server VM, named Server2025, I created 7 virtual disks </br>
<img width="722" height="687" alt="1  Disks Created" src="https://github.com/user-attachments/assets/991028a6-39f3-4441-bacc-c35685d9909b" /> </br>

2. In our member server VM, open Disk Management, right-click on each disk we created and bring them Online </br>
<img width="722" height="687" alt="2  Bring disks online" src="https://github.com/user-attachments/assets/c624de3a-c63f-4f51-a6e5-8f1d569362c0" /> </br>

3. After bringing the disks online, you should now see that they say "Not Initialized." Right-click one of the disks, and then click on **Initialize Disk**. This opens an Initialize Disk window, allowing you to select which disks to initialize, and which partition style to use. </br>
<img width="722" height="687" alt="3  Initialize all disks" src="https://github.com/user-attachments/assets/6c8fc583-56cb-4c67-aace-ed4d295a0b67" /> </br>
<img width="722" height="687" alt="3 1 Initialize disks" src="https://github.com/user-attachments/assets/6532929c-9565-452a-9af4-7789dc82c1d6" /> </br>

4. For each disk, you will now need to create a Volume / format the disk with a file system. </br>
<img width="722" height="687" alt="4  Create Volume" src="https://github.com/user-attachments/assets/923c47a5-4c10-4904-a380-8889bcf59c78" /> </br>
<img width="722" height="687" alt="4 1 Create Volume" src="https://github.com/user-attachments/assets/3811f341-3c17-4215-adad-d0c85f9cb485" /> </br>
<img width="722" height="687" alt="4 2 Create Volume" src="https://github.com/user-attachments/assets/5b2e657c-c001-4435-8b5a-76080224f8bf" /> </br>
<img width="722" height="687" alt="4 3 Create Volume" src="https://github.com/user-attachments/assets/1d0ebf12-49c8-4bbe-9ba3-d773292a83d9" /> </br>
<img width="722" height="687" alt="4 4 Create Volume" src="https://github.com/user-attachments/assets/1998b9a1-7701-404d-84f8-d2de5636264e" /> </br>

5. After creating a Volume for each disk, I rename them to help easily know what each disk is for. </br>
<img width="722" height="687" alt="5  Rename Disk" src="https://github.com/user-attachments/assets/3d8e1795-6c09-4c24-9a3e-d52a8df115bb" /> </br>
<img width="756" height="292" alt="6  All disks created" src="https://github.com/user-attachments/assets/9b577312-1e4b-4736-9bff-57ba994a5d77" /> </br>


# NO_SMS_ON_DRIVE.SMS File

Before continuing, we want to make sure to have a **NO_SMS_ON_DRIVE.SMS** file in any of our drives that we do not want any files from SCCM's Content Library to be copied to. 

1. First, create a a text file on your C:\ drive </br>
<img width="722" height="687" alt="7  No SMS file" src="https://github.com/user-attachments/assets/24e43d67-b0c5-41bd-9cec-df7b33baccef" /> </br>

2. Name the file as **NO_SMS_ON_DRIVE.SMS**. </br>
<img width="722" height="687" alt="7 1 Name the SMS file" src="https://github.com/user-attachments/assets/a1b61c25-6f88-47be-8e26-91b5ab6a72d6" /> </br>
<img width="722" height="687" alt="7 2 Extension change warning" src="https://github.com/user-attachments/assets/cafc66cd-e7ad-481b-90ac-8b420ff73aa5" /> </br>

3. You will be prompted a warning about changing file extension. Click Yes to close the warning box and you should see the file's type change from Text Document to SMS File. </br>
<img width="722" height="687" alt="8  Database folder" src="https://github.com/user-attachments/assets/b118ba40-8a6d-40b4-93e6-3c74db0794db" /> </br>

4. I will be copying that file onto all of the other drives I created except for my **SCCM_ContentLibrary** drive. </br>
<img width="722" height="687" alt="9  Install IIS" src="https://github.com/user-attachments/assets/94c013e1-5a51-46da-bb9e-6ff8250de596" /> </br>

5. Create a folder named Database onto the root of every drive that will be used for the different databases that will be used in SCCM. So this would be in my **SCCM_SQL_MDF**, **SCCM_SQL_LDF**, **SCCM_TempDB**, and **SQL_WSUS_Database** drives. </br>
<img width="722" height="687" alt="9 1 Install IIS Success" src="https://github.com/user-attachments/assets/0ee58d1a-45d1-40bf-8682-150f666687ca" /> </br>

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

1. Launch your SQL Server installer, and select **Custom**. </br>
<img width="722" height="687" alt="1  SQL Server Install" src="https://github.com/user-attachments/assets/21c16b76-d664-4aed-a1dd-af52f649834d" /> </br>

2. Specify the location that you want the media to be installed onto. Then wait for the install to finish. </br>
<img width="722" height="687" alt="2  Install location" src="https://github.com/user-attachments/assets/884e4179-0053-4212-a502-04d06d50573e" /> </br>
<img width="722" height="687" alt="2 1 Installing" src="https://github.com/user-attachments/assets/890de500-5565-49a5-8f1c-42311aa2c572" /> </br>

3. Click on **Installation** on the left pane, and then click on **New SQL Server standalone installation or add features to an existing installation**. </br>
<img width="722" height="687" alt="3  Installation" src="https://github.com/user-attachments/assets/ddd100bd-1ad9-492e-b281-7f7ffc404393" /> </br>

4. Specify your free edition (if testing in a lab), or enter in billing information for the subscription option, or a product key if you have one. </br>
<img width="722" height="687" alt="4  Edition" src="https://github.com/user-attachments/assets/db53c65b-d16c-403a-9298-a0a7e562f942" /> </br>

5. Read through and accept license agreement to continue. </br>
<img width="722" height="687" alt="5  License agreement" src="https://github.com/user-attachments/assets/cb861f7b-3e6d-4444-a799-f18c4b1a17a6" /> </br>

6. If you want, check the box to **Use Microsoft Update to check for updates**. </br>
<img width="722" height="687" alt="6  Updates" src="https://github.com/user-attachments/assets/cd9a6a2d-285c-48a5-8e7b-c654309ed895" /> </br>

7. Here, you will see a warning for Windows Firewall. We will need to ensure the necessary rules are in place to allow traffic through. The required ports are the following: 1433, 1434, 4022, and 135. </br>
<img width="722" height="687" alt="7  Install Rule" src="https://github.com/user-attachments/assets/ebf15293-e674-47ff-b542-9f1342a1e9fa" /> </br>

8. Let's set up some firewall rules to allow traffic inbound through those ports. Open up PowerShell and run the following commands:
```
New-NetFirewallRule -DisplayName “SQL Server” -Direction Inbound –Protocol TCP –LocalPort 1433 -Action allow
New-NetFirewallRule -DisplayName “SQL Admin Connection” -Direction Inbound –Protocol TCP –LocalPort 1434 -Action allow
New-NetFirewallRule -DisplayName “SQL Database Management” -Direction Inbound –Protocol UDP –LocalPort 1434 -Action allow
New-NetFirewallRule -DisplayName “SQL Service Broker” -Direction Inbound –Protocol TCP –LocalPort 4022 -Action allow
New-NetFirewallRule -DisplayName “SQL Debugger/RPC” -Direction Inbound –Protocol TCP –LocalPort 135 -Action allow
```
<img width="722" height="687" alt="8  Commands for ports enable" src="https://github.com/user-attachments/assets/faae062f-7883-4cf7-a853-2493b8b4b19d" /> </br>

9. You can enable Azure Extension. Since we won't be touching Azure, I leave it disabled. </br>
<img width="722" height="687" alt="9  Azure Extension" src="https://github.com/user-attachments/assets/0e2f2062-5d69-498e-a43b-1dc59eb5cb10" /> </br>

10. In Feature Selection, I am only enabled **Database Engine Services**. </br>
<img width="722" height="687" alt="10  Feature Selection" src="https://github.com/user-attachments/assets/2018d7bc-71cb-4a28-8cae-c0896a654c45" /> </br>

11. I will leave the instance name as default. Change it to your liking. </br>
<img width="722" height="687" alt="11  Instance Name" src="https://github.com/user-attachments/assets/74a02a88-8ff3-4379-9d8c-00c26effe2f5" /> </br>

12. Next, we will designate a service account for SQL Server Agent and SQL Server Database Engine. I have created an admin account for this, named SCCMAdmin. Make sure that your account you are using has been added to the Domain Admin and Administrators group. I also enabled Server Agent to have Automatic startup. </br>
<img width="722" height="687" alt="12  Service Accounts" src="https://github.com/user-attachments/assets/6e2c618b-1864-444c-b24a-9b8548db3917" /> </br>

13. In **Database Engine Configuration**, I will be keeping Authentication Mode as **Windows authentication mode**, and adding admin accounts I want to have unrestricted access to SQL Server. </br>
<img width="722" height="687" alt="13  Database Engine Configuration" src="https://github.com/user-attachments/assets/a778cbf1-3c72-4144-89e8-d55174d1fdbe" /> </br>

For Data Directories, we will keep the directories as default. </br>
<img width="722" height="687" alt="13 1 Data Directories" src="https://github.com/user-attachments/assets/0966321f-ccd1-4dd7-b4a6-06a986f0b2e4" /> </br>

What we are changing is TempDB. We are changing the Autogrowth for both TempDB data files and log files from 64 MB to 256 MB, and changing the directory to our H:\ drive that we aptly named TempDB. </br> 
<img width="722" height="687" alt="13 2 TempDB" src="https://github.com/user-attachments/assets/6fbd9974-8914-49bb-abaa-b463f18c2af5" /> </br>

15. Install and wait for the process to complete. </br>
<img width="722" height="687" alt="14  Install Success" src="https://github.com/user-attachments/assets/d839968d-f50b-4e32-b14a-74ddf175108d" /> </br>

# Power BI Report Server

With SQL Server 2025, Power BI Report Server (PBRS) replaces SQL Sever Reporting Services (SSRS). You can Grab PBRS [here](https://www.microsoft.com/en-us/download/details.aspx?id=105943&culture=en-us&country=us).

1. Run the PBRS installer and select **Instal Power BI Report Server**. </br>
<img width="722" height="687" alt="1  PBRS Installer" src="https://github.com/user-attachments/assets/f68a54e5-f117-4612-a4a5-b6a80a91e4f9" /> </br>

2. Choose your edition, or enter in a product key if you have one. </br>
<img width="722" height="687" alt="2  Edition selection" src="https://github.com/user-attachments/assets/2575c32e-8bcb-4abe-be19-83758a83aa80" /> </br>

3. Read and accept the license agreement to continue. </br>
<img width="722" height="687" alt="3  License Agreement" src="https://github.com/user-attachments/assets/d5a883f4-08cb-425a-add6-fded49cbff5a" /> </br>

4. Since we already installed the Database Engine earlier (when we installed SQL Server), simply click **Next**. </br>
<img width="722" height="687" alt="4  Database Engine" src="https://github.com/user-attachments/assets/bfa0233c-788f-4bd1-9ce2-aa1143064976" /> </br>

5. Choose your installation path. I will be leaving it default. </br>
<img width="722" height="687" alt="5  Installation location" src="https://github.com/user-attachments/assets/4a982606-77ea-4179-a81a-ac173631ac32" /> </br>

6. Install and wait for completion. Then click on **Configure report server**. </br>
<img width="722" height="687" alt="6  Finish Install" src="https://github.com/user-attachments/assets/84a4d7d1-0209-4139-a194-e2f285bfe958" /> </br>

7. Specify the Server Name and Report Server Instance, and connect to it. We will leave PBRS as it for now. </br>
<img width="722" height="687" alt="7  Server Name and Instance" src="https://github.com/user-attachments/assets/b195e5d9-adcd-4ff2-b811-1ccff113ae1c" /> </br>

# Installing SQL Server Management Studio

We now need to install SQL Server Management Studio (SSMS), which allows us to view our SQL database(s). You can find SSMS [here](https://learn.microsoft.com/en-us/ssms/install/install).

1. Run the SSMS installer, which will open up Visual Studio Instller. Click **Continue**. </br>
<img width="722" height="687" alt="1  SSMS Install" src="https://github.com/user-attachments/assets/4cf202c0-c68d-4df2-9823-25da2eda5a8f" /> </br>

2. Choose your optional components, and click **Install**. </br>
<img width="722" height="687" alt="2  Optional Components and Install" src="https://github.com/user-attachments/assets/d4295fb8-1942-41e2-901d-08b824f74da0" /> </br>

3. Once installation is complete, restart your system. </br>
<img width="722" height="687" alt="3  Restart" src="https://github.com/user-attachments/assets/3bda8606-eefe-4b5a-b4e9-415656b2832b" /> </br>

4. After restarting, launch SQL Server Management Studio. Enter in your Server Name, Authentication information, and connect. </br>
<img width="722" height="687" alt="4  Run SSMS" src="https://github.com/user-attachments/assets/058bdd17-805d-4dd9-a33b-3d04fb545803" /> </br>

5. Under Object Explorer, right-click on your server, and click **Properties**. </br>
<img width="722" height="687" alt="5  Properties" src="https://github.com/user-attachments/assets/7237f32f-c07f-46d4-a8ae-c2f3aa300b9c" /> </br>

6. Under **Memory**, set the minimum and maximum memory. It is recommended to set at least 8 GB minimum, and 90% of your system's total memory as the maximum. Then reboot once more. </br>
<img width="722" height="687" alt="6  Set Memory" src="https://github.com/user-attachments/assets/830e5e3a-ee90-4620-b859-667d7295b5f9" /> </br>

7. After restarting, install the latest cumulative update for SQL Server. </br>
<img width="722" height="687" alt="7  SQL Server Cumulative Update" src="https://github.com/user-attachments/assets/57532c89-5e09-484a-b8e0-fb10818f460c" /> </br>

8. Select the features you want installed. I left everything selected. </br>
<img width="722" height="687" alt="9  Select Feature" src="https://github.com/user-attachments/assets/00bd24f6-7f05-49d0-9e6c-dc36b0157dff" /> </br>

9. Wait for the check to finish, and continue. </br>
<img width="722" height="687" alt="10  Check" src="https://github.com/user-attachments/assets/14062a17-ec5e-4a9b-9a0f-1c8b0aa42dd6" /> </br>

10. Proceed with the update and wait for it to finish installing. </br>
<img width="722" height="687" alt="11  Update Success" src="https://github.com/user-attachments/assets/62dff9c8-09b5-4401-b216-0b82faeac222" /> </br>

# Installing WSUS

Next, we will install WSUS. WSUS will allow us to centrally manage and deploy software updates.

1. On our SCCM Server, Server2025, we go to Add Roles and Features.
<img width="1024" height="877" alt="1  Add Roles" src="https://github.com/user-attachments/assets/775fe51b-2d92-4336-b3cf-03f7338821a2" />

2. Selet **Role-Based or feature-based installation**.
<img width="1024" height="877" alt="2  Installation Type" src="https://github.com/user-attachments/assets/325a7b48-4c15-4a83-b0e9-9d3ef6d6531c" />

3. Our server should be selected automatically, as it is the only server listed. Click Next.
<img width="1024" height="877" alt="3  Server Selection" src="https://github.com/user-attachments/assets/bee221fe-3d95-4f32-a08f-01f31b14b3ab" />

4. Select Windows Server Update Services.
<img width="1024" height="877" alt="4  Select WSUS" src="https://github.com/user-attachments/assets/dae845a4-baed-4a2c-8136-e5e1e4e0d80c" />

5. There is nothing that needs selecting in **Select features**. Continue on.
<img width="1024" height="877" alt="5  Select Features" src="https://github.com/user-attachments/assets/763dee86-670b-402d-a13f-8444d801cc02" />

6. In **Select role services** for WSUS, uncheck WID Connectivity and check **SQL Server Connectivity**.
<img width="1024" height="877" alt="6  Role Services" src="https://github.com/user-attachments/assets/db20d09f-a48c-43a4-a94c-9de1a86f21d7" />

7. Set the location you want to store updates in. I have selected my SCCM_ContentLibrary drive (K:\), directly to a folder I created named WSUS.
<img width="1024" height="877" alt="7  Set location" src="https://github.com/user-attachments/assets/d16de0c5-8032-46e1-98a3-c8be2ea23a5d" />

8. Specify your database server.
<img width="1024" height="877" alt="8  Specify Database Server" src="https://github.com/user-attachments/assets/0414f7f4-632f-40a7-8e4e-a5b3539e4c87" />

9. Run the install and wait for completion.
<img width="1024" height="877" alt="9  Run Install" src="https://github.com/user-attachments/assets/7c15b50a-cdb8-4a65-828a-10caec0f9377" />

10. Finally, run the Post-Installation tasks.
<img width="1024" height="877" alt="10  Run Post Installation Tasks" src="https://github.com/user-attachments/assets/99c45494-810b-4eb1-96b2-26526df7e23c" />

11. According to Microsoft, it is good practice to set the WSUSPool Queue Length to 2000 and increase WsusPool Private Memory limit by 4 times. This is done in IIS. So open IIS Manager.
<img width="1024" height="877" alt="11  IIS Manager" src="https://github.com/user-attachments/assets/caf68a75-cc0a-42d6-bc91-6242bf251664" />

12. On the left pane, navigate to your server > Application Pool. Then right-click **WsusPool** and click on **Advanced Settings**.
<img width="1024" height="877" alt="12  Advanced Settings" src="https://github.com/user-attachments/assets/a3ae8fda-66f6-4021-b290-4e9a5f76c255" />

13. Change the **Queue Length** from the 1000 default value to 2000.
<img width="1024" height="877" alt="13  Change Queue Length" src="https://github.com/user-attachments/assets/b1b6b34a-1d78-4c92-b21f-ff1d0147a852" />

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

1. Run **ConfigMgr_2509.exe**. Designate where to extract the files to. Then press Extract.
2. Open up the folder that has been extracted. Then navigate to SMSSETUP\BIN\X64. Look for **extadsch.exe**.
3. While holding Shift key, right-click on **extadsch.exe** and click on **Run as different user**.
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

# Installing Config Manager

1. Return to the folder that we extracted from **ConfigMgr_2509.exe**. Within that folder, run **splash.hta**.
2. Click **Install**.
3. Click **Next**.
4. For our small lab environment, we choose **Install a Configuration Manager Primary site**.
5. If you have a product key, enter in your product key. Otherwise, select **Install the evaluation edition of this product**.
6. Read and accept the license terms to continue.
7. Since I don't already have existing prerequisite files, select to download required files. Wait for the download to finish.
8. Select your language.
9. Select client language.
10. Enter a site code, site name, and intallation path. I am setting my installation path to my E:\ that I created to house Config Manager.
11. Select to **Install the primary site as a stand-alone site**.
12. Your SQL Server name and Database name should already be prefilled. Since I don't have a custom instance name, I am leaving it blank. Keep Service Broker Port as is: 4022.
13. Enter in a path for your SQL Server data file and log file. Since I did create separate drives for them at the beginning, I will be using those drives.
14. We will install SMS provider on this machine, so nothing needs to be changed.
15. Select **Configure the comunication method on each site system role**.
16. We will install both management point and distribution point on this server.
17. On **Diagnostic and Usage Data**, read and hit next.
18. We will have the installer setup a Service Point Connection.
19. Review summary and go through Prerequisite Check.
20. Once the check has completed successfully, click on **Begin Install**.

# Post Installation: PBRS

With Configuration Manager now installed, there's some post-installation tasks that need to be done. First will be with PowerBI Report Services.

1. Under **Service Account**, specify an account to run report server service.
2. Under **Web Service URL**, I left everything as is and clicked on **Apply**. This should have created a Report Server Web Service URL.
3. Under **Database**, click on **Change Database**.
Then, select **Create a new report server database**.
The required information should be autofilled. Click on **Test Connection** to make sure connection is successful.
Leave the **Database** page as is, and continue on.
For **Credentials**, we will leave as Service Credentials.
Initiate and wait for the wizard to finish configuring the reporting database.
5. In **Web Portal URL**, click **Apply** button to create a URL that can be used to access the web portal.

# Post Installation: Configuration Manager Site System Roles

1. In Microsoft Configuration Manager, navigate through Administration > Site Configuration > Servers and Site System Roles.
2. Right-Click on your listed server and click on **Add Site System Roles**.
3. I don't have a proxy set up, so I will be leaving the **Proxy** page blank.
5. In **System Role Selection**, I selected the following roles: Endpoint Protection point, Fallback status point, Reporting services point, and Software update point.
6. In **Software Update Point**, we will leave the WSUS port numbers as 8530 and 8531.
7. **Proxy and Account Settings** page can be left blank, especially since WSUS is on the same system as well.
8. In **Synchronization Source** page, leave everything as default.
9. In **Synchronization Schedule** page, I will choose to enable synchronization on a schedule, every 1 day.
10. I will be leaving everything in **Supersedence Rules** as default.
11. In **WSUS Maintenance**, I will check to **Decline expired updates in WSUS according to supersedence rules** and **Remove obsolete updates from the WSUS database**.
12. I will leave **Maximum Run Time** page as default.
13. In **Update Files**, I will opt for **Download full fiels for all approved updates**.
14. In **Classifications**, I chose the following: Crticial Updates, Definition Updates, Security Updates, Service Packs, Update Rollups, Updates, and Upgrades.
15. In **Products**, I select Windows 7.
16. In **Languages**, choose whatever languages you desire. I am leaving it only on English.
17. I will leave **Fallback Status Point** as is.
18. In **Reporting services point**, click on the **Verify** button to verify the site database server connection, and set an account that the reporting services will use to connect to the Configuration Manager site database.
19. For **Cloud Protection Service**, I will choose **Basic membership**.
20. Install and wait for completion.

# Post Installation: Client Push

Client Push is the server-side installation method used to push the SCCM client onto target machines.

1. Back in Configuration Manager, navigate to Administration > Site Configuration > Sites. Right-click your site, and click Client Installation Settings > Client Push Installation.
2. Go to the **Accounts** tab, and click on the yellow star and click on **New Account**.
3. For this, I created a separate domain account, aptly named sccmpush, to be used solely for pushing the Config Manager client. Then hit Apply.
4. We will now move to our Domain Controller. Here, we will create a Group Policy that will allow for our sccmpush account to be a local administrator on clients to be able to push the agent/console out to. My workstations in this domain will be added to a my Workstations OU, and is where I will apply this Group Policy to. Right-click the OU you will be applying the policy to, and click on **Create a GPO in this domain**.
5. Once the policy is created, right-click the policy and click on **Edit**.
6. We will navigate the tree through this path: Computer Configuration > Preferences > Control Panel Settings > Local Users and Groups. Then, on the bottom of the window, click on **Standard**.
7. Right-click on the right pane, click on **New** then click on **Local Group**.
8. Leave the **Action:** field as **Update**, and select **Administrators** for the **Group Name:** field. Then add our sccmpush account to **Members:** section.
9. Next, in the same Group Policy editor window, go to Computer Configuration > Policies > Windows Settings > Security Settings > Windows Defender Firewall with Advanced Security. Then right-click **Inbound Rule** and click on **New Rule**.
10. Select **Predefined** and then select **Windows Management Instrumentation (WMI)** from the drop-down menu.
11. Leave **Predefined Rule** as is, which by default, should have everything selected.
12. In **Action** page, we will select **Allow the connection** and then click **Finish**.
13. Then create a new Inbound Rule again. We will select **Predefined:** again and select **File and Printer Sharing** from the drop-down menu.

# Post Installation: Boundary + Boundary Groups

Boundaries are defined network locations, and boundary groups tell clients which site/distribution point to get content from.

1. In Configuration Manager, go to Administration > Hierarchy Configuration > Boundaries. Right-click Bounadaries and click on **Create Boundary**.
2. Enter in a description to your liking. I opted to use IP address range, and gave it my lab's IP range. Click Apply, then Ok to create the boundary.
3. On the left pane, right-click on **Boundary Groups** and click on **Create Boundary Group**.
4. Provide a name, and a description (if you want).
5. On the **References** tab, add our SCCM system to the **Site system servers** field.
6. We can also create another boundary group for site assignment to offer more granularity in the setup. Create a new Boundary Group and give it a name that reflects that this boundary group is for. Then, in the Boundaries field, add in the IP range Boundary we created earlier.
7. In the **References** tab, check the box **Use this boundary group for site assignment**. Then, select the site you want to assign the IP range to.

# Post Installation: Configure Client Discovery

Configuring this will allow SCCM to discover clients in our site.

1. In Configuration Manager, go to Administration > Hierarchy Configuration > Discovery Methods. You will see see various methods listed. We will setup **Active Direcotry System Discovery**, so double-click to open its properties.
2. In the General tab, check the box to **Enable Active Directory System Discovery**.
3. Then, click on the yellow star and select where you want to discover your systems/clients at (whether you want to discover everything from within the domain, or from a specific OU). I will select my Workstation OU.
4. In the Polling Schedule tab, set how often you want Config Manager to poll your domain for computer data. I am setting it to poll everyday at 12 AM.
5. We can leave the remaining tabs as default.

# Post Installation: Client Settings

1. In Configuration Manager, go to Administration > Client Settings. Then in the ribbon on top, click on **Create Custom Client Device Settings**.
2. In the Create Custom Client Device Settings wizard, I added the following: Client Policy, Computer Agent, Endpoint Protection, Hardware Inventory, Software Center, and Software Updates.
3. In Client Policy, I changed the polling interval from 60 minutes to 15 minutes, and kept other settings as default.
4. For Computer Agent, I added an organization name to be displayed in Software Center and changed the various reminder intervals as follows:
5. In Endpoint Protection, turn on **Manage Endpoint Protection client on client computers** which should enable the other settings; keep other settings as default.
6. For Hardware Inventory, I kept everything as default.
7. In Software Center, I selected Yes for **Select these new settings to specify company information**.
Then, click on the Customize button. Edit whatever you like. I added in a company name
8. In Software Updates, I left everything as default.
9. Once done, click OK to confirm everything and have your custom client settings created.
