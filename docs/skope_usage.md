---
layout: default
title: Skope Usage
nav_order: 6
---
# Skope Usage

## Checklists
Skope usage checklist: [SkopeChecklist.docx](https://uwoca.sharepoint.com/:w:/s/BaronLab/EZjDY7DrkPtAhamTVLDtxpcB6N6qzYAAQtsedY-xCmXctg?e=qpQtmH)

## Skope DNS Server
can be reached at skope-cfmm.robarts.ca (129.100.44.178)

## Standard operating procedures

### Setup
1. To prepare the device for measurements, position the bottom of the field probe coil at the head of the cradle. First, connect the transmit cable to the socket. At this point, the patient/phantom being scanned can be placed on the scanner bed. Place the top half of the coil over the head and gently secure it into the bottom half. Ensure the two field probe coil cables are underneath the receiver cables. Connect the two field probe coil cables into "FP1" and "FP2". Finally, connect the 4 receiver cables into their respective sockets.
2. Under the MRI console workstation, locate the trigger box and swap out the BNC cable with the green-marked "skope" BNC cable.  
3. After the hardware is set up and ready to measure, power up the Field Monitoring System by switching on the RF back end's main circuit breaker and then power up the digitizer/computer by pressing the main power button.

### Operation
1. Launch the Skope application by selecting the icon "skope-fm 2020.1", which can be located on the desktop. You will be prompted to select a project path before being able to proceed, so select an existing directory or create a new folder. 
2. With the fpcali sequence loaded in the console patient study, a calibration scan can be performed by going to **Plan Scan** and selecting **Calibrate**. Select **Calibrate**, change the Name/Description of the calibration if necessary, and select **Start Scan**. Once selected, the fpcali sequence can be ran in the MRI console. If successful, the display will show the calculated decay signals as well as individual channel SNR and lifetime performances. Green boxes indicate sufficient SNR and lifetime, while red boxes indicate lifetimes that are too short, or the SNR is too low. Press Done to see the individual field probe positions as well as the offresonance values. By pressing Done once more, the calibration can be saved by going to save calibration. To check the current calibration used for scans or to upload a new calibration from the scan session, select Calibration Set in the Plan Scan settings, and browse available calibrations. **NOTE:** The general guidance is to keep the B0 shim exactly the same between the calibration sequence and the imaging sequences you want to monitor.  
3. To propagate B0 and B1 shims through different sequences, the B0 and B1 shims can be saved via the command prompt and going to c:\Medcom\MriCustomer\bin. Once you've shimmed on an acquisition and have started it, append the same acquisition down, open it, and run the 'SaveB0Shims.bat' and 'SaveB1Shims.bat'. Then for all subsequent sequences that you want to maintain the same shims for, run commands 'LoadB0Shims.bat' and 'LoadB1Shims.bat'. Also, copy the adjustment volume from the previous acquisition and ensure the B0 shim mode is set to brain from the previous acquisition.
4. The frequency offset should also be maintained between acquisitions and field maps. To do so, open up **options** -> **adjustments** -> **frequency tab** and select **apply**. 
5. The B0 map should be acquired right after the calibration is done so that any B0 drift from temperature is consistent between the two.
6. **RECOMMENDED:** run a quick acquisition of a few fids ***both before and after*** the scan of interest without using triggers (while MRI is idle). This can allow for correction of field offset changes from motion between calibration scan and scan of interest.
7. To field monitor a sequence, cue up the appropriate sequence in the patient study. Based on the parameters of the sequence, skope acquisition parameters can be adjusted accordingly from the same Plan Scan window.
    1. Set # of dynamics. Generally = num diff directions * num averages * num slices
        - Typically, there are some dummy/reference scans that play out before the actual sequences to be monitored occur. Therefore, if you know the number of dummy scans and the number of slices to be monitored, set the number of dynamics equal to that amount. If you just want to be safe, or if you don't know how many dummy scans there are, set the # of dynamics to some large amount that exceeds the number to be monitored (amount of slices*10 for example). The scan will stop on its own once all the dynamics are acquired.
    2. Set interleave TR
        - Important to note is that triggers from the pulse sequence sent to the skope system are ignored for an acquisition period equal to (Interleave TR) x (# of Interleaves). For acquisitions of more than one slice within a volume, set the Interleave TR to a value tens of milliseconds less than the expected trigger time for the respective slice. For example, if the slices of interest are to be triggered at 0 and 550 ms, set the interleave TR to 500 ms or something in that region. 
    3. Set acquisition time
        - Default is 50 ms, but this is way more than necessary for typical spiral, and could be too short for some EPI
        With the necessary parameters selected, select Start Scan to put the system in a position to field monitor. Start the scan of interest in the MRI console, which will initiate the triggers and the collection of skope data. Acquisition should continue on its own until completion. In the case that more dynamics have been listed than there actually are, no more triggers will be fired. At this point, Stop Scan can be pressed.
8. With the necessary parameters selected, select Start Scan to put the system in a position to field monitor. Start the scan of interest in the MRI console, which will initiate the triggers and the collection of skope data. Acquisition should continue on its own until completion. In the case that more dynamics have been listed than there actually are, no more triggers will be fired. At this point, **Stop Scan** can be pressed.
9. Two visualization windows can be used to visualize various data types such as raw magnitude, k-coefficients, parameters k-values, and gradient values. The dynamics, interleaves, and acquisition sample period to be displayed can also be changed by adjusting the bars at the top of the interface. 
10. Results are automatically saved and the different scans can be viewed by pressing  **Browse Scans** and selecting the scan of interest.

### Reprocessing 
1. Select an acquisition in the **Plan Scan** tab and select **Reprocess**.
2. Reprocessing can involve deselecting individual probes or changing the basis order. If deselecting individual probes, it is recommended to select **2nd Order Spherical Harmonics** instead of the default **Custom Spherical Harmonics** (if at least 9 probes remain selected) . 
3. Select the **Start Reprocessing**. Data is then reprocessed and saved with a new ID.

### Data Transfer
- Open git bash from the start menu, and use "scp" to transfer to baron1 or other server

### Shutdown
1. Before the field probe coil is removed or manipulated, the digitizer/computer first needs to be shut down via the Windows start menu (wait until the digitizer switches off). **NOTE**: It often occurs that the computer does not shut down properly, indicated by the green power button remaining on. Hold the power button down for a few seconds until the light turns off. This is the only solution for this at the moment. 
2. Turn off the power of the RF back end via the provided circuit breaker on the RF back end. 
3. All connections to the scanner must be removed before the MR scanner is used again without the field monitoring system. To remove the top half of the coil, lift the latch and pull the top up.
4. Remember to switch the BNC cables connected to the trigger box from the skope cable (green tape) to the standard cable (usually white tape).

### Simulating the B0 ECC Term

To correct for B0 eddy currents, the scanner applies frequency shifts that are invisible to the Skope system. Currently, the only method to correct for this is to simulate the execution of the pulse sequence. The steps to do so are:
1. Make sure you have the VE12U virtual machine for the 7T or VE11C virtual machine for the 3T. See [https://git.cfmm.uwo.ca/cfmm/info/-/wikis/siemens_idea](https://git.cfmm.uwo.ca/cfmm/info/-/wikis/siemens_idea)
2. Generate a .pro file from your specific acquisition. This can be obtained from the raw .dat file using matlab functions in the baron-matlab-recon repo [https://git.cfmm.robarts.ca/baron-lab/baron-matlab-recon](https://git.cfmm.robarts.ca/baron-lab/baron-matlab-recon):
    - Raw: siemensRaw2Protocol.m
    - Dicom: dicom2protocol.m
3. Start the VE12U or VE11C virtual machines if scan was performed on the 7T or 3T respectively, and:
    1. If you haven't yet pulled the ep2d_baron repo [https://git.cfmm.uwo.ca/baron-lab/a_ep2d_diff_baron_ve](https://git.cfmm.uwo.ca/baron-lab/a_ep2d_diff_baron_ve) and compiled the sequence, do so. Compiling just the debug version is fine. See Pulse Programming section.
        - Make sure the branch you compile is the branch used to acquire the scan (should usually be the master branch)
    2. Copy the .pro file from above step into the pulse sequence directory
    3. Open IDEA.net and choose the only available snapshot view
    4. Run "sys", and choose option "Investigational_Device_7T-XT_Plus_8Tx" for 7T and "Prisma-XR" for 3T
    5. Depends on 3T or 7T:
        - 7T: Run "sim -p filename.pro"
        - 3T: Open POET, load filename.pro, run simulation from poet
            - files are saved in C:/Users/cfmm/AppData/Local/Temp/SequenceSimulation
            - NOTE: AppData may be a hidden folder
    6. Copy the files ending with "GRX", "GRY", "GRZ", and "INF" to wherever you'll want them to live (typically near where the data lives).
        - If sending over a network, compress them to a zip file first (the INF can get huge, but it compresses VERY efficiently)

### Using Skope data in image recon
1. Pull both the matMRI [https://gitlab.com/cfmm/matlab/matmri](https://gitlab.com/cfmm/matlab/matmri) and baronRecon [https://git.cfmm.uwo.ca/baron-lab/baron-matlab-recon](https://git.cfmm.uwo.ca/baron-lab/baron-matlab-recon) repositories. 
2. Use the recon_ModBased.m from the baronRecon repo to perform the recon. Inputs include:
    1. filename for raw .dat file. Can include full path (e.g., '/mnt/baron1_loca1/data/myproject/rawdata1.dat')
    2. path to directory holding skope acquisition data
    3. ID for specific skope scan (because the skope data path might contain multiple acquisitions. The names for each acquisition start with the ID number).
    4. filename for the sim results described above, excluding "_GRX.dsv"
