---
layout: default
title: Servers
nav_order: 5
---
# Servers

We use shared servers for most data analysis. 

Save all data you do not want to lose to Baron Lab datashare, which is mounted on all below systems. The below descriptions give the mount location for each. Each trainee should have their own folder in the datashare.

1. CBS Server (Ubuntu 22.04)
    - Main uses: Image processing and analysis (registration etc)
    - Many toolboxes already installed. Check the full list using [modules](https://baron-lab.github.io/wiki/docs/modules.html)
    - Baron Lab datashare is mounted at: `/cifs/baron`
      - Other project shares:
        - `/cifs/baron_projects/hyades_mri`
    - See [https://hackmd.io/@CompCore/BJ0JLCnzxg](https://hackmd.io/@CompCore/BJ0JLCnzxg) for:
        - setting up account. All lab members should start with a Basic account, and then if they need more resources contact C. Baron about a Power User account.
        - Login and general log-in instructions
    - Troubleshooting forum: [https://cbs-discourse.uwo.ca/](https://cbs-discourse.uwo.ca/)  
        - Look here first if you're having issues. If you can't find a solution, post your own question! 
        - Technical issues can be addresed to support-cbs-server@uwo.ca

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

3. rri-baron1.robarts.uwo.ca (Baron1. Ubuntu 24.04 LTS)
    - Main uses: General proccesing and eddy_cuda, which unfortunately is not on CBS server. Neuroimaging software available with [modules](https://baron-lab.github.io/wiki/docs/modules.html).
    - log in with UWO credentials via:
      - ssh (e.g. `ssh mymail@uwo.ca@rri-baron1.robarts.uwo.ca`)
      - remote desktop connection (from Windows) or Remina (from Linux)
        - IMPORTANT: when connecting with remote desktop connection, go to "Experience" tab and choose "LAN (10Mbps or higher)" connection speed to drastically improve performance
    - Access to Baron Lab datashare: `/cifs/baron`
    - Specs: 
        - Intel(R) Core(TM) i9-7900X CPU @ 3.30GHz 
        - 128 GB RAM 
        - NVIDIA GeForce GTX 1080 Ti (12 GB) 


4. rri-baron10.robarts.uwo.ca (Baron10. Ubuntu 24.04 LTS)
    - Main uses: Another server option for eddy_cuda. Neuroimaging software available with [modules](https://baron-lab.github.io/wiki/docs/modules.html).
    - Same log in instructions as Baron1.
    - Access to Baron Lab datashare: `/cifs/baron`
    - Specs: 
        - Intel(R) Core(TM) i9-9900K CPU @ 3.60GHz
        - 32 GB RAM 
        - NVIDIA GeForce RTX 2080 Ti (12 GB)

## Baron lab groups 

These are the groups Robarts IT has set up for the lab, which determines permissions etc across the network. Most trainees don't need to worry about this. 

- RRI-U-Baron: C. Baron and trainees  
    - RRI-U-Baron is a member of *-Data & *-LinuxSys so they get access to those areas by default also. 
- RRI-UD-Baron-Data: have access to Baron lab datashare and Windows servers 
- RRI-U-Baron-SysAdmin: Data administrator and linux admin (just C Baron currently) 
- RRI-U-Baron-LinuxSys: Anyone needing access to linux machines (e.g., baron1.robarts.ca)
- RRI-U-Baron-srv-users: access to baronsrv.robarts.ca
- RRI-U-Baron-srv-admins: admins for baronsrv.robarts.ca
