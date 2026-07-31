---
layout: default
title: Using CBS SLurm for MatMRI Reconstruction
parent: CBS SLURM
nav_order: 1
---

# Using CBS SLurm for MatMRI Reconstruction

This page describes how to run the MATLAB-based MRI reconstruction pipeline ([`initReco_slurm.m`](#reference-matlab-recon-script)) as a SLURM job array on the CBS cluster, using [the reference submission script](#reference-submission-script) below.

> For general CBS SLURM cluster background (partitions, hardware, storage tiers, module system) see the [CompCore SLURM guide](https://hackmd.io/@CompCore/cbs_slurm). This page only covers the MATLAB recon-specific setup.

## Overview

The recon runs as a **job array**: one array task per raw `.dat` file, where `file_num == SLURM_ARRAY_TASK_ID`. Each task:

1. Stages the project directory to fast node-local scratch (`/localscratch`).
2. Runs the MATLAB recon headlessly against the staged copy.
3. Syncs results back to the shared project directory on exit — even if MATLAB errors out.

## Prerequisites

- A project directory on the lab share (NFS) containing `raw/`, `sim_files/`, `field_map/`, and the `fpDat` folder, laid out the same way as your local setup.
- Access to the recon package (`baron-matlab-recon-fork`) so `addMatlabPath.m` can be found.
- A `logs/` directory inside the project directory (SLURM writes array task output there).

## Directory layout expected by the script

```
PROJECT_DIR/
├── raw/            # input .dat files, one per array task
├── sim_files/      # siemensRaw2Protocol (MATMRI) to get prot. Run sim -p .prot in IDEA.*
├── field_map/      # Contains the .dat for the field map.
├── fpDat/          # Contains the concurrent or sequential field probe measurements.
└── logs/           # created before submission; holds recon_<jobid>_<task>.out
```

\* For running `sim -p .prot` over multiple `.prot` files with a single call, see [siemens-idea-sim-runner](https://git.cfmm.uwo.ca/fledo/siemens-idea-sim-runner).

## Setting up your submission script

Copy the [reference submission script](#reference-submission-script) below into your project as `submit_recon.sh` and edit the three paths at the top:

```bash
# ----- EDIT THESE THREE PATHS -------------------------------
PROJECT_DIR=/nfs/menon/fledoNFS/<your_project_dir>
RECON_PKG=/nfs/menon/fledoNFS/BaronRecoPkg/baron-matlab-recon-fork
# ------------------------------------------------------------
```

`RECON_PKG` should point wherever you `git clone`d the MatMRI recon package — it defaults to the lab's shared copy above, but set it to your own clone if you're using one.

Also check the `#SBATCH --array=` range matches the number of `.dat` files you're reconstructing (e.g. `--array=1-6` for 6 files).

## GPU / partition selection

GPU card type is controlled by `--partition`, not by the `--gpus-per-node` gres string:

| Partition | GPU        | VRAM  | Notes                              |
|-----------|------------|-------|-------------------------------------|
| `vx`      | A100       | 40 GB | Faster compute; has internet access |
| `hx`      | L40        | 46 GB | More VRAM, best for large raw data; slower per-op than `vx`; no internet access |
| `all`     | whatever the scheduler picks (seen both L40s and A100) | — | — |

## Submitting the job

Open a terminal on CBS Basic or Heavy, and log in to the SLURM login/master node:

```bash
ssh <user>@rri-cbs-slurm
```
Enter your password and you should be logged in to the master node.

**Warning**: do not run any analysis or recon on the master node — it's used to submit jobs only.

```bash
cd /path/to/your/project/directory
mkdir -p logs
sbatch submit_recon.sh
```

Check progress:

```bash
squeue -u $USER
tail -f logs/recon_<jobid>_<task>.out
```

## What the script does under the hood

- **Module load**: `module load matlab` — run `module avail matlab` first if you need a specific version.
- **rsync discovery**: some compute nodes have a leaner `PATH` than the login node, so the script falls back to checking `/usr/bin`, `/bin`, and `/usr/local/bin` for `rsync` before giving up.
- **Staging**: input data is rsynced to `/localscratch/recon_${SLURM_JOB_ID}_${FILE_NUM}` (excluding any existing `Recon_*` output and `logs/`) so MATLAB reads/writes against local SSD instead of the network share.
- **Copy-back on exit**: a `trap copy_back EXIT` ensures results sync back to `PROJECT_DIR` whether the job succeeds or MATLAB throws an error. Unchanged files (e.g. large `raw/*.dat`) are skipped via rsync's size+mtime check, so the copy-back is cheap.
- **Recon call**: runs headless via `matlab -batch`, adding the recon package path and the submission directory, then calling `initReco_slurm(${FILE_NUM})`.

## Troubleshooting

- **`rsync not found` error**: the compute node image is missing rsync — ask a cluster admin to install it, then resubmit.
- **Job dies with no output copied back**: check the `.out` log for the MATLAB error; the `copy_back` trap still runs on failure, so partial results should be present in `PROJECT_DIR`.
- **Wrong GPU type**: double check `--partition` — it determines the card, not `--gpus-per-node`.

## Reference submission script

Save as `submit_recon.sh` in your project directory:

```bash
#!/bin/bash
# submit_recon.sh
#SBATCH --job-name=mri_recon
#SBATCH --time=12:00:00
#SBATCH --cpus-per-task=16
#SBATCH --mem=80G
#SBATCH --array=1-6
#SBATCH --gpus-per-node=1
#SBATCH --partition=vx
#SBATCH --output=logs/recon_%A_%a.out

# ============================================================
# MATLAB MRI recon on the CBS SLURM cluster (job array)
# One array task per raw .dat file (file_num = SLURM_ARRAY_TASK_ID)
#
# GPU: you get the WHOLE card, so VRAM is fixed by card type. Card type
# is picked via --partition, NOT via a gres string on --gpus-per-node:
#   partition=all -> whatever the scheduler picks (seen both l40s and a100)
#   partition=vx  -> a100 nodes (40 GB VRAM, faster compute; pinned above)
#   partition=hx  -> l40 nodes (46 GB VRAM, best for big raw data, slower, no internet)
# CPU-only (no GPU): delete the --gpus-per-node line entirely.
#
# Usage (from the project directory on the master node):
#   mkdir -p logs
#   sbatch submit_recon.sh
# ============================================================

set -euo pipefail

# ----- EDIT THESE THREE PATHS -------------------------------
# Project dir on your lab share: must contain raw/, sim_files/,
# field_map/, and fpDat/ (same layout you use locally)
PROJECT_DIR=/nfs/menon/fledoNFS/<your_project_dir>

# Recon package (for addMatlabPath.m)
RECON_PKG=/nfs/menon/fledoNFS/BaronRecoPkg/baron-matlab-recon-fork
# ------------------------------------------------------------

# Check what's available with: module avail matlab
module load matlab

# ----- Locate rsync (some compute nodes have a leaner PATH/image than the login node) -----
RSYNC=$(command -v rsync || true)
if [ -z "${RSYNC}" ]; then
    for candidate in /usr/bin/rsync /bin/rsync /usr/local/bin/rsync; do
        if [ -x "${candidate}" ]; then
            RSYNC="${candidate}"
            break
        fi
    done
fi
if [ -z "${RSYNC}" ]; then
    echo "ERROR: rsync not found on $(hostname) (checked PATH and /usr/bin, /bin, /usr/local/bin)." >&2
    echo "Ask your cluster admin to install rsync on this node, then resubmit." >&2
    exit 1
fi

JOB_T0=$(date +%s)
FILE_NUM=${SLURM_ARRAY_TASK_ID}
echo "Array task ${FILE_NUM} on $(hostname), ${SLURM_CPUS_PER_TASK} CPUs"
echo "CUDA_VISIBLE_DEVICES (GPU device indices): ${CUDA_VISIBLE_DEVICES:-none}"
nvidia-smi --query-gpu=name,memory.total --format=csv,noheader || echo "nvidia-smi not available"

# ----- Stage input data to node-local scratch (fast SSD, auto-cleaned) -----
WORKDIR=/localscratch/recon_${SLURM_JOB_ID}_${FILE_NUM}
mkdir -p "${WORKDIR}"
echo "Staging data to ${WORKDIR} ..."
t0=$(date +%s)
"${RSYNC}" -a --exclude 'Recon_*' --exclude 'logs' "${PROJECT_DIR}/" "${WORKDIR}/"
echo "Staging done in $(( $(date +%s) - t0 )) seconds"
cd "${WORKDIR}"

# ----- Always copy everything back, even if MATLAB errors ------
# Unchanged files (e.g. large raw/*.dat) are skipped via rsync's size+mtime
# quick-check, so this is cheap despite covering the whole staged tree.
copy_back() {
    echo "Syncing results back to ${PROJECT_DIR}/ ..."
    "${RSYNC}" -a "${WORKDIR}/" "${PROJECT_DIR}/"
    elapsed=$(( $(date +%s) - JOB_T0 ))
    echo "Task ${FILE_NUM} total time: ${elapsed} seconds ($(printf '%02d:%02d:%02d' $((elapsed/3600)) $((elapsed%3600/60)) $((elapsed%60))))"
}
trap copy_back EXIT

# ----- Run the recon headless --------------------------------
matlab -batch "\
    run('${RECON_PKG}/addMatlabPath.m'); \
    addpath('${SLURM_SUBMIT_DIR}'); \
    initReco_slurm(${FILE_NUM});"

echo "Task ${FILE_NUM} finished OK"
```

## Reference MATLAB recon script

`initReco_slurm(file_num)` is the MATLAB entry point called by `submit_recon.sh` — it processes exactly one raw file per array task (`file_num = SLURM_ARRAY_TASK_ID`), builds an `opt` struct, calls `reconModBased_twix`, and saves results into an auto-named `Recon_<optTag>/` folder.

This copy is from a specific project, so treat the paths, `b0mapFiles`, and `skopeID` lookup tables as an example to adapt, not a drop-in script:
- `simPath` / `skopePath` — point at your own sim/skope output directories.
- `b0mapFiles` / `skopeID` — one entry per raw file, in the same order as `dir('./raw/*.dat')`; update the count and values for your dataset.
- `opt.*` fields — recon options (spiral k-space correction, field-map correction, navigator recon, etc.) — tune for your acquisition.

```matlab
function initReco_slurm(file_num)
    % SLURM version: processes exactly ONE raw file, given by file_num
    % Submitted as a job array; file_num = SLURM_ARRAY_TASK_ID.
    % Path setup (addMatlabPath.m) is done by the sbatch script.

    % --- Respect the SLURM CPU allocation ---
    ncpus = str2double(getenv('SLURM_CPUS_PER_TASK'));
    if ~isnan(ncpus) && ncpus > 0
        maxNumCompThreads(ncpus);
    end

    % --- GPU diagnostics ---
    fprintf('=== GPU Diagnostics ===\n');
    n = gpuDeviceCount;
    fprintf('  gpuDeviceCount: %d\n', n);
    if n > 0
        g = gpuDevice();
        fprintf('  Device: %s\n', g.Name);
        fprintf('  TotalMemory: %.1f GB\n', g.TotalMemory / 1e9);
        fprintf('  AvailableMemory: %.1f GB\n', g.AvailableMemory / 1e9);
        fprintf('  ComputeCapability: %s\n', g.ComputeCapability);
    else
        fprintf('  WARNING: No GPU device found — recon will run on CPU only\n');
    end
    fprintf('=======================\n\n');

    clear results
    clear opt
    rawFolder = './raw';
    rawFiles = dir(fullfile(rawFolder, '*.dat'));
    simPath = '/nfs/menon/fledoNFS/SpiralK_PAPER/RAW/2026_07_28_highResPhantomTest/sim_files';

    skopePath = '/nfs/menon/fledoNFS/SpiralK_PAPER/RAW/2026_07_28_highResPhantomTest/2026_07_28_HighResPhantomTest';
   
    b0mapFiles = repmat({'field_map/meas_MID00035_FID160823_gre_field_mapping.dat'}, 6, 1);
    skopeID = [4,2,5,3,4,5];

    opt.SpiralK.status = true;
    opt.findDelB0 = 1;
    opt.stopCrit.regParam = 0.2;
    opt.Nav.doRecon = true;
    opt.sampApproach = 1;
    opt.SpiralK.tempSigma = 1.5;

    % --- Subset recon (limit runtime): 16 slices, 20 volumes ---
    % opt.sliSub = 1:16;
    % opt.volSub = 1:20;

    opt.optFP = [];
    opt.optFP.fitOrder = 2;
    opt.optFP.fpBasisCompress = 0;
    opt.sampOpt.directComplex= 1;

    %--- Validate input ---
    assert(file_num >= 1 && file_num <= length(rawFiles), ...
        'file_num %d out of range (found %d raw files)', file_num, length(rawFiles));
    assert(length(rawFiles) == length(skopeID) && length(rawFiles) == length(b0mapFiles), ...
        'raw file count (%d) does not match skopeID/b0mapFiles entries (%d/%d) -- update the lookup tables', ...
        length(rawFiles), length(skopeID), length(b0mapFiles));

    k = file_num;
    rawFilename = fullfile(rawFolder, rawFiles(k).name);
    b0mapFilename = b0mapFiles{k};

    [~, simPathName, ~] = fileparts(rawFilename);
    simPath_file = fullfile(simPath, simPathName);

    % Short scan ID for filenames: 'meas_MID00256_FID151502_long_desc' -> 'MID00256_FID151502'
    scanID = shortenScanName(simPathName);

    % --- Guard so buildOptTag never hits a missing field ---
    if ~isfield(opt.SpiralK, 'tempSigma') || isempty(opt.SpiralK.tempSigma)
        opt.SpiralK.tempSigma = 0;
    end

    % --- Auto-generate output folder name from opt values ---
    optTag = buildOptTag(opt);
    outputFolder = ['./Recon_' optTag];
    fprintf('Output folder: %s\n', outputFolder);
    if ~exist(outputFolder, 'dir')
        mkdir(outputFolder);
    end

    opt.doSaveNifti = fullfile(outputFolder, [scanID '_' optTag '_']);
    opt.Nav.saveDir = fullfile(outputFolder, [scanID '_NAV.mat']);

    fprintf('\n\nProcessing %s (file_num %d of %d)\n', rawFiles(k).name, k, length(rawFiles));

    % Diagnostic-only: identify the skope scan file for this skopeID.
    % Match on '.scan' specifically since each skopeID has exactly one,
    % unlike the bare '%d*' glob which also catches sibling files
    % (.raw/.trig/.kcoco/.kspha/etc.) sharing the same leading digit.
    skopeMatch = dir(fullfile(skopePath, sprintf('%d_*.scan', skopeID(k))));
    if isempty(skopeMatch)
        skopeMatchName = sprintf('<no .scan file found for skopeID %d>', skopeID(k));
    else
        skopeMatchName = skopeMatch(1).name;
    end

    fprintf('Arguments for reconModBased_twix:\n');
    fprintf('  rawFilename: %s\n', rawFilename);
    fprintf('  skopePath: %s\n', fullfile(skopePath, skopeMatchName));
    fprintf('  skopeID: %d\n', skopeID(k));
    fprintf('  simPath: %s\n', simPath_file);
    fprintf('  b0mapFilename: %s\n', b0mapFilename);
    fprintf('  opt.doSaveNifti: %s\n\n', opt.doSaveNifti);

    [imout, optout, b0map, imb0, imMask] = reconModBased_twix(...
        rawFilename, skopePath, skopeID(k), simPath_file, b0mapFilename, opt);

    % --- Store results for THIS file only (no shared struct across jobs) ---
    results = struct();
    results.rawFile = rawFiles(k).name;
    results.imout   = imout;
    results.optout  = optout;
    results.b0map   = b0map;
    results.imb0    = imb0;
    results.imMask  = imMask;

    resultsFilename = fullfile(outputFolder, ['recon_results_' scanID '.mat']);
    try
        save(resultsFilename, 'results', 'opt', '-v7.3');
    catch ME
        warning('Could not save results: %s', ME.message);
    end

end

function tag = buildOptTag(opt)
    parts = {
        sprintf('spiralK%s',   bool2str(opt.SpiralK.status))
        sprintf('findB0%d',    opt.findDelB0)
        sprintf('regParam%s',  num2tag(opt.stopCrit.regParam))
        sprintf('nav%s',       bool2str(opt.Nav.doRecon))
        sprintf('sampApp%d',   opt.sampApproach)
        sprintf('fitOrd%d',    opt.optFP.fitOrder)
        sprintf('fpComp%d',    opt.optFP.fpBasisCompress)
        sprintf('tempSig%s',   sigma2str(opt.SpiralK.tempSigma))
        };
    tag = strjoin(parts, '_');
end

function s = num2tag(x)
    s = strrep(num2str(x), '.', 'p');
    s = strrep(s, '-', 'm');
end

function s = sigma2str(sigma)
    if isempty(sigma) || sigma == 0
        s = 'Off';
    else
        s = num2tag(sigma);
    end
end

function s = bool2str(b)
    if b, s = 'On'; else, s = 'Off'; end
end

function s = shortenScanName(name)
    tok = regexp(name, '^meas_(MID\d+_FID\d+)_.+$', 'tokens', 'once');
    if ~isempty(tok)
        s = tok{1};
    else
        s = name;
    end
end
```

## See also

- [CompCore CBS SLURM guide](https://hackmd.io/@CompCore/cbs_slurm) — partitions, storage tiers (`/nfs/<pi>`, `/nfs/scratch`, `/localscratch`), module system, and general submission basics.

---

Contributed by fledo (fledo@uwo.ca)
