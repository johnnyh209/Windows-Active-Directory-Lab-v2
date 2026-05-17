# Virtual Machine Config for Windows Server

Next, I created a VM, named DC01, that my Windows Server will be running on. This will be my Domain Controller in my lab environment. Here's the configuration of said VM. </br>
<img width="704" height="533" alt="1  Windows Server VM Config" src="https://github.com/user-attachments/assets/b82e6780-09d8-4613-8f3e-8445c77c808c" /> </br>
<img width="912" height="155" alt="2  VM Overview" src="https://github.com/user-attachments/assets/eace2be4-e5d4-461d-bfc7-4f27b778d74f" /> </br>

Note that I have designated its network adapter to be connected to the Private LAN virtual switch. This will be the case for the remainder of the VMs that I create. </br>

I will also be modifying the number of virtual processors and modifying the Checkpoints settings too much like I did for my FW01 VM (pfSense). For the virtual processors, I dropped it down to 2 processors. </br>
<img width="722" height="687" alt="3  Number of Virtual Processors" src="https://github.com/user-attachments/assets/cac20028-88ac-4f57-9641-c2041ffab67e" /> </br>

Under **Checkpoints**, I disabled **Use automatic checkpoints**. </br>
<img width="722" height="687" alt="4  Checkpoints Settings" src="https://github.com/user-attachments/assets/8481d3f1-4466-40b6-a4ce-25f07c576f8f" /> </br>

# Installing Windows Server 2025

With the VM configured and created, it is time to install Windows Server 2025. So, boot up DC01 VM and run through the Windows Server installation process.

1. Select the language you will be using. I left everything on English.
2. Select keyboard input method.
3. Select your install option. Since I am installing Windows Server, I selected **Install Windows Server**. Also check the  checkbox to agree that everything on the drive will be deleted.
4. Enter in your product key. If you don't have one, you can select the option that you currently don't have a product key and you can enter in a product key at a later time.
5. Select the version of Windows Server you want to install. I am installing the standard desktop version.
6. Accept the license agreement to continue to installation.
7. Select the disk that you will be installing Windows Server on. I only have 1 disk.
8. Wait for installation to finish.
9. When install has finished, you will be creating a local administrator account.
10. You will then be brought to the log-in page. Log-in to your administrator account, and you will be asked to select to send optional diagnostic data to Microsoft or only send required data.
11. Next, you will be fully logged in and should see Server Manager:

# Final Configurations

Now that you finally have Windows Server installed, there are a few more tasks to complete.

1. We need to assign an IP to the server. In Server Manager, select **Local Server** on the left pane. Then in the **Properties** box, click on **IPv4 address assigned by DHCP, IPv6 enabled**.
2. This opens the **Network Connections** window. Right-click on the network adapter and click on **Properties**.
3. in the Properties window, select **Internet Protocol Version 4(TCP/IPv4)** and click **Properties**.
4. Give your Windows Service/Domain Controller an IP address within the subnet range of that your pfSense's LAN Adapter is in. Keep the subnet mask the same as what you set on pfSense and the default gateway should be the address to your pfSense's LAN adapter. For the DNS server, you can give it a public DNS server address.
5. You should now see that your Windows Server/Domain Controller now has network access.
6. Next, if the timezone is not correct, go ahead and adjust it.
7. Next, set the hostname to your liking. I am changing mine to DC01. This will require you to restart the system.


