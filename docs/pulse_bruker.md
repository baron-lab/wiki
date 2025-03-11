---
layout: default
title: Bruker
parent: Pulse Programming
nav_order: 9
---
# Bruker Pulse Programming

## VMware Player Installation
- Download latest version of VMware Workstation Pro (now free for non-commercial use) 

## Programming
### Misc info
- You'll need to be added to the idea/midea project users to download the virtual machine (Ask Corey).
### Getting Started
- Install VMware (see above)
- Go to https://idea.cfmm.uwo.ca -> login in with UWO -> click Downloads -> click bruker -> download latest version (*.ova file)
- Open VMware -> "Open a virtual machine" -> select *.ova file downloaded above -> follow prompts
- Open virtual machine. 
- FIRST TIME setup
    - Set `git config --global user.name "your name here"` and `git config --global user.email yourmail@uwo.ca`
    - Do `ssh-keygen` and follow instructions on [gitlab](https://git.cfmm.uwo.ca/help/user/ssh) to add a key.
    - Create a shared folder, which makes it easy to transfer files between host PC and VM
        - Open "virtual machine settings" after VM has started
        - Selection Options tab -> select Shared Folders
        - Click "Enabled until next power off or suspend" (Note: I haven't had luck with "Always Enabled")
        - Click Add... and follow the prompts
        - The share will be mounted in /mnt/hgfs/SHARENAME
- start machine (PW = paravision)
- open paravision from the desktop (on the virtual machine)
- First time: need to create a coil configuration
    - NOTE: may not be needed for pv 360
        - Window->configuration->select coil tab->second click on "Coil Configurations" folder -> "Create default coil configurations"
        - second click on "generic transciever 1H" -> save
- Starting a study
    - open dataset browser window if not open already
    - "Add study..."
        - may need to select an rf coil config you made earlier
        - "Create"
    - Double click on study (can open one from another time too)
    - In Palette, Explorer tab
        - In Application menu, select user methods
        - Drag into white box
        - Change parameters etc
        - To simulate, click on 3rd box from left above list of scans
            - click on GOP simulation tab
            - click start
            - change to "Bruker Topspin" window (bottom bar in the operating system)
- Never suspend a virtual machine while Paravision is running
### Sequence location
- Your sequence folder should be in /opt/PV360.3.5/prog/curdir/nmrsu/ParaVision/methods/src/  (Folder name matches the PV version)
### Compiling your sequence
- You should see your pulse sequence folder in Workspace Explorer tab -> Method Development -> User Methods
- 2nd click, and choose Build/Install
### Exporting your modified sequence
- In Workspace Explorer, 2nd click on pulse sequence folder and select Share -> As Source
- It will get exported to /opt/PV<VERSION>/share (replace <VERSION> with whatever paravision version you're running)
### Tips
- Sometimes, after a successful compile, the sequence does not show up in the Palette explorer. Closing and reopening Paravision has fixed this for me.
- need  #DEFINE DEBUG 1 in your .c file for printf and db_msg commands to show up on the parx server tab in paravision
- The file explorer in the VM OS does not seem to update files reliably. If the files are not showing up, click Control -> refresh.

