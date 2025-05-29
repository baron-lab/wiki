---
layout: default
title: Essential Info
nav_order: 2
---

# New trainees
Look at the Onboarding wiki page

# Group Meetings 

Weekly in Faraday Cage Conference Room. Ask CB to add you to the calendar invite if you're not on it.
- will have round-table updates ~ once per month
- will go to grad club ~ once per month
- cancellations will be communicated through slack 

# Website 
[https://www.baronlab.ca/](https://www.baronlab.ca/)

# Printing
The printer located beside the trainee seating area is at ditto.imaging.robarts.ca. Installation:
- download drivers for model "e-STUDIO4505AC"
- you may have to let the installer access the network so that it can find the printer

# Slack
Ask C. Baron to be added to Superlab slack if not yet a member

Turn on notifications for #baronlab and #general

# Servers and data management
We do most data analysis by remotely logging into servers (see Servers page). All data should be saved onto C. Baron's datashare, which is backed up regularly. This datashare is mounted on all of our servers.

# Code Repositories & Git
We use git for version control. There are many good tutorials online that new lab members should use to familiarize themselves with git. There are two main locations for git repositories:
- Public repositories
  - https://gitlab.com/cfmm/matlab/matmri
    - this is our main public facing repository, which is mostly image reconstruction code and some dMRI fitting code
- Private repositories
  - for work-in-progress code or code that cannot be public (e.g., vendor code that we have research agreements to have access to).
  - those on the CFMM local gitlab, https://git.cfmm.robarts.ca/baron-lab
    - we have recently switched to using snakemake pipelines for data processing. These should also be version controlled, and stored in https://git.cfmm.robarts.ca/baron-lab/pipelines
    - baron-matlab-recon: Mainly for non-Cartesian image reconstructions. Basically a wrapper for matmri that covers interfacing with Siemens raw data format and prepping data for main iterative recon.
    - baronAnalysis: lots of depricated stuff, but enhDic2Nii.sh and niiCombDiffAve.m are still heavily used for preprocessing small animal data collected on Bruker systems.
    - baron-seqDesign-matlab: mostly code for generating waveforms for our custom Bruker sequence
    - cb_ep2d_diff_xa60 and fpcali_XA60: custom pulse sequence on Siemens 3T
    - a_ep2d_diff_baron_ve and girf_fid_ve12u: custom pulse sequence on Siemens 7T
  - custom Bruker pulse sequence: will be moving soon to enable collaborators external to Western, but currently in https://git.cfmm.robarts.ca/baron-lab

# Mental Health
Your mental health is important! You are encouraged to take the time off you are entitled to, which can sometimes help. Please let Dr. Baron know if you are encountering challenges, and you can work together to adjust projects or he can point you in the right direction for additional help and/or resources. Western provides health and wellness services at [https://www.uwo.ca/health//index.html](https://www.uwo.ca/health//index.html).

# Reimbursements 
Khan lab has a great wiki page for this (replace references to Dr. Khan with Dr. Baron!): [https://www.khanlab.ca/wiki/expense-claims](https://www.khanlab.ca/wiki/expense-claims) 

# Projects for MRI Bookings 

See [https://dicom.cfmm.uwo.ca/dm/project/](https://dicom.cfmm.uwo.ca/dm/project/) for full list 

- uStruct-TLE: any 3T or 7T scan related to CIHR project in Epilepsy patients. Any spiral scans. Most uFA scans. 

- uStruct-TLE-Histology: 9.4 T scanning related to CIHR project in Epilepsy patients. 

- Diffusion: general development on human scanners (should only need to be used rarely - when in doubt ask Corey)

# Resources for writing papers 

[https://doi.org/10.1371/journal.pcbi.1005619](https://doi.org/10.1371/journal.pcbi.1005619) 


