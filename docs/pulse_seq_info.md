---
layout: default
title: Pulse Sequence Info
parent: Pulse Programming
nav_order: 8
---
# Pulse Sequence Info

## 3T VE11C
### Git repo: baron-lab/ep2d_baron_ve11c
Branches:
1. master: product version of ep2d_diff. 
2. ogse: ogse and isotropic diffusion modes. Five columns diffusion table. Fourth column – diffusion mode: 0 = ogse; 1 = iso; 2 (or > 1) = pgse. Fifth column – ogse frequency. Tips: when loading/changing to a new “number of directions” in a .dvs file in free mode, it is better to reload the sequence from a fresh one without any preset diffusion. Spiral readout is not yet implemented in this VE11C version of diffusion sequence.

The diffusion direction text file is in C:\MIDEA\N4_VE11C_LATEST_20160120\n4\x86\prod\bin\DiffusionVectorSets. In VE, there is a gui to pick the file after you choose "free" modes, so in theory you could have as many different text files as you like and leave them in any location. In the text file. the fourth column is Type: 0 == OGSE, 1 == ISO (not implemented yet), any other == Siemens' PGSE. The fifth column is the OGSE frequency. See [directions=11] as the example.

## 7T VE12U
### Git repo: baron-lab/girf_fid_ve12u
Branches:

1. master: product version of fid which should sit in MrServer/MrSpecAcq/ in IDEA.
2. girf: GIRF measurement using gradient blips with Duyn method. Trigger to field probes can be switched on, so that GIRF can also be measured with field probes.
3. fpcali: Field probes calibration sequence as described in the Skope document. *In girf and fpcali branches, there is a SBB called “SBBFPTrig”. Todo: the trigger channel in the SBB should be changed to match the one chosen during the field probe installation.

### Git repo: baron-lab/a_ep2d_diff_baron_ve
Branches:

1. master: product version of a_ep2d_diff. SMS worked in simulation but still not tested on scanner.
2. ogseiso: Should work the same as VE11C’s baron-lab/ep2d_baron_ve11c/ogse
3. ogseiso_sp: A spiral version of ogseiso. Git diff between this and the ogseiso branch should show the changes required to change baron-lab/ep2d_baron_ve11c/ogse into a spiral sequence. Field probe synchronisation and triggering should be available in ogseiso_sp branch. Note: It depends on SBBFPTrig in baron-lab/girf_fid_ve12u. They need to be recompiled if the trigger channel is changed. EmptyICE.ipr does not work with the spiral sequence. One needs to use IDEACmdTools to switch on EmptyICE manually on the scanner. In long term, this problem could be resolved by using CFMM’s gadgetron ICE.

### Git repo: baron-lab/a_ep2d_bold_spiral_ve
Branches:

1. master: product version of a_ep2d_bold. SMS worked in simulation but still not tested on scanner.
2. spiral: A spiral version of ep2d_bold. Git diff between this and master would show how to add spiral to ep2d_bold in VE11C. Field probe synchronisation and triggering should be available in the spiral branch. The spiral branch depends on SBBFPTrig in baron-lab/girf_fid_ve12u. It should be recompiled if the trigger channel is changed. EmptyICE.ipr does not work with the spiral sequence. One needs to use IDEACmdTools to switch on EmptyICE manually on the scanner. In long term, this problem could be resolved by using CFMM’s gadgetron ICE.

### Enabling Mosaic

To conserve space on the dicom server, it is more efficient to use the Siemens mosaic format to store the images (this should not effect the raw data, only the dicoms). 
 
In order to enable the mosaic format, the number of diffusion weightings (located on the Diff tab in the console) must be set to 2 or more. There are some options here as to how you choose to incorporate this change into your protocol, the simplest ones are outlined below:
 
1. You can simply use the UI to add b0 images (easiest option). This means that your diffusion table will not contain any b0 lines. Instead, to acquire the b0 image(s) you can change the b-value for one of the two diffusion weightings to 0. This will add one b0 acquisition to your protocol, should you choose to have more than 1 b0 acquisition you can manually adjust the number of averages for each shell on the Diff tab. In this case the other b-value would be entered normally as your selected target b-value.
2. Use the UI to implement multiple shells. Note: this method is not recommended for OGSE scans as they use a separate b-value (found on the Sequence >> Special tab). Using this approach you essentially use the UI to encode your shells instead of scaling your diffusion vectors manually (ie. all your diffusion vectors will be scaled by the sequence to have different b-values). In this case your diffusion weightings would be equal to the number of shells you want and the b-values you enter will be the different b-values that are acquired. Here you can include the b0 using the UI as described above or you can keep it in the diffusion table, either should be fine.   

**Important** If you are going to implement your protocol with the mosaic option enabled, it's highly recommended that you simulate your protocol in poet first and view the results before proceeding to the scanner to ensure that the number of acquisitions and their b-values are what you expect.
 
For questions about using mosaic contact Corey 