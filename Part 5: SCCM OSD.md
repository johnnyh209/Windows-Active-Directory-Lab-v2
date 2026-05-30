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
