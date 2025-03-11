---
layout: default
title: MRtrix
parent: Useful Tools
nav_order: 6
---
# MRtrix

MRtrix3 provides a set of tools to perform various types of diffusion MRI analyses, from various forms of tractography through to next-generation group-level analyses.

## Preprocessing

Important things that MRtrix can not do are:
- eddy current correction. Can be done using FSL (eddy)
- distortion reduction. Can be done using FSL (topup)

MRtrix preprocessing:
- dwidenoise - preprocessing to reduce noise 
- mrdegibbs - preprocessing to reduce ringing
All these preprocessing steps can be performed fairly easily on baron1 using the dMRIpreproc.sh bash script that is in the baronAnalysis repository [https://git.cfmm.uwo.ca/baron-lab/baronAnalysis](https://git.cfmm.uwo.ca/baron-lab/baronAnalysis)

topup seems finicky, though, and may require a fair bit of tuning of parameters for your application

## "Bread and butter" functions

Check documentation for more details: [https://mrtrix.readthedocs.io/en/latest/reference/commands_list.html](https://mrtrix.readthedocs.io/en/latest/reference/commands_list.html)
- mrconvert - converts nifti to mrtrix .mif format.
    - Embeds .bvec and .bval files into the data file
    - Can extract specific volumes or slices
- mrcat - concatentate several images/volumes into one file
    - if you have a .mif with .bval and .bvec files embedded, this will automatically concatenate those, too
- dwi2adc - compute ADC only from data. Useful for 3 or 4 dir protocols where that's all you're interested in.
- dwi2tensor - compute DTI or kurtosis models
    - The tensor coefficients are stored in the output image as follows: volumes 0-5: D11, D22, D33, D12, D13, D23 ; If diffusion kurtosis is estimated using the -dkt option, these are stored as follows: volumes 0-2: W1111, W2222, W3333 ; volumes 3-8: W1112, W1113, W1222, W1333, W2223, W2333 ; volumes 9-11: W1122, W1133, W2233 ; volumes 12-14: W1123, W1223, W1233
- tensor2metric - get things like FA, axial diffusivity, color coded FA (-vector option)
    - unfortunately things like mean kurtosis, axial kurtosis, etc are not currently supported in MRtrix!
        - However, we are mostly concerned with the mean kurtosis, which can be estimated using Matlab scripts in the baronAnalysis repo (see below)
        - One can find the formulas for these in [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2997680/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2997680/) (Eqn 55 - 61). AK and RK are pretty simple to compute, but MK seems strangely complicated.

## Integration into MATLAB functions in baronAnalysis repository

- Repository location: [https://git.cfmm.uwo.ca/baron-lab/baronAnalysis](https://git.cfmm.uwo.ca/baron-lab/baronAnalysis)
- dMRIpreproc.sh
    - can input one or two nifti filenames. If two, assumes they are two identical acquisitions with opposite phase encoding directions to use for topup. If only one filename is inputted, no topup is performed
    - Performs, in order, denoising, degibbs, topup, eddy correction
    - also outputs a brain mask used for eddy. 
        - Should confirm it looks correct
- denoiseMult.sh
    - concatentates several nifti's, does denoising, then breaks them apart again
        - denoising is more effective for more volumes included
- nii2uFA
    - computes parameters based on what's present in the scan (e.g., computes uFA if isotropic scans were acquired)
    - can still use even for normal DTI with no uFA-type acquisitions
    - save's nifti's
    - calls mrtrix from matlab for diffusion tensor parameters
        - uses "system()" function in matlab