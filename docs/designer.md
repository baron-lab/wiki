---
layout: default
title: DESIGNER
parent: Useful Tools
nav_order: 6
---

# DESIGNER
DESIGNER is a pre-processing pipeline for dw-MRI maintained by the NYU diffusion biophysics group. 

It state-of-teh-art preprocessing steps general agreed by the dMRI community. It can also performs 

For more information check their webpage/documentation: https://nyu-diffusionmri.github.io/DESIGNER-v2/


## Using designer in baron servers

We have singularity containers to run DESIGNER v2 in '/cifs/baron/containers'

First, ensure you have singularity on your PATH loading singularity module: 

> module load singularity

In baron10 singularity module is apptainer:

> module load apptainer

The designer container needs binding the folder with the data in the host machine with a mounting point inside the container runs the pipeline. Fot rhis use --bind host_path:container_path to mount your folder (host_path) to a specified path inside the container (container_path). To see thsi in action, check the examples below.

### A typical run example
This example mounts the host_path (/path/to/folder/with/dataset) to the container_path (/mnt). The host_path contains the input series dwi.nii with its bvec (dwi.bvec), bval (dwi.bvec), and json (dwi.json containing PF factor, PE dir, and TE) along with reverse phase-encoding b=0 image (rpe_b0.nii). Its runs the designer contained in baron/containers/designer.sif (usually in cifs). The following is an example that calls designer and will process the data using the recommended designer pipeline:

``` bash
# creating auxiliary variables
path_dataset=/path/to/folder/with/dataset
pa=/mnt/rpe_b0.nii
dwi=/mnt/dwi.nii
dwi_out=/mnt/dwi_designer.nii
designer2_sif=baron/containers/designer.sif

# Singularity designer command
singularity run --nv --bind $path_dataset:/mnt \
$designer2_sif designer \
-denoise -rician \
-degibbs \
-eddy -rpe_pair $pa \
-normalize -mask \
-scratch /mnt/processing -nocleanup \
$dwi $dwi_out
```

Used options quick explanation:
- -- nv (singularity option): Indicate singularity to access the GPU so designer can run eddy_cuda
- --bind (singularity option): Mounts host folder inside the designer container
- -denoise: Perform MP-PCA denoising
- -rician: Rician correction (for magnitude denoising)
- -degibbs: Do degibbs. Note designer can perfomr Partial fourier degibbs
- -eddy: Note: Designer internally uses mrtrix eddy wraper and can uses its inputs. Check documentation.
- -rpe_pair: Indicates path of the PA for eddy
- -normalize: Normalize outputs (Depeing on the application usually not neccesary)
- -mask: Saves the outputs maks afte eddy.
- -scratch: Scratch folder for intermediary steps
- -nocleanup: Don't erase scratch files after preprocessing.

Check the designer [documentation](https://nyu-diffusionmri.github.io/DESIGNER-v2/docs/designer/usage/) to learn more about the options.

This example is based in the singularity example in the DESIGNER2 documentation [examples](https://nyu-diffusionmri.github.io/DESIGNER-v2/docs/designer/examples/). Check it for more examples.

### Advanced example

```bash
input_folder=/path/to/folder/with/dataset
output_folder=/path/to/folder/output/dataset
designer2_sif=baron/containers/designer.sif
dwi1=lte.nii
dwi2=ste.nii
rpe_pair=b0_rpe.nii
pe_dir=AP
echo_times=90
partial_fourier=0.75

singularity run --nv --bind $input_folder:/input,$output_folder:/output $designer_container \
designer \
-denoise -rician \
-degibbs -pf $partial_fourier \
-pre_align -ants_motion_correction \
-eddy -rpe_pair /input/$rpe_pair -rpe_te $echo_times -pe_dir $pe_dir \
-echo_time $echo_times,$echo_times \
-bshape 1,0 \
-eddy_groups 1,2 \
-mask \
-scratch /output/designer_scratch -nocleanup \
/input/$dwi1,/input/$dwi2 \
/output/designer_dwi.nii
```