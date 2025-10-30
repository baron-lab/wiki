---
layout: default
title: Getting your MRI data
parent: Data Analysis
nav_order: 6
---

# Getting your MRI data

## Automated approaches from Khan lab (recommended)
1. Easiest to do from CBS server (see Servers page), because everything is pre-installed
    - containers for cfmm2tar and tar2bids are located in /srv/containers
2. Use the Khan lab cfmm2tar tool to download the dicoms from the server
    - Best practice: save the call you're using in a clearly named file in your project directory (e.g., "run_cfmm2tar.sh")
    - test with a single date (-d flag) before pulling a whole study
    - use `apptainer run /srv/containers/cfmm2tar_<version>.sif -h` to get help text, where `version` is the version number 
      - you can check the versions that are available by looking in the /srv/containers directory
    - Example call: `apptainer run --bind /cifs/baron/studyData/morrow_ADD/dataTar:/dataTar /srv/containers/cfmm2tar_v1.0.0.sif -p 'Morrow^ADD-MRI' /dataTar`
3. Use the Khan lab "tar2bids" tool to convert the dicoms to NIFTI, organized in the BIDS format
    - Best practice: save the call you're using in a clearly named file in your project directory (e.g., "run_tar2bids.sh")
    - call with the no flags for help information. e.g. `apptainer run /srv/containers/tar2bids_v0.3.0.sif`
    - the rules for naming of files based on the metadata from the scanner can be supplied via a "heuristic" file supplied via the `-h` option
      - the default one created by the Khan lab will work for most standard scans
        - our advanced diffusion scans will just be given generic dwi names that do not reflect things like b-tensor encoding or ogse
      - if you supply a file, it replaces the default, so you'll have to redefine the rules for rest of the scans too
        - the heuristic is saved in the "code" directory that is outputted, so you can run tar2bids with the default heuristic in a sample subject, then modify the heuristic file it creates
          - the metadata is saved in a "dicominfo.tsv" file in the same folder, which you can use to create your own rules


## Manual download of Dicoms 
Go to [https://dicom.cfmm.uwo.ca/dm/](https://dicom.cfmm.uwo.ca/dm/) and follow the directions.
  - you'll need to convert to NIFTI for most tools. You can use "dcm2niix" from the command line to do so. 
    - check help with `dcm2niix -h`

## Raw Data
1. Tech will transfer data to the lab's CBS server allocation. This is mounted on baron1 at /srv/bmisrv_baron
2. The data will be in the folder "cfmm_data" on our CBS data share, which is at /cifs/bmisrv_baron/cfmm_data on most systems
3. Move the data from cfmm_data to the appropriate long term storage location (likely a trainee specific folder or studyDataRaw)