---
layout: default
title: Windows Subsystem for Linux
parent: Useful Tools
nav_order: 6
---
# Windows Subsystem for Linux 

Developers can access the power of both Windows and Linux at the same time on a Windows machine. The Windows Subsystem for Linux (WSL) lets developers install a Linux distribution (such as Ubuntu, OpenSUSE, Kali, Debian, Arch Linux, etc) and use Linux applications, utilities, and Bash command-line tools directly on Windows, unmodified, without the overhead of a traditional virtual machine or dualboot setup.

## Warning: Not for casual users

The WSL may be useful for some people, but for most users the CBS remote login will be easier.

## Enabling the Windows Subsystem for Linux

The WSL is a tool provided by Microsoft to allow developers to run a Linux environment directly on Windows without the need for a virtual machine. Before enabling the WSL, it is recommended that you update your OS to the most recent version of Windows. 

To enable the WSL in Windows, open Powershell as an Administrator and run:
>dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
 
Alternatively, in the search bar, search for **Turn Windows features on or off** to open up a GUI that allows you to manually enable the WSL by checking a box.

## Installing Ubuntu (or another Linux distribution)

After enabling the WSL, you can install Linux on Windows. Open the Windows store and search for your preferred distribution. The first time you run a new Linux distribution, a console window will open and you'll be asked to wait a few minutes for installation and setup. All future launches should take less than a second.
 
You will then need to set a user account and password for your new Linux distribution. 
 
To troubleshoot any errors during installation, consult the documentation:
[https://learn.microsoft.com/en-us/windows/wsl/install](https://learn.microsoft.com/en-us/windows/wsl/install)
 
Once Linux is installed and properly set up, you can install ssh on it and use it to acces baron1 and other servers remotely.
