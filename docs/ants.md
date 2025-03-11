---
layout: default
title: ANTs
parent: Useful Tools
nav_order: 6
---
# ANTs

ANTs (Advanced Normalization Tools) is a software package that can be used to perform image registration with variable transformations (elastic, diffeomorphic, diffeomorphisms, unbiased) and similarity metrics (landmarks, cross-correlation, mutual information, etc). The source code can be found at: [https://github.com/ANTsX/ANTs](https://github.com/ANTsX/ANTs)

## Installation

Installation instructions can be found here: [https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS](https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS)

The installation takes a while (could be a few hours).

Post installation, the environment variables PATH and ANTSPATH have to be set. On linux, this can be done by adding the following lines to ~/.bash_profile:

> <pre>export ANTSPATH=/opt/ANTs/bin/ 
> export PATH=${​​​ANTSPATH}​​​​​​​​​​:$PATH </pre>

To update the changes type this command in terminal:
> <pre>source ~/.bash_profile </pre>

## Registering an Image

> <pre>antsRegistration -d 3 \                                       #3D image
> -r [anatomical.nii,meanb0.nii,1] \
> -m MI[anatomical.nii,meanb0.nii,1,32] \                       #similarity metric: mutual information​ 
> -t Affine[0.1] \                                              #Transform 1: Affine transform, step size = 0.1​ 
> -c 10000x10000x10000x10000x10000 \                            #convergence factors 
> -s 0.8x0.6x0.4x0.2x0mm \                                      #smoothing sigmas 
> -f 5x4x3x2x1 \                                                #shrink factors 
> -l 1 \
> -r [anatomical.nii,meanb0.nii,1] \
> -m MI[anatomical.nii,meanb0.nii,1,32] \
> -t SyN[0.1] \                                                 #Transform 2: Symmetric diffeomorphic registration​ 
> -c 50x35x15 \
> -f 3x2x1 \
> -s 0.4x0.2x0mm \
> -n BSpline \                                                  #3rd order B-spline interpolation​ 
> -u true \
> -l 1 \
> -o [transform,b0_warped.nii.gz,b0_inversewarped.nii.gz]       #output: "transform" produces 3 registration transform files: transform0GenericAffine.mat, transform1Warp.nii.gz, transform1InverseWarp.nii.gz
>
> #the registered images are: b0_warped.nii.gz and b0_inversewarped.nii.gz (which is the anatomical registered to the b0) </pre>

- Smoothing sigmas (-s option) may be scaled according to the DWI resolution (for the code above the resolution was 0.2x0.2x0.5 mm).
- The resolution of the output registered images will be the same as anatomical image.

## Applying Registration Transforms

This is an example to register DWI data (4D image) to the anatomical data by applying the registration transforms acquired with antsRegistration:

> <pre>antsApplyTransforms -d 3 \
> -e 3 \                                     #specifies input image type: the default is scalar (0); the options are 0/1/2/3/4 which corresponds to scalar/vector/tensor/time-series/multichannel
> -i DWI.nii.gz \                            #input image
> -r meanb0.nii \                            #reference image: the output image will have the same resolution as the reference
> -o DWI_reg.nii.gz \                        #output image: registered DWI
> -n BSpline \
> -t [transform1Warp.nii.gz] \               #transforms to be applied
> -t [transform0GenericAffine.mat,0] </pre>

- ANTs applies transforms from the end to the beginning of the command line, so in this case, the Affine transform will be applied first
- To deform the anatomical to the mean b0 space, the inverse warp nifti file is used and the affine transform command is changed to:
    - -t [transform0GenericAffine.mat,1]
- Identity Transform: If the -t option is omitted in antsApplyTransforms, the "identity transform" is applied, which can be useful if only the resolution is being changed to match the resolution of the "reference image."
- Similarity metrics (such as MI, CC, etc.) of the registered image can be computed via the MeasureImageSimilarity command:

> <pre>MeasureImageSimilarity -d 3 -m MI[anatomical.nii,DWI_reg.nii.gz,1,32] </pre>