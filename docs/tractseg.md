---
layout: default
title: TractSeg
parent: Useful Tools
nav_order: 6
---
# TractSeg

TractSeg is a tool for fast and accurate white matter bundle segmentation from Diffusion MRI. It can create bundle segmentations, segmentations of the endregions of bundles and Tract Orientation Maps (TOMs). Moreover, it can do tracking on the TOMs creating bundle-specific tractogram and do Tractometry analysis on those. See: [https://github.com/MIC-DKFZ/TractSeg](https://github.com/MIC-DKFZ/TractSeg)

## Usage on the CBS server
> <pre>singularity exec -e /srv/containers/brainlife_tractseg_2.7.sif TractSeg </pre>

## Usage tips
- before using tractseg, you'll need a nifti. Use dcm2niix to convert dicoms to nifti. This will also output the .bval and .bvec files that tractseg needs.
- you have to run it multiple times to the same output folder, but with different --output_type each time (it reads in the data from the output folder too)

## Segmenting the bundles on a nifti image

> <pre>TractSeg -i Diffusion.nii.gz --raw_diffusion_input </pre>
- bvals and bvecs must be in the same directory as the nifti
- ouputs:
    - bundle_segmentations folder, with all the segmentations listed as .nii.gz files 
    - peaks.nii.gz
    - nodif_brain_mask.nii.gz
- To ensure that segmentations were performed properly, it is good to check that generated brain masks look as expected, and that there is no strange behaviour in the peak maps
 
> <pre>TractSeg -i my/path/my_diffusion_image.nii.gz
>         -o my/output/directory
>         --bvals my/other/path/my.bvals
>         --bvecs yet/another/path/my.bvec
>         --raw_diffusion_input</pre>

- custom input and output if files are in different directories
- Can also supply your own brain masks with the following extension:
> <pre>--brain_mask nameof_brainmask.nii.gz </pre>

## Example:

> <pre>singularity exec -e /srv/containers/brainlife_tractseg_2.7.sif TractSeg -i input.nii.gz \
> -o output --bvals b.bval --bvecs b.bvec --raw_diffusion_input</pre>