# Setting Up A Member Server

In my lab environment, I want to create my own Windows image using both Deployment Workbench and Windows Deployment Services (WDS). This image will be used to install a standardized Windows on regular workstation nodes and will automatically add those nodes to the domain during the iamging process. As such, I will be using Deployment Workbench and WDS on a 
member server to further isolate my domain controller. This way, in the event that something happens to my member server(s), this isolation should minimize effects to my domain controller.

I quickly spun up another VM and installed Windows Server 2025 onto it. The VM's specs are identical to my Domain Controller VM's specs. At this point, I now have 3 VMs set up: </br>
<img width="916" height="134" alt="nylBAqWKDm" src="https://github.com/user-attachments/assets/00f2dc74-6ca6-4282-93fe-d046a4832516" /> </br>

# Joining Member Server to Domain

Before we can join a member server to a domain, we have to first adjust its network settings.

1. First, open up the **Network Connections** page either by going through Control Panel or clicking on **IPv4 address assigned by DHCP, IPv6 enabled** under Properties in Server Manager.
2. Right-Click on your network adapter and click on **Properties**.
3. Click on **Internet Protocol Version 4(TCP.IP)** and click on **Properties**.
4. Since this will be a server, I will give it a static IP within the range of the IPv4 Scope I created on my domain controller (176.26.224.1/24). For the preferred DNS, enter in the IP address of your domain controller.

With the network settings configured, it is time to join the Server to the Domain.

1. Open up System Properties. You can do so by opening up Settings > System > About and clicking on **Domain or workgroup** or click on your server's name under the **Properties** section of Server Manager.
2. In System Properties window, go to the **Computer Name** tab (which you should be on by default when opening up System Properties), and click on the **Change...** button.
3. Under the **Member of** section, select **Domain:** and enter in the fully qualified domain name of your domain.
4. Enter in the account credentials of a domain administrator to provide permission.
5. If it works, you will see confirmation like so:
6. For this change to take effect, restart your Server.

After your Server restarts and you are on the log in page, the account that you are shown is the last account this system was logged into. Which means what you are currently seeing is a local account. To log in to a domain account, click on **Other user** and enter in the username of a domain account in this format: DomainName\Username.

After logging in, I can verify that this server has now been joined to my domain: </br>
<img width="1024" height="877" alt="10  Verify Server joined to domain" src="https://github.com/user-attachments/assets/827812df-0db2-44e7-9be5-d8165fb8f23f" /> </br>
