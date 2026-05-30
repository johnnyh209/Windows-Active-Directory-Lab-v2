# SCCM Server Disk Setup
Adding this as a reminder of what my SCCM server VM's disk setup is like: </br>
<img width="756" height="292" alt="image" src="https://github.com/user-attachments/assets/86cfabdf-0a37-4d6b-8f1c-90c81740104f" /> </br>

# Creating the sources$ share

1. In my J:\ (SCCMM_Application_Sources) drive, I created a folder named **sources**. This is where our operating system files will be placed.
2. Next, we need to mturn this **sources** folder into a share. First, right-click on **sources** folder and click on **Properties**.
3. Go to the **Sharing** tab and click on **Advanced Sharing**.
4. Check the **Share this folder** box.
5. Add a **$** sign to the end of the share name. This will hide the folder from casual access.
6. Then click on **Permissions**. For the Everyone group, give it **Full Control**. Then apply all changes made.

# Getting install.wim

1. The Windows 11 Business iso comes with various versions of Windows. For this lab, we want only the Enterprise edition. To do so, first extract your Windows 11 Business Edition iso.
2. Open the **sources** folder and look for the **install.wim** file.
3. Either move or copy + paste install.wim to the **sources$** shared folder.
4. In Configuration Manager, navigate to Software Library > Operating Systems. Right-click on **Operating System Images** and click on **Add Operating System Image**.
5. For the **Path** field, enter in the UNC path to our **sources$** share that points directly to the install.wim file.
6. Then, check the box that says **Extrack a specific image index from the specified WIM file**. Then select the version you want. I selected Windows 11 Enterprise.
7. Select your language, and choose x64 architecture.
8. Type in a name (and version and comment if desired).
9. Import and wait for the process to complete.
10. Back in the **Operating System Images** page on Configuration Manager, you should see the OS you imported now listed.
