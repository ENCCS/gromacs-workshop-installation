# Gromacs workshop installation instructions

This page contains software installation instructions for Gromacs workshops. When relevant, separate instructions are given for Windows, MacOS and Linux systems.

- For the tutorials on umbrella-sampling ("pulling") simulations and applied-weight histogram methods ("AWH"), you will only need a non-MPI version of GROMACS along with Python3 and some packages. We suggest that you install these using the conda package manager. 
- We will also be using Xmgrace (a.k.a. Grace) and optionally VMD.
- The replica-exchange molecular dynamics tutorial requires an MPI version of GROMACS to run production simulations, but you are not strictly required to install it since the tutorial will be about learning the concepts. You can however find general installation instructions for MPI-GROMACS below. 
- For the tutorial "Computing trajectories efficiently on GPUs" we will use an external cluster and instructions will be provided during the workshop.
- After you have gone through the installation steps below, please download the [tutorial Jupyter notebook](tutorial.ipynb), run the notebook from terminal with `jupyter-notebook tutorial.ipynb`, and execute all cells in the notebook to make sure if you have everything installed correctly.
To download the notebook, either clone this repository (`git clone https://github.com/ENCCS/gromacs-workshop-installation.git`) or press the green "Code" button and select "Download ZIP". 


## GROMACS and Python packages

#### For Windows users

We strongly recommend to use (and install if necessary) the Windows Subsystem for Linux, WSL. Inside it you will need Python 3 and the conda Python environment manager. A useful guide to doing this is found at https://github.com/kapsakcj/win10-linux-conda-how-to. Once you have WSL and conda installed, make an environment for the GROMACS tutorials with:

```bash
conda create --name gromacs-tutorials -c conda-forge -c bioconda gromacs=2020.4 matplotlib nglview notebook numpy requests pandas seaborn
```

And activate it by:

```bash
conda activate gromacs-tutorials
```

#### For Linux users

Install miniconda for Python 3 by following 
https://docs.conda.io/en/latest/miniconda.html#linux-installers 

Once you have conda installed, make an environment for the GROMACS tutorials with:

```bash
conda create --name gromacs-tutorials -c conda-forge -c bioconda gromacs=2020.4 matplotlib nglview notebook numpy requests pandas seaborn
```

And activate it by:

```bash
conda activate gromacs-tutorials
```

#### For MacOS

Install miniconda for Python 3 by following https://docs.conda.io/en/latest/miniconda.html#macosx-installers 

Once you have conda installed, make an environment for the GROMACS tutorials with (note that 2019.1 is the highest available version for MacOS):

```bash
conda create --name gromacs-tutorials -c conda-forge -c bioconda gromacs=2019.1 matplotlib nglview notebook numpy requests pandas seaborn
```

And activate it by:

```bash
conda activate gromacs-tutorials
```

## (Optional) VMD

VMD is a powerful molecular visualization package, but it is not required for the GROMACS tutorials since `nglview` can be used instead. If you nonetheless want to install VMD, please visit
https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD to register, download and install.

## Xmgrace

Xmgrace is a WYSIWIG 2D plotting tool which will be used in some of the tutorials to plot output data. It can be installed on Linux, MacOS and WSL under Windows.

On Windows-WSL and Linux, type 

```bash
sudo apt-get install grace
```

On MacOS, follow these instructions: http://www.phy.ohio.edu/~hadizade/blog_files/XmGrace_Mac.html

## (Optional) MPI-enabled GROMACS

The tutorial on replica-exchange MD simulations requires MPI-based GROMACS. Since we will not be running production MD simulations during the tutorial it is not necessary to install an MPI version, and we will be providing reference output files for non-MPI GROMACS as a backup. 
However, if you want to install MPI-based GROMACS on your own computer we provide here a general installation based on conda which should work on MacOS, Linux and WSL under Windows. If you already have compilers and an MPI library installed on your computer, you can skip the first section below or refer to the official installation instructions (https://manual.gromacs.org/documentation/current/install-guide/index.html). 

Install compilers, OpenMPI and CMake:

```bash
conda create --name mpi
conda activate mpi
conda install -c conda-forge compilers
conda install -c conda-forge openmpi
conda install -c conda-forge cmake
```

Download GROMACS 2020.4:

```bash
wget http://ftp.gromacs.org/pub/gromacs/gromacs-2020.4.tar.gz 
cd gromacs-2020.4
(or git clone https://gitlab.com/gromacs/gromacs.git; cd gromacs; git checkout v2020.4)
```

Compile GROMACS:

```bash
mkdir build
cd build
cmake .. -DCMAKE_C_COMPILER=mpicc -DCMAKE_CXX_COMPILER=mpicxx   -DGMX_MPI=ON -DGMX_DOUBLE=OFF  -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON
make check
sudo make install
source /usr/local/gromacs/bin/GMXRC
```

You will now hopefully be able to run gmx_mpi!

