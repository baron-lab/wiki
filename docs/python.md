---
layout: default
title: Python
nav_order: 7
---
# Python

## conda
- conda is a package manager that saves a lot of headache in terms of installation and preventing conflicts between applications
- can install anaconda or miniconda. Anaconda is pretty bloated, so I recommend minconda. Use installation instructions at [https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)
- lots of good info for using conda at [https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)
    - Each "environment" is isolated, so that you can keep apps that conflict with each other separate.
- Do not install lots of applications on servers like baron1. For a shared system, it is better for me to install it once for everybody to use (with conda, everyone would have their own copy of the app, which is innefficient).
    - However, conda would still be useful for python packages in your home directory

## pip (package installer for Python)
- pip is a package manager for python
- conda can do almost everything pip can. However, one thing pip can do is install directly from github, which is useful for CFMM's pybruker package (see below)

## pybruker
- used for reading bruker enhanced dicoms
- see [gitlab.com/cfmm/bruker/pybruker](gitlab.com/cfmm/bruker/pybruker)
- use the following (see [gitlab.com/cfmm/bruker/pybruker](gitlab.com/cfmm/bruker/pybruker) for more info)
> <pre>pip install pydicom
> pip install git+https://gitlab.com/cfmm/bruker/pybruker </pre>
- Pip may give a warning that the installed executables are not on the path. If so, add the path to ~/.bashrc using the below template (you will need to change the path to whatever pip tells you):
> <pre># Path for executables installed by pip
> export PATH="/path/to/add:$PATH"</pre>