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

# Adding Operating System to Configuration Manager

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
11. Right-Click on the listed operating system and click on **Distribute Content**.
12. In **Review selected content** page, click Next.
13. In **Specify the content destination**, click on the **Add** button then click on **Distribution Point**.
14. Select your distribution point and click **OK**.
15. With the distribution point selectd, press **Next**.
16. Review the summary and complete the task.
17. Wait until the Content Status turns green, indicating that the task has succeeded/completed.

# Applications

I want my Windows image to be deployed with VLC Media. So let's first create a package for VLC.

1. On the left pane, under Software Library > Application Management, right-click on **Packages** and click on **Create Package**.
2. In **Specify information about this package**, give it a name, description, manufacturer, language, and version. Then check the box for **This package contains source files** and add the UNC path to the source file of your program.
3. In **Choose the program type that you want to create**, select **Do not create a program**.
4. Review summary and create package.
5. Right-click on the package that you just created and click on **Distribute Content**.
6. In **Review selected content** page, click Next.
7. In **Specify the content destination**, add your distribution point.
8. Review and complete the task.
9. Wait until the Content Status turns green, indicating that the task has succeeded/completed.
10. Next, right-click on the package and click **Create Program**.
11. In **Choose the program type that you want to create**, select **Standard program**.
12. In **Specify information about this standard program**, give it a name and command line to run the installer. Make any other changes that you want.
13. In **Specify the requirements for this standard program**, select **This program can run on any platform**, and give set an estimated disk space. For VLC, somewhere around 100-200 MB should be sufficient, but I gave it 300 MB to be on the safer side. For maximum runtime, I lowered it from 120 minutes to 30 minutes.
14. Review the summary and wait for task to complete.

# Create Task Sequences

1. On the left pane, under Software Library > Operating Systems, right-click on **Task Sequences** and click on **Create Task Sequence**.
2. Select **Install an existing image package** and click **Next**.
3. Give your task sequence a name (and description if desired).
4. Click on **Browse** button.
5. Select your boot image.
6. In **Install the Windows operating system** page, click on the **Browse** button.
7. Select your operating system and click **OK**.
8. I left **Partition and format the target computer before installing the operating system** checked. For the purposes of this lab, I unchecked **Configure task sequence for use with BitLocker**. Enter a product key if you have one. Then I selected **Enable the account and specify the local administrator password**.
9. In **Configure the network** page, select **Join a domain**. Then select your domain, and the OU that the imaged client computer will be placed in in your Active Directory. Also provide the account that has the necessary privileges to join clients to the domain.
10. In **Intall the Configuration Manager client**, I left as default.
11. In **Configure state migration**, I unchecked everything.
12. In **Include software updates**, and for the purposes of this lab, I left on **Do not install any software updates**.
13. In **Install Applications**, we will leave blank for now.
14. Review summary and create task sequence.
15. You should now see your task sequence listed. Right-click your task sequence, and click on **Edit**.
16. In the left pane, under **Setup Operating System**, click on **Setup Windows and Configuration Manager** to highlight it. Then, on the top of the left pane, click on Add > Software > Install Package.
17. This will add a new sequence. Give the sequence a name, then select a package to be installed during the imaging process. Then click Apply, and OK.


