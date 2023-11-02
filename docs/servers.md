---
layout: default
title: Servers
nav_order: 4
---
# Servers

We use shared servers for most data analysis. 

Save all data you do not want to lose to Baron Lab datashare, which is mounted on all below systems. The below descriptions give the mount location for each. Each trainee should have their own folder in the datashare.

1. CBS Server (Ubuntu)

    - Main uses: image preprocessing and analysis (registration etc)
    - Many toolboxes already installed 
    - Access to Baron Lab datashare: use the command `mount /srv/baron` in a terminal every time you log in to get access to the data
    - See [https://osf.io/k89fh/wiki/Computational%20Core%20Server/](https://osf.io/k89fh/wiki/Computational%20Core%20Server/) for
        - setting up account. All lab members should start with a Basic account, and then if they need more resources contact C. Baron about a Power User account.
        - login instructions
    - Troubleshooting forum: [https://cbs-discourse.uwo.ca/](https://cbs-discourse.uwo.ca/)  
        - Look here first if you're having issues. If you can't find a solution, post your own question! 

2. baronsrv.robarts.ca (Windows)
    - Main uses: image recon with MATLAB
    - IMPORTANT: when ending a session do not choose "shutdown". Either log out, or just close the RDP window. 
    - access to Baron Lab data share: P drive
    - log in using Remote Desktop Connection with UWO credentials (use full UWO email as username)
        - Use Remmina from a Ubuntu machine (it is pre-installed)  
            - Click the icon with a page symbol and little plus sign to add profile. Enter "baronsrv.robarts.ca" for server, and your full UWO email for Username. Do not enter a password (if you leave it blank it will prompt you for one - it's more secure to not let it save the password). 
            - You may receive some sort of error that says that the Color Depth Settings are not supported. This may have to be changed to a different color depth for it to work. For example, if the default is GFX AVC444 (32 bpp) and this does not work, try changing it to GFX RFX (32bpp).   
    - Specs:   
        - Intel(R) Core(TM) i9-11900K @ 3.50GHz  
        - 128 GB RAM 
        - Two of: NVIDIA GeForce RTX 4090 (24 GB each) 

3. baron1.robarts.ca (Ubuntu)  
    - Main uses: fsl eddy_cuda, which unfortunately is not on CBS server. Camino and mrtrix are also installed.
    - log in via ssh with UWO credentials
    - Access to Baron Lab datashare: `/srv/bmisrv_baron`. If you don't see the data there, you may have to run `mount /srv/bmisrv_baron`.
    - Specs: 
        - Intel(R) Core(TM) i9-7900X CPU @ 3.30GHz 
        - 128 GB RAM 
        - Two of: NVIDIA GeForce GTX 1080 Ti (12 GB each) 

## Baron lab groups 

These are the groups Robarts IT has set up for the lab, which determines permissions etc across the network. Most trainees don't need to worry about this. 

- RRI-U-Baron: C. Baron and trainees  
    - RRI-U-Baron is a member of *-Data & *-LinuxSys so they get access to those areas by default also. 
- RRI-UD-Baron-Data: have access to Baron lab datashare and Windows servers 
- RRI-U-Baron-SysAdmin: Data administrator and linux admin (just C Baron currently) 
- RRI-U-Baron-LinuxSys: Anyone needing access to linux machines (e.g., baron1.robarts.ca) 
