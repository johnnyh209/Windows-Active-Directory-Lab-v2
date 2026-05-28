# Virtual Machine Config for Windows Server

Next, I created a VM, named DC01, that my Windows Server will be running on. This will be my Domain Controller in my lab environment. Here's the configuration of said VM. </br>
<img width="598" height="453" alt="1  Windows Server VM Config" src="https://github.com/user-attachments/assets/b82e6780-09d8-4613-8f3e-8445c77c808c" /> </br>
<img width="730" height="124" alt="2  VM Overview" src="https://github.com/user-attachments/assets/eace2be4-e5d4-461d-bfc7-4f27b778d74f" /> </br>

Note that I have designated its network adapter to be connected to the Private LAN virtual switch. This will be the case for the remainder of the VMs that I create. </br>

I will also be modifying the number of virtual processors and modifying the Checkpoints settings too much like I did for my FW01 VM (pfSense). For the virtual processors, I dropped it down to 2 processors. </br>
<img width="578" height="550" alt="3  Number of Virtual Processors" src="https://github.com/user-attachments/assets/cac20028-88ac-4f57-9641-c2041ffab67e" /> </br>

Under **Checkpoints**, I disabled **Use automatic checkpoints**. </br>
<img width="578" height="550" alt="4  Checkpoints Settings" src="https://github.com/user-attachments/assets/8481d3f1-4466-40b6-a4ce-25f07c576f8f" /> </br>

# Installing Windows Server 2025

With the VM configured and created, it is time to install Windows Server 2025. So, boot up DC01 VM and run through the Windows Server installation process.

1. Select the language you will be using. I left everything on English. </br>
<img width="722" height="687" alt="5  Language" src="https://github.com/user-attachments/assets/93a780b2-defa-41a4-b9cd-a8539277b236" /> </br>

2. Select keyboard input method. </br>
<img width="722" height="687" alt="6  Keyboard input method" src="https://github.com/user-attachments/assets/d6c8a222-7985-43d1-a32f-65b31ea3a72c" /> </br>

3. Select your install option. Since I am installing Windows Server, I selected **Install Windows Server**. Also check the  checkbox to agree that everything on the drive will be deleted. </br>
<img width="722" height="687" alt="7  Select to install" src="https://github.com/user-attachments/assets/fbbc22d8-8a47-40a7-9686-0abb3b31d509" /> </br>

4. Enter in your product key. If you don't have one, you can select the option that you currently don't have a product key and you can enter in a product key at a later time. </br>
<img width="722" height="687" alt="8  Enter product key" src="https://github.com/user-attachments/assets/a473923d-fa6c-4ed9-a84f-d9fec792736d" /> </br>

5. Select the version of Windows Server you want to install. I am installing the standard desktop version. </br>
<img width="722" height="687" alt="9  Select Windows Server version" src="https://github.com/user-attachments/assets/aa8c6df6-346b-440b-88c2-d9f547b2c0cb" /> </br>

6. Accept the license agreement to continue to installation. </br>
<img width="722" height="687" alt="10  License Agreement" src="https://github.com/user-attachments/assets/1774bfa0-7c38-49e9-8aa9-f55785e7a326" /> </br>

7. Select the disk that you will be installing Windows Server on. I only have 1 disk. </br>
<img width="722" height="687" alt="11  Select Disk" src="https://github.com/user-attachments/assets/f5d1c706-f66d-4baa-8319-4daad546f206" /> </br>

8. Wait for installation to finish. </br>
<img width="722" height="687" alt="12  Installing" src="https://github.com/user-attachments/assets/084a145e-ced5-4a56-b395-da6239ebaf6f" /> </br>

9. When install has finished, you will be creating a local administrator account. </br>
<img width="722" height="687" alt="13  Local Admin Account" src="https://github.com/user-attachments/assets/362aa9da-2c22-4d7a-9071-1d7e8d6d2196" /> </br>

10. You will then be brought to the log-in page. Log-in to your administrator account, and you will be asked to select to send optional diagnostic data to Microsoft or only send required data. </br>
<img width="722" height="687" alt="14  Diagnostics Data" src="https://github.com/user-attachments/assets/789d9e78-9b84-4dad-a553-ace09849f1c0" /> </br>

11. Next, you will be fully logged in and should see Server Manager: </br>
<img width="722" height="687" alt="15  Server Manager" src="https://github.com/user-attachments/assets/91780e03-fdc9-4dea-85ed-74b71f4b8325" /> </br>

# Final Configurations

Now that you finally have Windows Server installed, there are a few more tasks to complete.

1. We need to assign an IP to the server. In Server Manager, select **Local Server** on the left pane. Then in the **Properties** box, click on **IPv4 address assigned by DHCP, IPv6 enabled**. </br>
<img width="722" height="687" alt="16  Set IP" src="https://github.com/user-attachments/assets/1ba6e026-644b-4833-aade-668c7c1b2021" /> </br>

2. This opens the **Network Connections** window. Right-click on the network adapter and click on **Properties**. </br>
<img width="722" height="687" alt="16 1 Select Properties" src="https://github.com/user-attachments/assets/50547b47-5129-4651-a2d8-db7b6a497151" /> </br>

3. in the Properties window, select **Internet Protocol Version 4(TCP/IPv4)** and click **Properties**. </br>
<img width="722" height="687" alt="16 2 IPv4 Properties" src="https://github.com/user-attachments/assets/7051fe57-75a6-410d-8d98-2f4edfcccf0f" /> </br>

4. Give your Windows Service/Domain Controller an IP address within the subnet range of that your pfSense's LAN Adapter is in. Keep the subnet mask the same as what you set on pfSense and the default gateway should be the address to your pfSense's LAN adapter. For the DNS server, you can give it a public DNS server address. </br>
<img width="722" height="687" alt="16 3 IPv4 Settings" src="https://github.com/user-attachments/assets/3dafc9c8-a1ab-4a5f-9a77-5a079ceb7fc0" /> </br>

5. You should now see that your Windows Server/Domain Controller now has network access. </br>
<img width="722" height="687" alt="16 4 Network Connectivity" src="https://github.com/user-attachments/assets/53e5115b-cb70-4efb-8fa0-85c4cb294175" /> </br>

6. Next, if the timezone is not correct, go ahead and adjust it. </br>
<img width="722" height="687" alt="17  Timezone" src="https://github.com/user-attachments/assets/e5952e86-cf35-48e4-8f84-c8bbacee87ed" /> </br>
<img width="722" height="687" alt="17 1 Timezone" src="https://github.com/user-attachments/assets/73193e81-e49d-47c1-aa28-42f850a43570" /> </br>

7. Next, set the hostname to your liking. I am changing mine to DC01. This will require you to restart the system. </br>
<img width="722" height="687" alt="18  Change hostname" src="https://github.com/user-attachments/assets/40d1e169-bfeb-4fa7-97ae-12a0773c1f65" /> </br>
<img width="722" height="687" alt="18 2 Change hostname" src="https://github.com/user-attachments/assets/0403cdfc-6899-4120-88e9-b91b35284a5e" /> </br>
<img width="722" height="687" alt="18 3 Change hostname" src="https://github.com/user-attachments/assets/4d6a04cf-9419-4fba-8b94-f81049d3de26" /> </br>

# Domain Controller

We will now promote this Windows Server to be the Domain Controller of our lab environment.

1. Back on Server Manager, click on **Manage** then click on **Add Roles and Features**. </br>
<img width="722" height="687" alt="19  Domain Controller" src="https://github.com/user-attachments/assets/220f172b-6cf0-4a7d-ac1d-1b2f3c1e1761" /> </br>

2. In the **Before you begin** page, click **Next**. </br>
<img width="722" height="687" alt="19 1 Domain Controller" src="https://github.com/user-attachments/assets/561ba699-a6ec-4a23-aae9-ca194f1cc9fa" /> </br>

3. Select **Role-based or feature-based installation**. </br>
<img width="722" height="687" alt="19 2 Domain Controller" src="https://github.com/user-attachments/assets/2df1647d-2ea7-4d9a-bd60-c199337ce53a" /> </br>

4. Select **Select a server from the server pool** and select the server of your choosing. At this stage, we only have one, which is this Windows Server we just installed. </br>
<img width="722" height="687" alt="19 3 Domain Controller" src="https://github.com/user-attachments/assets/b3b12e86-8de8-450b-b529-c1dd0d3694fb" /> </br>

5. Select **Active Directory Domain Services**. This will open a new window; simply press **Add Features** button. Then click **Next**. </br>
<img width="722" height="687" alt="19 4 Domain Controller" src="https://github.com/user-attachments/assets/dce74699-7c07-4926-9809-072ced9629cc" /> </br>

6. In the **Select features** page, you can click **Next**. </br>
<img width="722" height="687" alt="19 5 Domain Controller" src="https://github.com/user-attachments/assets/2f50c0f1-5fce-470e-a794-0347da105d1c" /> </br>

7. In the **Active Directory Domain Services** page, click **Next**. </br>
<img width="722" height="687" alt="19 6 Domain Controller" src="https://github.com/user-attachments/assets/507f07de-8c42-4e3a-a8f1-3532d333db7c" /> </br>

8. Check the box to restart the system if required, and then install. Wait until installation completes. </br>
<img width="722" height="687" alt="19 7 Domain Controller" src="https://github.com/user-attachments/assets/b8a4de8c-0ff9-4799-84a0-bc5584a65b36" /> </br>
<img width="722" height="687" alt="19 8 Domain Controller" src="https://github.com/user-attachments/assets/8fc48dcc-9cdd-4f25-b530-7272698a1726" /> </br>

9. After installation is completed, click on the notification flag symbol and then click on **Promote this server to a domain controller**. </br>
<img width="722" height="687" alt="19 9 Domain Controller" src="https://github.com/user-attachments/assets/aa83b340-2bb0-4cfe-94e8-9235c3af0be7" /> </br>

10. Since there doesn't exist a forest yet, select **Add a new forest** and provide a root domain name. </br>
<img width="722" height="687" alt="19 10 Domain Controller" src="https://github.com/user-attachments/assets/460148c4-a6df-4a18-a012-8cc7256f6890" /> </br>

11. Set a password for Directory Services Restore Mode. </br>
<img width="722" height="687" alt="19 11 Domain Controller" src="https://github.com/user-attachments/assets/3a37e61b-9bbf-42c3-b0d4-ce6541a77a1e" /> </br>

12. For **DNS Options** page, we can click **Next**. </br>
<img width="722" height="687" alt="19 12 Domain Controller" src="https://github.com/user-attachments/assets/d18173c6-e2ba-42a7-a6ff-e2190a754792" /> </br>

13. The NetBIOS domain name will automatically fill in. Click **Next**. </br>
<img width="722" height="687" alt="19 13 Domain Controller" src="https://github.com/user-attachments/assets/9bda91f2-45e3-4b89-bd81-3d90214fae10" /> </br>

14. Nothing needs to be done on **Paths** page. Click **Next**. </br>
<img width="722" height="687" alt="19 14 Domain Controller" src="https://github.com/user-attachments/assets/20f71b32-68e2-4345-80f9-6feb38f71bb4" /> </br>

15. Review everything and then install. Note that the server will reboot afterwards. </br>
<img width="722" height="687" alt="19 15 Domain Controller" src="https://github.com/user-attachments/assets/842b6548-1d24-41aa-b6a4-1eae6ced001a" /> </br>
<img width="722" height="687" alt="19 16 Domain Controller" src="https://github.com/user-attachments/assets/73698f30-cdfd-4b47-bd15-b826c721974f" /> </br>

16. After restarting, you will now see your domain name before your account's name like so: </br>
<img width="722" height="687" alt="19 17 Domain Controller" src="https://github.com/user-attachments/assets/2f8f80ab-7f89-4e54-8639-f9f58831cf2d" /> </br>

# DHCP Setup

The final thing we need to configure on our Domain Controller is a DHCP server.

1. Back on Server Manager, click on **Manage** then click on **Add Roles and Features**.
2. Run through the **Add Roles and Features** wizard. When you get the **Select server roles** page, selet **DHCP Server**.
3. Once DHCP is installed, click on the notification flag symbol and then click on **Complete DHCP configuration**.
4. In the **Description** page, click **Next**.
5. In the **Authorization** page, select the user credential that you will use to authorize the creation/configuration of this DHCP server.
6. Once done, open the DHCP console. You can do so by clicking on the Search bar and typing in DHCP.
7. In the left pane, expand your server and you should see IPv4 and IPv6. Right-click on IPv4 and select **New Scope**.
8. Provide a name (and description if you want) for this new scope.
9. Fill out the start and end IP address for the IP Address range. This means when this scope is active, this DHCP server will provide other nodes in the LAN an IP address within this range. The range will match that of the IP address I assigned to the LAN Adapter on my pfSense VM.
10. Add any IP addresses you want to be excluded so that the DHCP server will not assign those. I have included the IP addresses of the LAN adapter on my pfSense VM and the IP address of my Domain Controller.
11. Set the lease duration of an IP address. For example, if the duration is 8 days, a device is assigned an IP address and that assignment should last 8 days before the device has to renew or give up the IP address.
12. Choose yes to configure the DHCP options now.
13. For the default gateway, we want to provide it the IP address of our pfSense router/firewall.
14. For the DNS server, makes sure the information automatically filled out points to your domain controller.
15. You can leave the WINS Server page blank.
16. Select to make scope active now.
