---
layout: default
title: Modules
parent: Useful Tools
nav_order: 6
---

# Modules
The modules system is a flexible way to "activate" software in the current working session (i.e. the active terminal). It's common in computer clusters. It helps to avoid software conflicts and is a way to have installed different versions of the same software. CBS server and baron1/10 uses it to admin it's software.

## Using modules

In general you only need two command to work with the modules system:
- `module load <software>` : Loads the module <software> into your enviroment.
- `module avail` : List the available software. It also displays loaded modules.


There is a small version of these two commands for easy use in the terminal:
- ` ml <software> `: Equivalent to the module load command.
- ` ml av `: Equivalent to the module avail command.

By default module loads the most recent version of the software available. You can specify a different version typing the full version from the list ( e.g. module load mrtrix/3.3).

Other useful commands:
- ` module unload <software> `: Unload the module from your sessions.
- ` module switch <software/v> `: Switch to a different version of a module
- ` module list `: List your current active modules.
- `module purge`: Unloads all modules.

A more comprehensive list of options [here](https://lmod.readthedocs.io/en/latest/010_user.html): 

### CBS server

CBS server is configured to automatically load common modules for neuroimaigng analysis. You can still use the commands in the above section to explore more modules or switch to different versions.

### baron1 and rri-baron10
baron1 and rri-baron10 have common neuroimaging software installed that are accesible with modules. These servers don't load any modules automatically to your session. You can load the modules manually for each session but you can create your own configuration to load modules by default in any session.

An easy way to automatically load modules by default is to edit your bashrc (typing 'gedit .bashrc' in a new terminal) and add (at the end of the file) the list of modules that you want to load (e.g. module load matlab).
