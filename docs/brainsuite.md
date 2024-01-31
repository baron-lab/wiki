---
layout: default
title: Brainsuite
parent: Useful Tools
nav_order: 6
---
# BrainSuite

BrainSuite is a collection of open-source software tools that enable largely automated processing of magnetic resonance images (MRI) of the human brain. The major functionality of these tools is to extract and parameterize the inner and outer surfaces of the cerebral cortex, to segment and label gray and white matter structures, to analyze diffusion imaging data, and to process functional MRI. 

## Generating brain masks

BrainSuite is useful for generating masks, especially if manual edits need to made to the masks.

BrainSuite is already installed on the CBS server and can be opened with these commands:
> <pre>cd /srv/software
> ./BrainSuite19b/bin/BrainSuite19b </pre>

1. Open the volume on which the mask will be drawn.

2. To automatically generate a mask: Cortex>Skull Stripping (BSE)>Step. You can automate the parameter selection and check 'trim spinal cord' and 'dilate final mask'.

3. To manually edit the mask that is generated: Tools>Mask Tool. Check 'edit mask' and 'paint on all mouse clicks'. You can right click to add to the mask and left click to erase part of the mask.

4. To save the mask: File>Save Volume>Mask Volume.