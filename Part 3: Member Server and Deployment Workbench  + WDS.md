# Setting Up A Member Server

In my lab environment, I want to create my own Windows image using both Deployment Workbench and Windows Deployment Services (WDS). This image will be used to install a standardized Windows on regular workstation nodes and will automatically add those nodes to the domain during the iamging process. As such, I will be using Deployment Workbench and WDS on a 
member server to further isolate my domain controller. This way, in the event that something happens to my member server(s), this isolation should minimize effects to my domain controller.

I quickly spun up another VM and installed Windows Server 2025 onto it. The VM's specs are identical to my Domain Controller VM's specs. At this point, I now have 3 VMs set up: </br>
<img width="916" height="134" alt="nylBAqWKDm" src="https://github.com/user-attachments/assets/00f2dc74-6ca6-4282-93fe-d046a4832516" /> </br>


