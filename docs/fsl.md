---
layout: default
title: FSL
parent: Useful Tools
nav_order: 5
---
# FSL

FSL is a comprehensive library of analysis tools for FMRI, MRI and diffusion brain imaging data.

## Usage on baron1 server

We use a singularity container for FSL because it has very picky installation requirements. To use it, add the following to ~/.bash_aliases (if the file does not exist, create it):

> <pre>alias eddy_cuda="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif eddy_cuda9.1"
> alias fslmaths="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif fslmaths"
> alias fslmerge="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif fslmerge"
> alias bet="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif bet"
> alias fslnvols="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif fslnvols"
> alias fslroi="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif fslroi"
> alias eddy_correct="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif eddy_correct"
> alias topup="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif topup"
> alias applytopup="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif applytopup"
> alias fast="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif fast"
> alias flirt="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif flirt"
> alias first="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif flirt"
> alias run_first_all="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif run_first_all"
> alias applywarp="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif applywarp"
> alias convert_xfm="singularity exec --nv /opt/containers/singularity-fsl_6.0.4.sif convert_xfm" </pre>

The list of aliases is not exhaustive, but should cover the most common ones. If there are other FSL commands you need access to, simply add them to your .bash_aliases using the same format as above.
 
For these aliases to work within bash scripts, you'll need to add the following to the beginning of the script (not to ~/.bash_aliases):

> <pre>shopt -s expand_aliases
> source ~/.bash_aliases </pre>
 
You will also need to update your .bashrc file so that it adds singularity to your path. This only needs to be done once, and won't take effect in already open terminals (you just have to open a new terminal after doing these instructions). Open ~/.bashrc in a text editor, and add the following lines to the bottom:
 
> <pre># Singularity
> export PATH=/opt/singularity/bin:${​​​​​​PATH}​​​​​​
> . /opt/singularity/etc/bash_completion.d/singularity </pre>