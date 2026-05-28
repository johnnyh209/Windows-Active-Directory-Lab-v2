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
<img width="722" height="687" alt="13  License Agreement" src="https://github.com/user-attachments/assets/8da91782-828b-476b-8e47-a60fd2851758" /> </br>

2. In the next page, select **Install**. </br>
<img width="722" height="687" alt="14  Select Install" src="https://github.com/user-attachments/assets/780770a3-a372-4573-85f6-7e20a9b37fe9" /> </br>

3. Select **OK** to set up network for installation. </br>
<img width="722" height="687" alt="15  Select OK" src="https://github.com/user-attachments/assets/6d681941-3358-4ed3-aecf-3f62ce4854d9" /> </br>

4. Select the WAN interface to use. </br>
<img width="722" height="687" alt="16  Select WAN Interface" src="https://github.com/user-attachments/assets/9fa8816c-5eab-47c0-8b34-ba56a08dc9f1" /> </br>

5. Modify settings for the WAN interface if needed. Otherwise, select **OK**. </br>
<img width="722" height="687" alt="17  Network Mode Setup" src="https://github.com/user-attachments/assets/6d654551-ad85-4fd3-a931-730b25ec98c4" /> </br>
<img width="722" height="687" alt="17 1 Interface" src="https://github.com/user-attachments/assets/5e8e2d76-a124-421d-9cdc-4bc86eedd498" /> </br>

6. Netgate Installer will try to detect if you have an active pfSense Plus subscribtion. We won't need one for this lab, so go ahead and continue to install CE. </br>
<img width="722" height="687" alt="19  Subscription Detect" src="https://github.com/user-attachments/assets/5e3fa061-a335-4b35-967d-dc8a8299b65f" /> </br>

7. We will keep the file system as ZFS, so no changes are needed here. Click **OK** to continue with installation. </br>
<img width="722" height="687" alt="20  File System Settings" src="https://github.com/user-attachments/assets/df96679c-0e74-4cd3-a0ff-aa711ea0b759" /> </br>

8. For the fact that this is a lab, I opted to have no redundancy (very bad for live production environments). </br>
<img width="722" height="687" alt="21  Redundancy" src="https://github.com/user-attachments/assets/ab1c9029-f98c-4cba-be13-93d075f9b539" /> </br>

9. Select the disk to install pfSense on. </br>
<img width="722" height="687" alt="22  Select Disk" src="https://github.com/user-attachments/assets/85c4af90-19da-4de6-aadf-9c7e2608bfbc" /> </br>

10. Netgate Installer will give a final warning that all current contents on the disk will be destroyed. If everything is good to go, click **Yes** to continue with installation. </br>
<img width="722" height="687" alt="23  Final Confirmation" src="https://github.com/user-attachments/assets/a7951d32-a958-46f8-af5f-f49d0f19b21a" /> </br>

11. Select which version of pfSense CE to install. I will be installing the latest version, 2.8.1. </br>
<img width="722" height="687" alt="24  pfSense Version Select" src="https://github.com/user-attachments/assets/6863313e-583f-4663-b186-42a8215d012c" /> </br>

12. Once installation is finished, reboot when prompted. </br>
<img width="722" height="687" alt="25  Install Progress" src="https://github.com/user-attachments/assets/bca9cc90-6abd-4f5f-a130-1f63594306ea" /> </br>
<img width="722" height="687" alt="26  Reboot" src="https://github.com/user-attachments/assets/d8ecd306-db85-4868-902a-98baf3a357be" /> </br>

13. Once rebooted, you will be asked if you want to set up VLANs. I will be selecting **No**. </br>
<img width="722" height="687" alt="27  VLAN Setup" src="https://github.com/user-attachments/assets/a4d095ac-1edd-4e20-8cd2-f004ed319e09" /> </br>

14. Enter in the WAN interface you will be using. I only have one here. </br>
<img width="722" height="687" alt="28  WAN Interface Select" src="https://github.com/user-attachments/assets/ed890fea-b07a-43b8-b8e8-c20dbcec9f54" /> </br>

15. Enter your LAN interface. If you don't have one like me at this point, press Enter to continue. </br>
<img width="722" height="687" alt="29  LAN Interface" src="https://github.com/user-attachments/assets/1b33c299-f036-4d23-a89a-ec2236411812" /> </br>

16. Do a final confirmation of the interfaces you have selected. </br>
<img width="722" height="687" alt="30  Final Interface Confirmation" src="https://github.com/user-attachments/assets/303a2551-eeb9-4327-82d1-564e2d9088f5" /> </br>

17. Once complete, you should see that your WAN interface has been assigned an IP address. You should see a screen similar to this: </br>
<img width="722" height="687" alt="31  Configuring Interface" src="https://github.com/user-attachments/assets/c827700e-483c-42d9-a470-633c8649398a" /> </br>

18. Now, go ahead and shutdown the pfSense VM. Then, open up the VM's settings and add a new Network Adapter. This new Network Adapter will be connected to our Private LAN virtual switch that we created at the beginning. </br>
<img width="722" height="687" alt="33  2nd Network Adapter" src="https://github.com/user-attachments/assets/a915e489-d76d-4baf-b8b9-e011d9eee745" /> </br>

19. Boot up the pfSense VM again. On the main menu, select option 1. </br>
<img width="722" height="687" alt="34  Select Option 1" src="https://github.com/user-attachments/assets/70bfb2f1-e263-441d-a428-c422a0dd056d" /> </br>

Notice here that you now see 2 interfaces like mine. </br>
<img width="722" height="687" alt="35  2 interfaces" src="https://github.com/user-attachments/assets/2127bc53-380e-4415-b752-053b5f0e308f" /> </br>

20. You will be asked to setup VLANs again. Choose No again. </br>

21. Enter in your WAN interface again. </br>
<img width="722" height="687" alt="36  WAN interface again" src="https://github.com/user-attachments/assets/d2d15bf6-bbc1-4ec0-9cc7-3f3bc571c504" /> </br>

22. Enter in your LAN interface. This was the step we left blank earlier. </br>
<img width="722" height="687" alt="37  LAN Interface" src="https://github.com/user-attachments/assets/45a78c0e-8671-4d1b-9e81-c3b3de40f7b7" /> </br>

23. Confirm that the correct interface has been selected for both WAN and LAN. </br>
<img width="722" height="687" alt="38  Interface Selection Confirmation" src="https://github.com/user-attachments/assets/2b97e365-1899-4277-a634-e8ccb2841c08" /> </br>

24. You will be brought back to the main menu once this is completed. Now select option 2: **Set interface(s) IP address**.
25. Select option 2 to configure our LAN interface. </br>
<img width="722" height="687" alt="39  Configure IP for LAN" src="https://github.com/user-attachments/assets/e1c230af-efe4-4405-b8d2-cb125b000f57" /> </br>

26. You will be asked if you want to use DHCP to give your LAN interface an IPv4 address. Select No and then enter in whatever IP address you want to give it. </br>
<img width="722" height="687" alt="40  LAN Interface IP" src="https://github.com/user-attachments/assets/14d78d1c-9e75-4c5a-8f5f-897d163df91a" /> </br>

27. Enter in the subnet bit for the LAN subnet. I gave it /24. </br>
<img width="722" height="687" alt="41  Subnet Bit" src="https://github.com/user-attachments/assets/928e7ca3-220a-4f87-bf92-761d7d95c8fc" /> </br>

28. We don't need to give it any gateway address, so leave blank and press Enter. </br>
<img width="722" height="687" alt="42  Press Enter" src="https://github.com/user-attachments/assets/cc771d3e-d28b-4f80-b933-42459cef7250" /> </br>

29. We don't need an IPv6 address, type **n** and press Enter. </br>
<img width="722" height="687" alt="43  No IPv6" src="https://github.com/user-attachments/assets/47e8f113-b607-407c-ae58-780143756779" /> </br>
<img width="722" height="687" alt="43 1 No IPv6" src="https://github.com/user-attachments/assets/879e3a7d-1317-4984-bc4d-86b8ab12c8c2" /> </br>

30. You will then be asked if you want to enable DHCP server on LAN. I will be selecting No because I will be setting up a DHCP server on Windows Server. </br>
<img width="722" height="687" alt="44  No DHCP" src="https://github.com/user-attachments/assets/cb703147-0072-4bfd-8129-abfa6e3b4123" /> </br>

31. Select **y** when asked if you want to revert to HTTP as the webConfigurator protofol. This will allow us to use a browser to access the web configurator. </br>
<img width="722" height="687" alt="45  Web Configurator" src="https://github.com/user-attachments/assets/f3910661-1cc4-4604-abae-f6d78a5d289f" /> </br>

32. Now you should see that both WAN and LAN adapters are online. This is all the configuring we should need to do on pfSense. </br>
<img width="722" height="687" alt="46  Final Look" src="https://github.com/user-attachments/assets/a95737c3-045e-44bb-80f5-750bd2a03271" /> </br>


