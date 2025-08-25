---
layout: default
title: DESIGNER
parent: Useful Tools
nav_order: 6
---

# DESIGNER
DESIGNER is a pre-processing pipeline for dMRI maintained by the NYU diffusion biophysics group. For more information check their webpage/documentation: https://nyu-diffusionmri.github.io/DESIGNER-v2/


## Using designer in baron servers

We have singularity/apptainer containers to run DESIGNER v2 in '/cifs/baron/containers'

The designer container needs to bind the folder with the data in the host machine to a directory where the container runs the pipeline. Fot this use --bind host_path:container_path to mount your folder (host_path) to a specified path in the container (container_path). Check the next section for examples

### Basic example
This example binds the host_path (/path/to/folder/with/dataset) to the container_path (/mnt). The host_path contains the input series dwi.nii with its bvec (dwi.bvec), bval (dwi.bvec), and json (dwi.json containing PF factor, PE dir, and TE) along with reverse phase-encoding b=0 image (rpe_b0.nii). The designer containers are in baron/containers (usually in cifs). The following example calls designer and will process the data using the recommended designer pipeline:

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

Quick explanation of the used options:
- -- nv (singularity option): Indicate singularity to access the GPU so designer can run eddy_cuda.
- --bind (singularity option): Mounts host folder inside the designer container.
- -denoise: Perform MP-PCA denoising.
- -rician: Rician correction (for magnitude denoising).
- -degibbs: Do degibbs. Note designer can perfomr Partial fourier degibbs.
- -eddy: Note: Designer internally uses mrtrix eddy wraper and can uses its inputs. Check documentation.
- -rpe_pair: Indicates path of the PA for eddy.
- -normalize: Normalize outputs (Depeing on the application usually not neccesary).
- -mask: Saves the outputs maks afte eddy.
- -scratch: Scratch folder for intermediary steps
- -nocleanup: Don't erase scratch files after preprocessing.

Check the designer [documentation](https://nyu-diffusionmri.github.io/DESIGNER-v2/docs/designer/usage/) to learn more about the options.

This example is based in the singularity example in the DESIGNER2 documentation [examples](https://nyu-diffusionmri.github.io/DESIGNER-v2/docs/designer/examples/). Check it for more examples.

### Advanced example

The next example runs DESIGNER in a dataset with LTE and STE acquisitions. Note that DESIGNER requires separated volumes for this (lte.nii and ste.nii here) and the bshapes option. The new -eddy_group option that make eddy run separately in both volumes.

For this example we also provide the echo times and the partial fourier treshold as explicit parameters instead of letting DESIGNER to read them from the json files. 

Additionally, this example shows how to output diles to a different location in case you required the outputs in other folder.

```bash
input_folder=/path/to/folder/with/dataset
output_folder=/path/to/folder/with/output
designer2_sif=baron/containers/designer.sif
dwi1=lte.nii
dwi2=ste.nii
bshape1=1
bshape2=0
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
-bshape $bshape1,$bshape2 \
-eddy_groups 1,2 \
-mask \
-scratch /output/designer_scratch -nocleanup \
/input/$dwi1,/input/$dwi2 \
/output/designer_dwi.nii
```