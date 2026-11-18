---
layout: default
title: Preprocessing
parent: Data Analysis
nav_order: 6
---

# Preprocessing 

## Bruker
- basic conversion of enhanced dicom to nifti should be done with bruk_dcm_prep: [https://github.com/baron-lab/bruk_dcm_prep](https://github.com/baron-lab/bruk_dcm_prep)
- this code does the following:
  - extracts lots of information about the scan and saves these as supporting files. This includes:
    - a <basename>_method.json file that contains ALL the parameters used
    - bval, bvec, bmat for diffusion MRI, and all diffusion waveform shapes, their frequency centroids, and b-tensor rank
  - combines coils if separate in complex data
  - combines double sampling using complex combination instead of the default magnitude combination the scanner uses
  - corrects for image shifts from frequency drift (if applicable)
- initial setup and usage:
  - follow instructions in readme in bruk_dcm_prep repository

## Diffusion MRI
TODO