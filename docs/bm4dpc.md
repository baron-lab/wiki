---
layout: default
title: BM4D-PC
parent: Application Tips
nav_order: 6
---
# BM4D-PC

Baronlab alumni Vinicius Campos has developed BM4D-PC, which is a very effective denoising algorithm for diffusion MRI.

Preprint: [arXiv link](https://arxiv.org/abs/2512.17138) 
Journal: under review at MRM

## Setup
1. C. Baron has made a fork that has an optimized phase alignment that reduces noise floor more than the original version (complex input only). Download the cbaron branch here: [https://github.com/coreybaron/BM4D-PC/tree/cbaron](https://github.com/coreybaron/BM4D-PC/tree/cbaron)

2. Follow the directions in the readme to get the supporting BM4D files

## Usage
In Matlab: 
> I_denoised = denoise_BM4D_PC('/path/to/filename_magnitude.nii.gz','/path/to/filename_phase.nii.gz');

Note that a nii file is automatically saved with the prefix 'BM4D_PC_'
