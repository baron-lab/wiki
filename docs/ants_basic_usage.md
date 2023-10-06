---
layout: default
title: ANTs Basic Usage
parent: useful_tools
---
# ANTs Basic Usage

ANTs (Advanced Normalization Tools) is a software package that can be used to perform image registration with variable transformations (elastic, diffeomorphic, diffeomorphisms, unbiased) and similarity metrics (landmarks, cross-correlation, mutual information, etc). The source code can be found at: [https://github.com/ANTsX/ANTs](https://github.com/ANTsX/ANTs)

## Installation

Installation instructions can be found here: [https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS](https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS)

The installation takes a while (could be a few hours).

Post installation, the environment variables PATH and ANTSPATH have to be set. On linux, this can be done by adding the following lines to ~/.bash_profile:

> export ANTSPATH=/opt/ANTs/bin/ 
>
> export PATH=${​​​ANTSPATH}​​​​​​​​​​:$PATH

To update the changes type this command in terminal:
> source ~/.bash_profile

## Registering an Image



> antsRegistration -d 3 \                                       #3D image <br>
> -r [anatomical.nii,meanb0.nii,1] \
> -m MI[anatomical.nii,meanb0.nii,1,32] \                       #similarity metric: mutual information​ <br>
> -t Affine[0.1] \                                              #Transform 1: Affine transform, step size = 0.1​ <br>
> -c 10000x10000x10000x10000x10000 \                            #convergence factors <br>
> -s 0.8x0.6x0.4x0.2x0mm \                                      #smoothing sigmas <br>
> -f 5x4x3x2x1 \                                                #shrink factors <br>
> -l 1 \
> -r [anatomical.nii,meanb0.nii,1] \
> -m MI[anatomical.nii,meanb0.nii,1,32] \
> -t SyN[0.1] \                                                 #Transform 2: Symmetric diffeomorphic registration​ <br>
> -c 50x35x15 \
> -f 3x2x1 \
> -s 0.4x0.2x0mm \
> -n BSpline \                                                  #3rd order B-spline interpolation​ <br>
> -u true \
> -l 1 \
> -o [transform,b0_warped.nii.gz,b0_inversewarped.nii.gz]       #output: "transform" produces 3 registration transform files:  transform0GenericAffine.mat, transform1Warp.nii.gz, transform1InverseWarp.nii.gz
                                                                #the registered images are: b0_warped.nii.gz and b0_inversewarped.nii.gz (which is the anatomical registered to the b0)