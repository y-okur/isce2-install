# Manual for ISCE installation with Anaconda / Miniconda (MacOS)


There are several ways to install ISCE on MacOS Machine with Anaconda / Miniconda.
* Install from conda-forge repositories
* Build from source code with cmake

First, a conda distribution must be installed.

There are two versions you can choose from.  
1. Anaconda  
It comes with a lot of python modules preinstalled (numpy, pandas, matplotlib etc.). You can choose this if you have enough disk space. Also, it comes with anaconda-navigator which has various tools such as DataLore, JupyterLab & Notebook, Spyder IDE, VS Code, etc.  
2. Miniconda  
It only installs conda environment with basic python packages. You can install all the other packages or tools later.

## What should you choose? Anaconda or Miniconda?
Choose Anaconda if you:
1. Are new to conda or Python.
2. Like the convenience of having Python and over 1,500 scientific packages automatically installed at once.
3. Have the time and disk space---a few minutes and 3 GB.
4. Do not want to individually install each of the packages you want to use.
5. Wish to use a curated and vetted set of packages.

Choose Miniconda if you:
1. Do not mind installing each of the packages you want to use individually.
2. Do not have time or disk space to install over 1,500 packages at once.
3. Want fast access to Python and the conda commands and you wish to sort out the other programs later.

## Installing Anaconda / Miniconda
Download either Anaconda or Miniconda:

### 1. Anaconda:
* You can either choose graphical or command line installer from >https://www.anaconda.com/products/individual#macos 
 
* Direct link for 2021.05 version > https://repo.anaconda.com/archive/Anaconda3-2021.05-MacOSX-x86_64.sh
* Make sure to verify hash code after downloading. Link to check hash codes > https://docs.anaconda.com/anaconda/install/hashes/index.html 
* Run `shasum -a 256 filename` on the terminal.

### 2. Miniconda:
* You can either choose graphical or command line installer from > https://docs.conda.io/en/latest/miniconda.html
* Direct link for latest version > https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh
* Hash codes are also written on the download page.

* Use the graphical installer or run `bash Miniconda3-latest-MacOSX-x86_64.sh` or `bash Anaconda3-2021.05-MacOSX-x86_64.sh` on the terminal to install from the command line.
* Choose "yes" for prompts. This will install Anaconda / Miniconda in your home directory.

### 3. Checking the Installation
The installation will write conda initialize in your default shell profile. You can find the shell profile in your home directory by running:
```
cd  
ls -la  
```
Zshell > .zprofile
Bash   > .bash_profile

* An example for conda initialize:
```
# >>> conda initialize >>>  
# !! Contents within this block are managed by 'conda init' !!  
__conda_setup="$('/Users/yagizalp/anaconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"  
if [ $? -eq 0 ]; then  
    eval "$__conda_setup"  
else  
    if [ -f "/Users/yagizalp/anaconda3/etc/profile.d/conda.sh" ]; then  
        . "/Users/yagizalp/anaconda3/etc/profile.d/conda.sh"  
    else  
        export PATH="/Users/yagizalp/anaconda3/bin:$PATH"  
    fi  
fi  
unset __conda_setup  
# <<< conda initialize <<<  
```
If you want to use conda with a different shell (bash for example) you need to run conda init.
```
source /<path to conda>/bin/activate  
conda init bash  
```
This will write conda initialize in .bash_profile.  
Close and reopen the terminal and check if you have (base) at the beginning of the terminal.   
```(base) bash-3.2$  ```
If the (base) is missing you may have to create .bashrc file in your home directory and cut conda initialize from .bash_profile paste it onto .bashrc. You can create .bashrc file by running:  
```
cd  
touch .bashrc  
```

Run `echo $PATH` to check if you have anaconda / miniconda in your path.  
Run `which python` and `which python3` to make sure the python is running through anaconda / miniconda path.  
If not, check your path `sudo nano /etc/paths` and make sure to comment other python paths.  
Also check shell profiles (.bash_profile, .bashrc, .zprofile, .zshrc) and and comment python paths other than anaconda / miniconda.  

### 4. Basic Conda Commands
When you open a new terminal you should see (base) at the beginning of the prompt. Some useful conda commands are:  
`conda list`  shows installed packages  
`conda info --envs`  shows available environments  
`conda create -n envName python=3.8`  creates a new environment called "envName" with python 3.8  
`conda activate envName` activates envName environment  
`conda deactivate envName` deactivates envName environment.  
`conda env remove -n envName` deletes envName environment.  
  
We use environment because we can create separate python builds (different versions or with different modules) on the same machine without messing up the paths. This way you can activate which environment you want to work with and keep track of module versions, compatibility, etc.

## ISCE

There are two ways to install ISCE with anaconda / miniconda.
### 1. Installing from conda-forge repositories.  
This is very easy, but you have to add ISCE variables to your path manually.
* Create an environment called "isce2" (or you can choose whatever you like) with python 3.8 version.  
```
conda create - n isce2 pyhon=3.8  
```  
then run  
```
conda config --set channel_priority strict  
```  
finally  
```
conda install -c conda-forge isce2
```
or
```
conda install -c "conda-forge/label/cf202003" isce2
```
Conda will automatically installs all dependencies and solve the environment.  
After the installation is finished, check if the $ISCE_HOME variable is set properly by running:
```
echo $ISCE_HOME
/Users/yagizalp/anaconda3/envs/isce262/lib/python3.11/site-packages/isce
````


```
#!/bin/env bash -f
#ISCE
export CPL_ZIP_ENCODING=UTF-8
#export ISCE_ROOT="~/src/isce2"
export ISCE_HOME="${CONDA_PREFIX}/lib/python3.8/site-packages/isce"
export ISCE_STACK="${CONDA_PREFIX}/share/isce2"
export PATH="${ISCE_HOME}/bin:${ISCE_HOME}/applications:${PATH}"
export PYTHONPATH="${ISCE_HOME}/applications:${PYTHONPATH}"
#--select topsStack..if you wish to use stripmapstack, change the the following lines.
export stripmapStack=""
#export stripmapStack=${ISCE_STACK}/stripmapStack
export topstack=""
export PATH_ALOSSTACK=${ISCE_STACK}/alosStack
##export topstack=${ISCE_STACK}/topsStack
export timeseries="${ISCE_STACK}/timeseries/prepStackToStaMPS/bin"
export PATH="${PATH}:${topstack}:${stripmapStack}:${PATH_ALOSSTACK}:${timeseries}"
export PYTHONPATH="${topstack}:${stripmapStack}:${PATH_ALOSSTACK}:${timeseries}:${PYTHONPATH}"
```
After you activate "isce2" environment run:
```
source /path to file/iscesource
```
This will temporarily add ISCE variables to your path. You have to run the source again if you close the terminal. This is useful if you want to have different versions of ISCE installed at the same time. If you want to use only one version and set up paths manually for each version then you can add above lines in your preferred shell profile file.  
  
If the first method fails to solve environment or you don't want to set up paths manually you can build ISCE with cmake.  
### 2. Building ISCE with cmake. (recommended) 
Create an environment with python 3.8 version and activate:  
```
conda create -n isce2 python=3.8
conda activate isce2
```
Install required packages:
```
conda install -c conda-forge git cmake cython gdal h5py libgdal pytest numpy fftw scipy basemap opencv pybind11 shapely
```
To compile/install mdx, you will also need:  
```
conda install -c conda-forge openmotif openmotif-dev xorg-libx11 xorg-libxt xorg-libxmu xorg-libxft libiconv xorg-libxrender xorg-libxau xorg-libxdmcp
```
Optionally, if you have NVDIA graphics card, you can install CUDA compiler to speed up the process.  
You will also need C/C++/Fortran compilers. Easiest way to get these to install Xcode from apple store.  
Now you are ready to compile isce2.  
Download source package:  
```
mkdir -p $HOME/tools/src 
cd $HOME/tools/src 
git clone https://github.com/isce-framework/isce2.git
```
Compile and install isce2:  
```
cd $HOME/tools/src/isce2
mkdir build  && cd build
cmake .. -DCMAKE_INSTALL_PREFIX=$CONDA_PREFIX -DPYTHON_MODULE_DIR=lib/python3.8/site-packages -DCMAKE_CUDA_FLAGS="-arch=sm_60" -DCMAKE_PREFIX_PATH=${CONDA_PREFIX} -DCMAKE_BUILD_TYPE=Release 
make install
```

### Checking the Installation
Close & reopen the terminal and activate the "isce2" environment. Then run:   
```
conda activate isce2
topsApp.py -h
```
This should give you an introduction text of topsApp.py module and details about its parameters.
  
For more information about ISCE, please check the github page of ISCE > https://github.com/isce-framework/isce2
