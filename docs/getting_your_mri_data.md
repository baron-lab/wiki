---
layout: default
title: Getting your MRI data
nav_order: 7
---

# Getting your MRI data

## Manual download of Dicoms
Go to [https://dicom.cfmm.uwo.ca/dm/](https://dicom.cfmm.uwo.ca/dm/) and follow the directions.


## Automatic shell script to download Dicoms
NOTE: Needs to be updated- use manual download for now. 
1. Login into server (e.g., baron1) if data will live there
2. Download fetchData.sh and cfmm_baron.py from Lab-Info repo, and put in same directory (e.g., ~/bin) somewhere in your home directory
3. cd ~/bin
4. Run fetchData.sh script, with 3 input arguments: Date (YYYMMDD), Study folder name, Description
- e.g. 
> <pre>./fetchData.sh 20180509 OGSE_testing 'first tests of ep2d_baron to confirm diff waveforms and develop pipelines'</pre>

The above will download the dicoms to baron1.robarts.ca:/mnt/baron1_local1/data/OGSE_testing and convert them to NIFTI organized in the BIDS format.

1. It is STRONGLY ENCOURAGED to add a list of scans with descriptions to the description .txt file that will be created automatically
2. To just view the NIFTI's, you can mount the data directory on your local system (see above Servers section)
3. For computationally intensive operations, ssh to baron1 to perform them.

## Raw Data
1. Tech will transfer data to the lab's CBS server allocation. This is mounted on baron1 at /srv/bmisrv_baron
2. The data will be in the folder "cfmm_data" on our CBS data share, which is at /srv/bmisrv_baron/cfmm_data on baron1
3. Move the data from cfmm_data to the appropriate long term storage location (likely a trainee specific folder or studyDataRaw)