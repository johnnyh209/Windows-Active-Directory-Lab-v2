# Setting Up A Member Server

In my lab environment, I want to use Configuration Manager to deploy a Windows image. This image will be used to install a standardized Windows on regular workstation nodes and will automatically add those nodes to the domain during the iamging process. As such, I will be configuring SCCM on a 
member server to further isolate my domain controller. This way, in the event that something happens to my member server(s), this isolation should minimize effects to my domain controller.

I quickly spun up another VM and installed Windows Server 2025 onto it. The VM's specs are identical to my Domain Controller VM's specs. At this point, I now have 3 VMs set up: </br>
<img width="916" height="134" alt="nylBAqWKDm" src="https://github.com/user-attachments/assets/00f2dc74-6ca6-4282-93fe-d046a4832516" /> </br>

# Joining Member Server to Domain

Before we can join a member server to a domain, we have to first adjust its network settings.

1. First, open up the **Network Connections** page either by going through Control Panel or clicking on **IPv4 address assigned by DHCP, IPv6 enabled** under Properties in Server Manager.
<img width="1024" height="877" alt="1  Open Network Connections" src="https://github.com/user-attachments/assets/cbde2432-c4a1-464a-9389-fff29eedae12" />

2. Right-Click on your network adapter and click on **Properties**.
<img width="1024" height="877" alt="2  Open Adapter&#39;s Property" src="https://github.com/user-attachments/assets/8a93679c-a530-4c67-b5f3-7b6491e7b56b" />

3. Click on **Internet Protocol Version 4(TCP.IP)** and click on **Properties**.
<img width="1024" height="877" alt="3  IPv4 Properties" src="https://github.com/user-attachments/assets/64e998c9-7b40-46d6-af5c-7e7e36cd1d0b" />

4. Since this will be a server, I will give it a static IP within the range of the IPv4 Scope I created on my domain controller (176.26.224.1/24). For the preferred DNS, enter in the IP address of your domain controller.
<img width="1024" height="877" alt="4  IPv4 Settings" src="https://github.com/user-attachments/assets/d940b81a-73f6-4fa8-a4e8-45207c6046c0" />

With the network settings configured, it is time to join the Server to the Domain.

1. Open up System Properties. You can do so by opening up Settings > System > About and clicking on **Domain or workgroup** or click on your server's name under the **Properties** section of Server Manager.
<img width="1024" height="877" alt="5  Open System Properties" src="https://github.com/user-attachments/assets/2ad483e8-e587-4e63-acc2-c82a9e306fc6" />

2. In System Properties window, go to the **Computer Name** tab (which you should be on by default when opening up System Properties), and click on the **Change...** button.
<img width="1024" height="877" alt="6  Open up Change" src="https://github.com/user-attachments/assets/a8fa0de5-871e-43e3-aabc-f758b7f4f27f" />

3. Under the **Member of** section, select **Domain:** and enter in the fully qualified domain name of your domain.
<img width="1024" height="877" alt="7  Enter in Domain name" src="https://github.com/user-attachments/assets/17a889ae-da65-4034-8a23-c5260094baf3" />

4. Enter in the account credentials of a domain administrator to provide permission.
<img width="1024" height="877" alt="8  Provide credentials for permission" src="https://github.com/user-attachments/assets/90f06a8a-b094-4b1d-9582-a1f4e85a1499" />

5. If it works, you will see confirmation like so:
<img width="1024" height="877" alt="9  Confirmation it worked" src="https://github.com/user-attachments/assets/f765b737-97ac-41a6-86ab-7a9b7e3adf63" />

6. For this change to take effect, restart your Server.
<img width="1024" height="877" alt="10  Verify Server joined to domain" src="https://github.com/user-attachments/assets/25959a14-5945-45f4-9d3e-7285606a3e09" />

After your Server restarts and you are on the log in page, the account that you are shown is the last account this system was logged into. Which means what you are currently seeing is a local account. To log in to a domain account, click on **Other user** and enter in the username of a domain account in this format: DomainName\Username.

After logging in, I can verify that this server has now been joined to my domain: </br>
<img width="1024" height="877" alt="10  Verify Server joined to domain" src="https://github.com/user-attachments/assets/827812df-0db2-44e7-9be5-d8165fb8f23f" /> </br>
