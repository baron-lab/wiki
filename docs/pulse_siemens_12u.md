---
layout: default
title: Siemens Programming VE12U
parent: Pulse Programming
nav_order: 9
---
# Siemens Pulse Programming for VE12U (7T system at CFMM)

## Preamble
- This is just a summary to get you started. Lots of info is on the CFMM wiki (Ask Corey for link).
- idea/midea project wiki has helpful information for pulse programming. Need to be added to idea group to access (not necessary unless you'll be modifying code).

## Virtual Box Installation 
- Pulse sequence programming will require virtualbox
- set up so that ubuntu automatically updates current version 
    - For Ubuntu, Debian-based Linux distributions in  https://www.virtualbox.org/wiki/Linux_Downloads)
- to update to next version, use (where XXX is the version number, like 5.2):
> <pre>sudo apt-get update
> sudo apt-get install virtualbox-XXX</pre>

## Siemens 7 T
### Getting Started
- Install Virtual Box (see above for instructions; can also just download )
- In VBox: File->Preferences->General Tab, then change Default Machine Folder to where ever you want your virtual machines stored (imported machines will go there)
- follow directions in https://git.cfmm.uwo.ca/cfmm/info/-/wikis/siemens_idea to get mars_VE12U and VE12U_F50 (or whatever F version is most recent) virtual machines from smb.robarts.ca server.
    - EDIT (June 2022): The virtual machines are located at https://idea.cfmm.uwo.ca/downloads/b/cfmm-vm/VE12U_F50
    - Most work is done in the VE12U_F50 machine. The mars machine is required for compiling a pulse sequence to be put on the scanner
- CFMM User PW: cfmm
### Setup for Pulse Programming
- should get Martyn to add you to the idea group on CFMM gitlab
- Update git name and email:
    - Open git bash from start menu and enter:
    > <pre>git config --global user.name FIRSTNAME LASTNAME
    > git config --global user.email name@uwo.ca</pre>
- Follow instructions in Idea->MIDEA->Wiki->git to set up ssh key (or use HTTPS to clone). The .profile file may already be there if you imported from baron1
- pull baseline code updates from idea > VE12U. Open git bash, then:
> <pre>cd /c/midea
> git clone git@git.cfmm.uwo.ca:idea/ve12u.git baseline
> cp -r baseline/.git N4_VE12U_LATEST_20181126/.git
> rm -rf baseline
> cd N4_VE12U_LATEST_20181126
> git checkout . </pre>
### Specific Sequence Setup
- fpcali (used for calibration of field probes)
    - open git bash
    > <pre>cd /c/midea/N4_VE12U_LATEST_20181126/n4/pkg/MrServers/MrSpecAcq
    > mv fid fid_old
    > git clone git@git.cfmm.uwo.ca:baron-lab/girf_fid_ve12u.git fid
    > cd fid
    > git checkout fpcali </pre>
- ep2d_baron
    - fpcali must already have been setup
    - open git bash
    > <pre>cd /c/midea/N4_VE12U_LATEST_20181126/n4/pkg/MrServers/MrImaging/Seq
    > mv a_ep2d_diff a_ep2d_diff_old
    > git clone git@git.cfmm.uwo.ca:baron-lab/a_ep2d_diff_baron_ve.git a_ep2d_diff</pre>

### Simulating a sequence
- Start IDEA.net from start menu
    - choose the only drive that's there
- use cs command to change sequence
- First time, or when seq name changes, use RegTarg command
    - before changing seq name (should almost never have to do), use UnregTarg. Then after changing sequence name, use RegTarg
- compile debug version of sequence using ms 1
- simulate using poet
### Creating a new custom sequence
- copy the sequence you'll be modifying with a new name (e.g., "cp fid fid_orig")
- use "git init" in existing pulse sequence you will modify
    - add all the files required for compilation (*.c, *.h, makefile.trs)
    - ignore visual studio files (*.dsp, *.sln, and *.vcproj). Can make a .gitignore if desired.
- create new project in cfmm gitlab, in baron-lab group
    - use the "push existing Git repository" instructions that come up to push to master
- create a new branch called "product" that has the initial sequence (can do from cfmm gitlab website). No custom changes to the sequence should go into this branch. It will be used when there are updates to the product sequence.
- register the new sequence on https://idea.cfmm.uwo.ca to get a project number. Project -> Create
    - Use "C" as the symbol.
    - adjust makefile.trs to have the sequence name have _CN, where N is the project number (e.g., _C26)
        - will have to use "unregtarg" and "regtarg" commands in IDEA.Net so that the build uses the new name
- Make all changes in a separate branch. Only push to master when it's been successfully tested.
### Compiling a sequence for transferring to scanner
- First time setup
  - create a new NAT network in virtual box using File > Tools > Network Manager
    - Name it "Siemens" 
    - Make sure this NAT Network is assigned bo both virtual machines
    - open mars VM, login with root (no PW), and type "ip addr" to get the ip address of enmain. e.g., 10.0.2.4
    - open windows VM, open MIDEA, and run "externalmars -ip 10.0.2.4", where the same ip as you observe in the previous step is used
      - if there are still issues connecting to mars VM, see [https://git.cfmm.robarts.ca/idea/MIDEA/-/wikis/mars](https://git.cfmm.robarts.ca/idea/MIDEA/-/wikis/mars) for more tips
    - try compiling with "ms 4". You'll likely get a "key is expired message". If so:
      - in windows VM, in MIDEA, run "marspasswd". Enter a new pw of your choice.
      - see [https://git.cfmm.uwo.ca/idea/MIDEA/-/wikis/tips#expired-passwords](https://git.cfmm.uwo.ca/idea/MIDEA/-/wikis/tips#expired-passwords) for more info.
- make sure mars VM is turned on
- ms 4 to compile .so file only. ms 7 to compile everything.
- when done, use "sudo poweroff" to shutdown mars VM
### Transferring sequence to scanner
- You need the TARGET.dll and TARGET.so files
- can grab the files from C:\Users\cfmm\AppData\Local\Temp, since everything always gets copied there on successful build.
- the files go in c:\medcom\mricustomer\seq\ on the MRI host computer
### Path for where to save direction sets
- C:\MIDEA\N4_VE12U_LATEST_20181126\n4\x86\prod\bin\DiffusionVectorSets
### Enabling Bidirectional Clipboard
To copy text and files between the Virtual Machine and Host OS (Untested for non Windows VMs)
- Ensure that the birectional clipboard option is selected from Devices > Shared Clipboard > Bidirectional
- Install Guest addition. Devices > Insert Guest Additions
- Restarting the Virtual Machine should enable the changes to take effect
    - To check: open Task manager (ctrl+del on VM) and ensure the process VBoxTray.exe is running, if it is not running create a new task and start the application from C:\Program Files\Oracle\VirtualBox Guest Additions
### Troubleshooting issues for communication between VMs for compiling SO
- It seems problems come up very often for this
    - See idea > MIDEA > Wiki > Compiling SO (shared object) library using mars VM
    - Common errors
        - "The user mars does not exist on the local system" -> create a new user through "User Accounts -> Manage Another Account" in Control Panel. Name it "mars"
