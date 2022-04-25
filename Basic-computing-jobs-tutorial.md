# Basic-computing-jobs-tutorial

This tutorial will get you up and running with BEAST2 (Bayesian evolutionary analysis by sampling trees), a free software package for Bayesian evolutionary analysis of molecular sequences (https://www.beast2.org/). In this tutorial, you will get acquaninted with the basic workflow starting from a scratch (installation and setup your environment with conda) and visualization your results at the end of the tutorial. However, you can also install this software as a graphical user interface tool on your computer as BEAST2 is Java program. After BEAST2 installation successfully, you will have BEAST2 and BEAUTi2 (used for generating BEAST2 XML configuration files). At the end, you may need another program to visualize your time-tree, for example, FigTree but we will not go into details to interpret your tree because our objective for this tutorial is to help you get familiar yourself with bioinformatics tools. Perhaps you may have more questions after. Let's start setting up BEAST2! 

**1. Setting up BEAST2 environment**
```
conda create -n beast2_env beast2
```
This command line will generate a working environment as following the name 'beast2_env' and install Beast2 to your beast2_env environment.
```
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /home/gauthier/miniconda3/envs/beast2_env

  added / updated specs:
    - beast2


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    _libgcc_mutex-0.1          |      conda_forge           3 KB  conda-forge
    _openmp_mutex-4.5          |            1_gnu          22 KB  conda-forge
    beagle-lib-3.1.2           |       h9f5acd7_4         227 KB  bioconda
    beast2-2.6.3               |       hf1b8bbb_0         8.0 MB  bioconda
    expat-2.4.8                |       h27087fc_0         187 KB  conda-forge
    font-ttf-ubuntu-0.83       |       hab24e00_0         1.9 MB  conda-forge
    fontconfig-2.14.0          |       h8e229c2_0         305 KB  conda-forge
    freetype-2.10.4            |       h0708190_1         890 KB  conda-forge
    libgcc-ng-11.2.0           |      h1d223b6_15         911 KB  conda-forge
    libgomp-11.2.0             |      h1d223b6_15         432 KB  conda-forge
    libpng-1.6.37              |       h21135ba_2         306 KB  conda-forge
    libstdcxx-ng-11.2.0        |      he4da1e4_15         4.2 MB  conda-forge
    libtool-2.4.6              |    h9c3ff4c_1008         511 KB  conda-forge
    libuuid-2.32.1             |    h7f98852_1000          28 KB  conda-forge
    libxcb-1.13                |    h7f98852_1004         391 KB  conda-forge
    libzlib-1.2.11             |    h166bdaf_1014          60 KB  conda-forge
    openjdk-8.0.121            |   zulu8.20.0.5_0        47.7 MB  conda-forge
    pthread-stubs-0.4          |    h36c2ea0_1001           5 KB  conda-forge
    xorg-fixesproto-5.0        |    h7f98852_1002           9 KB  conda-forge
    xorg-inputproto-2.3.2      |    h7f98852_1002          19 KB  conda-forge
    xorg-kbproto-1.0.7         |    h7f98852_1002          27 KB  conda-forge
    xorg-libx11-1.7.2          |       h7f98852_0         941 KB  conda-forge
    xorg-libxau-1.0.9          |       h7f98852_0          13 KB  conda-forge
    xorg-libxdmcp-1.1.3        |       h7f98852_0          19 KB  conda-forge
    xorg-libxext-1.3.4         |       h7f98852_1          54 KB  conda-forge
    xorg-libxfixes-5.0.3       |    h7f98852_1004          18 KB  conda-forge
    xorg-libxi-1.7.10          |       h7f98852_0          46 KB  conda-forge
    xorg-libxtst-1.2.3         |    h7f98852_1002          31 KB  conda-forge
    xorg-recordproto-1.14.2    |    h7f98852_1002           8 KB  conda-forge
    xorg-xextproto-7.3.0       |    h7f98852_1002          28 KB  conda-forge
    xorg-xproto-7.0.31         |    h7f98852_1007          73 KB  conda-forge
    zlib-1.2.11                |    h166bdaf_1014          88 KB  conda-forge
    ------------------------------------------------------------
                                           Total:        67.3 MB

The following NEW packages will be INSTALLED:

  _libgcc_mutex      conda-forge/linux-64::_libgcc_mutex-0.1-conda_forge
  _openmp_mutex      conda-forge/linux-64::_openmp_mutex-4.5-1_gnu
  beagle-lib         bioconda/linux-64::beagle-lib-3.1.2-h9f5acd7_4
  beast2             bioconda/noarch::beast2-2.6.3-hf1b8bbb_0
  expat              conda-forge/linux-64::expat-2.4.8-h27087fc_0
  font-ttf-ubuntu    conda-forge/noarch::font-ttf-ubuntu-0.83-hab24e00_0
  fontconfig         conda-forge/linux-64::fontconfig-2.14.0-h8e229c2_0
  freetype           conda-forge/linux-64::freetype-2.10.4-h0708190_1
  libgcc-ng          conda-forge/linux-64::libgcc-ng-11.2.0-h1d223b6_15
  libgomp            conda-forge/linux-64::libgomp-11.2.0-h1d223b6_15
  libpng             conda-forge/linux-64::libpng-1.6.37-h21135ba_2
  libstdcxx-ng       conda-forge/linux-64::libstdcxx-ng-11.2.0-he4da1e4_15
  libtool            conda-forge/linux-64::libtool-2.4.6-h9c3ff4c_1008
  libuuid            conda-forge/linux-64::libuuid-2.32.1-h7f98852_1000
  libxcb             conda-forge/linux-64::libxcb-1.13-h7f98852_1004
  libzlib            conda-forge/linux-64::libzlib-1.2.11-h166bdaf_1014
  openjdk            conda-forge/linux-64::openjdk-8.0.121-zulu8.20.0.5_0
  pthread-stubs      conda-forge/linux-64::pthread-stubs-0.4-h36c2ea0_1001
  xorg-fixesproto    conda-forge/linux-64::xorg-fixesproto-5.0-h7f98852_1002
  xorg-inputproto    conda-forge/linux-64::xorg-inputproto-2.3.2-h7f98852_1002
  xorg-kbproto       conda-forge/linux-64::xorg-kbproto-1.0.7-h7f98852_1002
  xorg-libx11        conda-forge/linux-64::xorg-libx11-1.7.2-h7f98852_0
  xorg-libxau        conda-forge/linux-64::xorg-libxau-1.0.9-h7f98852_0
  xorg-libxdmcp      conda-forge/linux-64::xorg-libxdmcp-1.1.3-h7f98852_0
  xorg-libxext       conda-forge/linux-64::xorg-libxext-1.3.4-h7f98852_1
  xorg-libxfixes     conda-forge/linux-64::xorg-libxfixes-5.0.3-h7f98852_1004
  xorg-libxi         conda-forge/linux-64::xorg-libxi-1.7.10-h7f98852_0
  xorg-libxtst       conda-forge/linux-64::xorg-libxtst-1.2.3-h7f98852_1002
  xorg-recordproto   conda-forge/linux-64::xorg-recordproto-1.14.2-h7f98852_1002
  xorg-xextproto     conda-forge/linux-64::xorg-xextproto-7.3.0-h7f98852_1002
  xorg-xproto        conda-forge/linux-64::xorg-xproto-7.0.31-h7f98852_1007
  zlib               conda-forge/linux-64::zlib-1.2.11-h166bdaf_1014


Proceed ([y]/n)? y


Downloading and Extracting Packages
xorg-recordproto-1.1 | 8 KB      | ################################################################################################################################# | 100%
xorg-fixesproto-5.0  | 9 KB      | ################################################################################################################################# | 100%
xorg-libxi-1.7.10    | 46 KB     | ################################################################################################################################# | 100%
beast2-2.6.3         | 8.0 MB    | ################################################################################################################################# | 100%
expat-2.4.8          | 187 KB    | ################################################################################################################################# | 100%
xorg-xproto-7.0.31   | 73 KB     | ################################################################################################################################# | 100%
libzlib-1.2.11       | 60 KB     | ################################################################################################################################# | 100%
libstdcxx-ng-11.2.0  | 4.2 MB    | ################################################################################################################################# | 100%
xorg-libxdmcp-1.1.3  | 19 KB     | ################################################################################################################################# | 100%
xorg-libx11-1.7.2    | 941 KB    | ################################################################################################################################# | 100%
_libgcc_mutex-0.1    | 3 KB      | ################################################################################################################################# | 100%
beagle-lib-3.1.2     | 227 KB    | ################################################################################################################################# | 100%
xorg-libxfixes-5.0.3 | 18 KB     | ################################################################################################################################# | 100%
libpng-1.6.37        | 306 KB    | ################################################################################################################################# | 100%
xorg-libxtst-1.2.3   | 31 KB     | ################################################################################################################################# | 100%
xorg-inputproto-2.3. | 19 KB     | ################################################################################################################################# | 100%
libxcb-1.13          | 391 KB    | ################################################################################################################################# | 100%
font-ttf-ubuntu-0.83 | 1.9 MB    | ################################################################################################################################# | 100%
xorg-xextproto-7.3.0 | 28 KB     | ################################################################################################################################# | 100%
libgomp-11.2.0       | 432 KB    | ################################################################################################################################# | 100%
libtool-2.4.6        | 511 KB    | ################################################################################################################################# | 100%
freetype-2.10.4      | 890 KB    | ################################################################################################################################# | 100%
zlib-1.2.11          | 88 KB     | ################################################################################################################################# | 100%
fontconfig-2.14.0    | 305 KB    | ################################################################################################################################# | 100%
libgcc-ng-11.2.0     | 911 KB    | ################################################################################################################################# | 100%
xorg-libxau-1.0.9    | 13 KB     | ################################################################################################################################# | 100%
libuuid-2.32.1       | 28 KB     | ################################################################################################################################# | 100%
xorg-kbproto-1.0.7   | 27 KB     | ################################################################################################################################# | 100%
_openmp_mutex-4.5    | 22 KB     | ################################################################################################################################# | 100%
xorg-libxext-1.3.4   | 54 KB     | ################################################################################################################################# | 100%
pthread-stubs-0.4    | 5 KB      | ################################################################################################################################# | 100%
openjdk-8.0.121      | 47.7 MB   | ################################################################################################################################# | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
```

Now you have created beast2_env environment and install Beast2. Let's check if you have done it correctly. Bear in mind that you need to enter the environment name exactly the same as what you create (if you create your environment in CAPITAL LETTER then don't forget!)

```
conda activate beast2_env 
```

If you see your path as below, you are now ready for running a simple analysis with BEAST2.

```
(beast2_env) USER@srvlxhpc2:~$
```
Remember to activate your environment everytime when you need to run Beast2. It is recommnended that you should create the name of your environment for easy access and not too long. 

**2. Getting the data**

Before we can start running an analysis with BEAST2, as mentioned earlier, we need Beauti2 to generate BEAST2 XML configuration files. First of all, you need to have the input data containing DNA sequences of your interest in nexus or fasta format and import an alignment to Beauti2. This tutorial will guide you only how to run Beast2 so you can download *'Primates.xml'* file from this link; https://raw.githubusercontent.com/taming-the-beast/Introduction-to-BEAST2/master/xml/Primates.xml by running a single command line as below.

```
wget https://raw.githubusercontent.com/taming-the-beast/Introduction-to-BEAST2/master/xml/Primates.xml
```
When you are ready, type the command line below to check if you have the file in your directory.

```
ls
```

Now you are ready to run Beast2 but let's have a look what inside this xml file first.

```
less Primates.xml
```
You will notice that under the line ***data id="primate-mtDNA"***, 14 sequences and parameter settings, for example, MCMC chainlength="1000000" and storeEvery="5000", all included in .xml file. If you want to quit from viewing *'Primates.xml'* file, type 'q' (quit) then it will return to your terminal.

**3. Running an analysis with BEAST2**

You are now about to run an analysis for the first time with a single command line but before that we need to create a folder to save your output and move your xml file to this folder.

```
mkdir beast2
```
Move your *'Primates.xml'* file to beast2 folder and enter and view to your beast2 directory if your file is already there.
``` 
mv Primates.xml beast2
cd beast2
ls 
```
Now you are ready to run Beast2 with a single command line below. You will get a window popup to select your xml file by adding '&' to the end of your command line or you can run it without '&' to see what differences.

```
nice beast -threads 1 Primates.xml & 
```
Beast2 is running and let's do 'CTRL+Z' to see what happens to the job. You will see after you press CTRL+Z, your job is stopped on the way, CTRL+Z will undo your job and send it to the background. Let's check if there is anything running in your background. 

```
htop -u $USER
```
This htop command in linux system allows  you to interactively monitor the server's process in real time. If you type your username after '-u', it will show only your process. Type only 'htop' again to see the difference. If you want to return to your base screen, press 'F10'.

```
htop
```




**4. Analysing tree estimates and visualising your tree**


In order to analyse and visualise your tree, Treeannotator and Figtree are required to install in your environment. Let's check if you have both software programs in your beast2_env environment.

**Treeannotator**

```
treeannotator
```
Treeannotator window will be popped up if you successfully installed beast2 in  your environment because it is provided as part of the Beast2 package. This means you do not need to install it separately. This program is used to summarise the posterior sample of your trees to produce a maximum clade credibility tree. Select your input tree file (primate-mtDNA.trees) and output file location, click 'run' then you will have your output tree. 

**FigTree** 

You can check if you have FigTree in your environment the same way as you did before. If not, you need to install 'FigTree' for viewing your tree within beast2_env environment. You can also install the program on your base environment but it is easy for us to install in the same environment with Beast2 for visualization your tree after.

```
conda install figtree
```
```
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /home/gauthier/miniconda3/envs/beast2_env

  added / updated specs:
    - figtree


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    bzip2-1.0.8                |       h7f98852_4         484 KB  conda-forge
    ld_impl_linux-64-2.36.1    |       hea4e1c9_2         667 KB  conda-forge
    libffi-3.4.2               |       h7f98852_5          57 KB  conda-forge
    libnsl-2.0.0               |       h7f98852_0          31 KB  conda-forge
    ncurses-6.3                |       h27087fc_1        1002 KB  conda-forge
    openssl-3.0.2              |       h166bdaf_1         2.9 MB  conda-forge
    pip-22.0.4                 |     pyhd8ed1ab_0         1.5 MB  conda-forge
    python-3.10.4              |h2660328_0_cpython        28.6 MB  conda-forge
    python_abi-3.10            |          2_cp310           4 KB  conda-forge
    readline-8.1               |       h46c0cb4_0         295 KB  conda-forge
    setuptools-62.1.0          |  py310hff52083_0         1.3 MB  conda-forge
    sqlite-3.38.2              |       h4ff8645_0         1.5 MB  conda-forge
    tk-8.6.12                  |       h27826a3_0         3.3 MB  conda-forge
    tzdata-2022a               |       h191b570_0         121 KB  conda-forge
    wheel-0.37.1               |     pyhd8ed1ab_0          31 KB  conda-forge
    xz-5.2.5                   |       h516909a_1         343 KB  conda-forge
    ------------------------------------------------------------
                                           Total:        42.0 MB

The following NEW packages will be INSTALLED:

  bzip2              conda-forge/linux-64::bzip2-1.0.8-h7f98852_4
  ca-certificates    conda-forge/linux-64::ca-certificates-2021.10.8-ha878542_0
  figtree            bioconda/noarch::figtree-1.4.4-hdfd78af_1
  ld_impl_linux-64   conda-forge/linux-64::ld_impl_linux-64-2.36.1-hea4e1c9_2
  libffi             conda-forge/linux-64::libffi-3.4.2-h7f98852_5
  libnsl             conda-forge/linux-64::libnsl-2.0.0-h7f98852_0
  ncurses            conda-forge/linux-64::ncurses-6.3-h27087fc_1
  openssl            conda-forge/linux-64::openssl-3.0.2-h166bdaf_1
  pip                conda-forge/noarch::pip-22.0.4-pyhd8ed1ab_0
  python             conda-forge/linux-64::python-3.10.4-h2660328_0_cpython
  python_abi         conda-forge/linux-64::python_abi-3.10-2_cp310
  readline           conda-forge/linux-64::readline-8.1-h46c0cb4_0
  setuptools         conda-forge/linux-64::setuptools-62.1.0-py310hff52083_0
  sqlite             conda-forge/linux-64::sqlite-3.38.2-h4ff8645_0
  tk                 conda-forge/linux-64::tk-8.6.12-h27826a3_0
  tzdata             conda-forge/noarch::tzdata-2022a-h191b570_0
  wheel              conda-forge/noarch::wheel-0.37.1-pyhd8ed1ab_0
  xz                 conda-forge/linux-64::xz-5.2.5-h516909a_1


Proceed ([y]/n)? y


Downloading and Extracting Packages
ncurses-6.3          | 1002 KB   | ################################################################################################################################# | 100%
libnsl-2.0.0         | 31 KB     | ################################################################################################################################# | 100%
python-3.10.4        | 28.6 MB   | ################################################################################################################################# | 100%
sqlite-3.38.2        | 1.5 MB    | ################################################################################################################################# | 100%
wheel-0.37.1         | 31 KB     | ################################################################################################################################# | 100%
ld_impl_linux-64-2.3 | 667 KB    | ################################################################################################################################# | 100%
xz-5.2.5             | 343 KB    | ################################################################################################################################# | 100%
openssl-3.0.2        | 2.9 MB    | ################################################################################################################################# | 100%
setuptools-62.1.0    | 1.3 MB    | ################################################################################################################################# | 100%
libffi-3.4.2         | 57 KB     | ################################################################################################################################# | 100%
bzip2-1.0.8          | 484 KB    | ################################################################################################################################# | 100%
tk-8.6.12            | 3.3 MB    | ################################################################################################################################# | 100%
tzdata-2022a         | 121 KB    | ################################################################################################################################# | 100%
python_abi-3.10      | 4 KB      | ################################################################################################################################# | 100%
readline-8.1         | 295 KB    | ################################################################################################################################# | 100%
pip-22.0.4           | 1.5 MB    | ################################################################################################################################# | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
```

Ready to have a look at your tree? If you want to run figtree, you need to run a command below. FigTree windown will be popped up then click on 'File' and 'Open' to select your ouput file (primate-mtDNA.tree). Finally, you can visualise your tree !! 

```
figtree
```


**5. Running your analysis in SCREEN**

Sometimes you may experience a long-running process and you may not want to leave your computer on for days or weeks only for running your analysis. It is recommended that you submit your analysis to the server with a command below and leave it running alone until finish. This also allows you to resume the sessions at some points whenever you want.  

```
screen
```



















