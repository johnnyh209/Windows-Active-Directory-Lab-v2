# Windows-Active-Directory-Lab-v2

A couple years back, I dipped my toes into using Virtual Box and configuring an Active Directory domain for the first time. It was documented in this [repository](https://github.com/johnnyh209/Active-Directory).

For a while now, I have been wanting to redo the entire lab. This time, I decided to use Hyper-V, and configured a more robust network using pfSense.

The documentation of my lab is currently broken down into 4 parts:

Part 1. Configuring pfSense: The idea here is to have my pfSense VM with 2 network adapter, one WAN adapter and one LAN adapter. The WAN adapter will be connected to my host machine's physical network adapter, while the LAN adapter will have the remainder of my VMs connected to it. This way, all VMs in the LAN will have Internet access upstream.

Part 2. Setting up Windows Server Domain Controller: I install and configure Windows Server 2025 and creating my own Active Directory. This VM also acts as my domain controller.

Part 3. Member Server Setup: I wanted to set up a server that would handle heavy workloads (such as file sharing, application hosting) rather than having everything on my domain controller like I did in the first iteration of the lab. This server, currently, is where I am hosting my SCCM set up.

Part 4. Configuration Manager: I installed and configured a Configuration Manager/SCCM instance on my member server. The goal is to both deploy applications to regular Windows 11 clients in the domain, and to setup a custom Windows image for Operating System Deployment (OSD).
