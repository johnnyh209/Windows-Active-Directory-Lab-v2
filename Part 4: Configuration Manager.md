# Disk setup for Configuration Manager

1. In my member server VM, named Server2025, I created 7 virtual disks
2. In our member server VM, open Disk Management, right-click on each disk we created and bring them Online
3. After bringing the disks online, you should now see that they say "Not Initialized." Right-click one of the disks, and then click on **Initialize Disk**. This opens an Initialize Disk window, allowing you to select which disks to initialize, and which partition style to use.
4. For each disk, you will now need to create a Volume / format the disk with a file system.
5. After creating a Volume for each disk, I rename them to help easily know what each disk is for. 
