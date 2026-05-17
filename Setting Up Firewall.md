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

2. Select Generation 1 as pfSense is built on Linux </br>
<img width="704" height="533" alt="4  Gen 1 Selection" src="https://github.com/user-attachments/assets/5d0d3023-de90-481d-8348-335b97e32304" /> </br>

3. As this is a lab environment, 1024 MB (1 GB) of memory should be sufficient for my use. </br>
<img width="704" height="533" alt="5  Memory allocation for FW VM" src="https://github.com/user-attachments/assets/2b735b9d-439f-47d8-988c-31cd912e4822" /> </br>

4. Connect this VM to the WAN virtual switch. </br>
<img width="704" height="533" alt="6  Connect to WAN switch" src="https://github.com/user-attachments/assets/cb05cbdf-e60c-43e1-8341-984eaec2d6cc" /> </br>

5. Configure the virtual disk that will be the storage for the VM. This this will have nothing more than pfSense installed, I will give it 20 GB of storage. </br>
<img width="704" height="533" alt="7  Create Virtual Disk" src="https://github.com/user-attachments/assets/3db6e5b8-3d3a-4f88-b800-2a1730a647ab" /> </br>

6. We will have this VM boot from a pfSense .iso so that we can go straight to installing pfSense once we boot up this VM for the first time. </br>
<img width="704" height="533" alt="8  Set to boot into iso" src="https://github.com/user-attachments/assets/912b4a21-44c2-4d20-a5ed-29b2e48ae65c" /> </br>

<img width="1194" height="263" alt="9  VM Created" src="https://github.com/user-attachments/assets/abde49c6-342c-43d3-a8cc-883aa2ea7797" /> </br>

With the VM finally created, we want to modify some of the default settings as well. In our FW01's VM Settings, navigate to the **Processor** tab. By default, the VM is assigned 12 virtual processors. This is wholly unnecessary for this VM's purpose and I will be dropping it down to 1 virtual processor. </br>
<img width="722" height="687" alt="10  Virtual Processor" src="https://github.com/user-attachments/assets/ce65fc44-2566-4a44-8566-79c430fd11f9" /> </br>

For the final change, go to the **Checkpoints** tab, uncheck **User automatic checkpoints**. </br>
<img width="722" height="687" alt="11  Checkpoints Setting" src="https://github.com/user-attachments/assets/efa8d6b6-bba7-4f20-bc9f-f96d7d986188" /> </br>



