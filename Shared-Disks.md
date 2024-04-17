# Shared Disk

Windows/MacOS

### What is a Shared Disk ?

A shared disk is a disk that can be accesed by the network.

The disk is usually stored in a NAS, and can be accesed from an other device: TV, laptop, phone, IoT, etc...

## Windows 10

If you want to configure a shared disk on your network using windows 10, [Click Me](https://kb.netgear.com/fr/19864/Comment-mapper-un-lecteur-r%C3%A9seau-sous-Windows?language=fr) *if you understand french*, or do the following steps:

### Step 1

Click the `startup/windows` logo button or the `magnifying glass` located at the **bottom left** of your screen.

### Step 2 

In the **search bar** type `This PC` and **click** on `This PC`.

### Step 3

In the menu located at the top left of the window that you just opened, click on `Computer`, and click on `Map network drive`.

### Step 4

In the **Map Network Drive** window, enter the letter of your choice using the dropdown menu. Example: `Z:`

### Step 5

In the **Folder** section, enter the **drives path**, example: `\\server\share`.
>[!tip]
>In `\\server\path` for `server`, enter the server's **Name**, **IP** or **Domain**.
>
>And to find the `path`, go onto the device where the disk you want to share is, open your file explorer, find the disk you want to share, go into the *path bar* located under the Files, Computer and View buttons, finally copy the disk's path by clicking into the bar and pressing `ctrl` + `A` then `ctrl` + `C`. 

### Step 6

Click the `Finish` button located at the **bottom right hand corner** of the **Map Network Drive** window.

## MacOS

Coming soon...
