# Configuring Virtual Switch

For this lab, 2 virtual switches are required: 1 switch will be for the upstream connection (connected to the WAN) and 1 switch will be for the local network that our virtual machines will be connected to.

Here's my configuration of the WAN virtual switch. Notice that this switch is connected to the network adapter of the host machine. This is what allows the upstream traffic. </br>
<img width="540" height="512" alt="1  WAN switch" src="https://github.com/user-attachments/assets/c03f6658-7de6-4f53-b697-9b0b368d75e0" />

Here's my configuration of the LAN virtual switch. This switch will not be connected to any external adapters and is what my other virtual machines will be connected to directly. </br>
<img width="540" height="512" alt="2  LAN Switch" src="https://github.com/user-attachments/assets/43a2cb47-4ede-4199-9fea-a570023439e2" />

# Creating + Configuring Virtual Machine for pfSense

With the virtual switches created, we move to configuring the virtual machine (VM) that we will install pfSense onto.

1. I will be naming this VM as **FW01** </br>
<img width="704" height="533" alt="3  pfSense VM Name" src="https://github.com/user-attachments/assets/6f933b78-6254-4c8c-b5fa-893f5c80d2e7" /> </br>
2. I will be leaving this VM on Generation 2 architecture. </br>
<img width="704" height="533" alt="4  Generation 2" src="https://github.com/user-attachments/assets/4a6db56f-d621-4ba0-97ba-7f12928c4e8f" /> </br>
3. We will be using ZFS storage, which requires 4 to 8 GB of memory. I am opting for 4 GB of memory for this lab. Make sure to also unselect **Use Dynamic Memory for this virtual machine**.</br>
<img width="704" height="533" alt="5  Memory Allocation" src="https://github.com/user-attachments/assets/8d789ff9-8b80-4318-845b-63daa2c9d328" /> </br>
4. Connect this VM to the WAN virtual switch. </br>
<img width="704" height="533" alt="6  Connect to External Switch" src="https://github.com/user-attachments/assets/8f64ada5-5e82-4899-8867-08b0a24d7ef3" /> </br>
5. Configure the virtual disk that will be the storage for the VM. This this will have nothing more than pfSense installed, I will give it 20 GB of storage. </br>
<img width="704" height="533" alt="7  VM Disk" src="https://github.com/user-attachments/assets/7161ffb3-8494-49df-baa5-c2a5fe22f252" /> </br>
6. We will have this VM boot from a pfSense .iso so that we can go straight to installing pfSense once we boot up this VM for the first time. </br>
<img width="704" height="533" alt="4  VM Creation" src="https://github.com/user-attachments/assets/ed51312a-7154-4d25-882d-bf7784587fc3" /> </br>
<img width="1194" height="263" alt="9  VM Created" src="https://github.com/user-attachments/assets/abde49c6-342c-43d3-a8cc-883aa2ea7797" /> </br>

With the VM finally created, we want to modify some of the default settings as well. In our FW01's VM Settings, navigate to the **Processor** tab. By default, the VM is assigned 12 virtual processors. This is wholly unnecessary for this VM's purpose and I will be dropping it down to 1 virtual processor. </br>
<img width="722" height="687" alt="10  Virtual Processor" src="https://github.com/user-attachments/assets/ce65fc44-2566-4a44-8566-79c430fd11f9" /> </br>

If you are using Generation 2 architecture, make sure to disable Secure Boot. </br>
<img width="722" height="687" alt="10  Disable Secure Boot" src="https://github.com/user-attachments/assets/42b0b3f0-5e5d-46d4-858f-b839b1a71a49" /> </br>

For the final change, go to the **Checkpoints** tab, uncheck **User automatic checkpoints**. </br>
<img width="722" height="687" alt="11  Checkpoints Setting" src="https://github.com/user-attachments/assets/efa8d6b6-bba7-4f20-bc9f-f96d7d986188" /> </br>

# Configuring pfSense

A couple years ago, pfSense has moved away from publishing .iso files for pfSense and has moved to distributing their own installer called Netgate Installer. This is what is being used in this lab to install the latest version of pfSense.

1. After booting up the VM, wait for processes to run and you will come to the Copyright and Trademark page. Hit **Enter** to accept the notice to continue on. </br>
<img width="1024" height="877" alt="13  License Agreement" src="https://github.com/user-attachments/assets/8da91782-828b-476b-8e47-a60fd2851758" /> </br>

2. In the next page, select **Install**. </br>
<img width="1024" height="877" alt="14  Select Install" src="https://github.com/user-attachments/assets/780770a3-a372-4573-85f6-7e20a9b37fe9" /> </br>

3. Select **OK** to set up network for installation. </br>
<img width="1024" height="877" alt="15  Select OK" src="https://github.com/user-attachments/assets/6d681941-3358-4ed3-aecf-3f62ce4854d9" /> </br>

4. Select the WAN interface to use. </br>
<img width="1024" height="877" alt="16  Select WAN Interface" src="https://github.com/user-attachments/assets/9fa8816c-5eab-47c0-8b34-ba56a08dc9f1" /> </br>

5. Modify settings for the WAN interface if needed. Otherwise, select **OK**. </br>
<img width="1024" height="877" alt="17  Network Mode Setup" src="https://github.com/user-attachments/assets/6d654551-ad85-4fd3-a931-730b25ec98c4" /> </br>
<img width="1024" height="877" alt="17 1 Interface" src="https://github.com/user-attachments/assets/5e8e2d76-a124-421d-9cdc-4bc86eedd498" /> </br>

6. Netgate Installer will try to detect if you have an active pfSense Plus subscribtion. We won't need one for this lab, so go ahead and continue to install CE.

7. We will keep the file system as ZFS, so no changes are needed here. Click **OK** to continue with installation.
8. For the fact that this is a lab, I opted to have no redundancy (very bad for live production environments).
9. Select the disk to install pfSense on.
10. Netgate Installer will give a final warning that all current contents on the disk will be destroyed. If everything is good to go, click **Yes** to continue with installation.
11. Select which version of pfSense CE to install. I will be installing the latest version, 2.8.1.
12. Once installation is finished, reboot when prompted.
13. Once rebooted, you will be asked if you want to set up VLANs. I will be selecting **No**.
14. Enter in the WAN interface you will be using. I only have one here.
15. Enter your LAN interface. If you don't have one like me at this point, press Enter to continue.
16. Do a final confirmation of the interfaces you have selected.
17. Once complete, you should see that your WAN interface has been assigned an IP address. You should see a screen similar to this:
18. Now, go ahead and shutdown the pfSense VM. Then, open up the VM's settings and add a new Network Adapter. This new Network Adapter will be connected to our Private LAN virtual switch that we created at the beginning.
19. Boot up the pfSense VM again. On the main menu, select option 1.
Notice here that you now see 2 interfaces like mine.
20. You will be asked to setup VLANs again. Choose No again.
21. Enter in your WAN interface again.
22. Enter in your LAN interface. This was the step we left blank earlier.
23. Confirm that the correct interface has been selected for both WAN and LAN.
24. You will be brought back to the main menu once this is completed. Now select option 2: **Select interface(s) IP address**.
25. Select option 2 to configure out LAN interface
26. You will be asked if you want to use DHCP to give your LAN interface an IPv4 address. Select No and then enter in whatever IP address you want to give it.
27. Enter in the subnet bit for the LAN subnet. I gave it /24.
28. We don't need to give it any gateway address, so leave blank and press Enter.
29. We don't need an IPv6 address, type **n** and press Enter.
30. You will then be asked if you want to enable DHCP server on LAN. I will be selecting No because I will be setting up a DHCP server on Windows Server.
31. Select **y** when asked if you want to revert to HTTP as the webConfigurator protofol. This will allow us to use a browser to access the web configurator.
32. Now you should see thatb oth WAN and LAN adapters are online. This is all the configuring we should need to do on pfSense.


