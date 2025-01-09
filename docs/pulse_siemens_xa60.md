---
layout: default
title: Pulse Programming Old
parent: Pulse Programming
nav_order: 9
---
# Pulse Programming for Siemens XA60 (CFMM 3T)

## Preamble
- This is just a summary to get you started. Lots of info is on the CFMM wiki (Ask Corey for link).
- idea/midea project wiki has helpful information for pulse programming. Need to be added to idea group to access (not necessary unless you'll be modifying code).

## VMware Player Installation 
- Download latest version of VMware Workstation Pro (now free from non-commercial use)

## Siemens 3T
### Getting Started
- log into Siemens C2P exchange (https://webclient.ca.api.teamplay.siemens-healthineers.com) and get virtual machine from Views->IDEA Virtual Machine, or ask Corey for it.
Install Virtual Box (see above for instructions; can also just download )
- Import the virtual machine into VMware
- Set up for code signing. See https://git.cfmm.uwo.ca/idea/MIDEA/-/wikis/XA60_Code_Signing 

### Setup for Pulse Programming
- should get Martyn to add you to the idea group on CFMM gitlab
- Update git name and email in VM:
    - Open git bash from start menu and enter:
> <pre>git config --global user.name FIRSTNAME LASTNAME
> git config --global user.email name@uwo.ca</pre> 
- Follow instructions in Idea->MIDEA->Wiki->git to set up ssh key.  In short:
    - run "ssh-keygen" in git bash, from within home directory
    - press enter to use the default filename
    - enter a password that you'll remember
    - copy the contents of id_rsa.pub to ssh keys section on cfmm gitlab site
- get baseline code from Corey, which you'll copy into code repo.

### Specific Sequence Setup
- fpcali (used for calibration of field probes)
    - open git bash
    > <pre>cd /c/midea/NXVA60A_<buildnumber>/src/MrSpecAcq
    > git clone git@git.cfmm.uwo.ca:baron-lab/fpcali_xa60.git fpcali</pre> 
- ep2d_baron
    - fpcali must already have been setup
    - open git bash
    > <pre>cd /c/midea/NXVA60A_<buildnumber>/src/MrImaging/seq
    > mv ep2d_diff ep2d_diff_old
    > git clone git@git.cfmm.uwo.ca:baron-lab/cb_ep2d_diff_xa60.git ep2d_diff
    > cd ep2d_diff
    > git checkout <branchName> </pre>


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
- Make all changes in a separate branch. Only push to master when it's been successfully tested.

### Compiling a sequence for transferring to scanner
- ms 4 to compile .so file only. ms 7 to compile everything.
- when done, use "sudo poweroff" to shutdown mars VM

### Transferring sequence to scanner
- You need to install you developer certificate to the scanner first (one time only). See https://git.cfmm.uwo.ca/idea/MIDEA/-/wikis/XA60_Code_Signing
- You need the TARGET.dll and TARGET.so files
- can grab the files from C:\Users\IDEA\AppData\Local\Temp
- send the files to the MR tech (onedrive link) to copy onto the scanner

### Path for where to save direction sets
- C:\MIDEA\NXVA60A_<buildnumber>\ProgramData\MriCustomer\CustomerSeq\DiffusionVectorSets
  - note that you may need to create the DiffusionVectorSets folder

### Enabling debug output (from email from Omer - not tested)
Do the following to enable DEBUG level of UTrace messages for the sequence dll. 
- typing 'UTraceConfigUI' in the IDEA command window. In the opened window, add a new category by right-clicking MrImaging in the tree and select "Add a New Category". 
- in the new entry that appears, replace "<change_name>" with the sequence name (e.g., "cb_ep2d_diff_C64")
- click on the checkbox in the Markers area for 0x00000001
- make sure both "process parent" and "use all parent appenders" are clicked on
- set trace-level dropdown to "debug"
- After saving the changes, debug messages will start to appear. 
- To revert back, just select the "Notice" level from "Trace-level" dropdown menu and save again. 
- The changes become effective immediately after saving even before closing UTraceConfigUI window, i.e. no need to restart the IDEA window or re-compile the sequence, etc (it might be required to re-open POET window if it opened).
