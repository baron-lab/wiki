---
layout: default
title: Python
nav_order: 9
---
# Pulse Programming

## Preamble
- This is just a summary to get you started. Lots of info is on the CFMM wiki.
    - idea/midea project wiki has helpful information for pulse programming. Need to be added to idea group to access (not necessary unless you'll be modifying code)

## Virtual Box Installation (3T and 7T)
- Pulse sequence programming will require virtualbox
- set up so that ubuntu automatically updates current version 
    - For Ubuntu, Debian-based Linux distributions in  https://www.virtualbox.org/wiki/Linux_Downloads)
- to update to next version, use (where XXX is the version number, like 5.2):
> <pre>sudo apt-get update
> sudo apt-get install virtualbox-XXX</pre>

## VMware Player Installation (Bruker)
- Download latest version of VMware Workstation Player (should be a .bundle file for linux)
- navigate to folder it downloaded to using terminal
> <pre>chmod u+x FILENAME.bundle
> sudo ./FILENAME.bundle</pre>

## Bruker, Paravision360 (9.4T and 15T)
### Misc info
- See  https://git.cfmm.uwo.ca/bruker/documentation/-/wikis/ParaVision-360-Licenses for off-campus tips
### Getting Started
- Install VMware (see above)
- Go to https://idea.cfmm.uwo.ca -> login in with UWO -> click Downloads -> click bruker -> download latest version (*.ova file)
- Open VMware -> "Open a virtual machine" -> select *.ova file downloaded above -> follow prompts
- Open virtual machine. 
- FIRST TIME setup
    - set git config --global user.name "your name here" and git config --global user.email todo@uwo.ca
    - Do ssh-keygen and follow instructions on gitlab to add a key
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
- Your sequence folder should be in /opt/PV6.0.1/prog/curdir/nmrsu/ParaVision/methods/src/
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

## Siemens 3T
TO BE COMPLETED