# Modules
The modules system is a flexible way to "activate" software in the current working session (i.e. the active terminal). It's common in computer clusters to avoid software conflicts or to have installed different versions of the same software. CBS server uses it to admin it's software.

## Using modules

ToDo: how to us modules 

### CBS server

CBS server is configured to automatically load common modules for neuroimaigng analysis. You can still use the commands in the above section to explore more modules or switch to different ones.

### baron1 and rri-baron10
baron1 and rri-baron10 have common neuroimaging software installed that are accesible with modules. These servers don't load any modules automatically to your session. You can load the modules manually for each session but you can create your own configuration to load modules by default in any session.

An easy way to automatically load modules by default is to edit your bashrc (typing 'gedit .bashrc' in a new terminal) and add (at the end of the file) the list of modules that you want to load (e.g. module load matlab).
