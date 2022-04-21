# Conda and Bioconda tutorial

This tutorial will get you up and running with Conda and Bioconda.


### Today we are going to:

1. Install Conda via the Miniconda distribution
2. Learn about activation of the base Conda environment
3. Configure Conda with Software "Channels"
4. Learn how to:
    * install tools using Conda
    * remove tools
    * install a particular version of a tool
    * update tools
5. Learn how to handle version conflicts
    * Conda environments
6. Learn how to use Conda environments
    * Create an environment
    * Add tools to an environment
    * Activate different environments

**Instructions:**
1. Read through the following
2. Run the commands that start with `>$` in your SSH session.

For example, the command `pwd` will be shown as:
```
>$ pwd
```
or
```
(base) >$ pwd
```

Don't type in the `>$` or the `(base) >$`! It's just the representation of the prompt in the tutorial document.

## 1. Install Conda *via* the Miniconda distribution.

First thing we have to do is download the installer from the Miniconda documentation website.

SSH Login to your Linux server

Then from your HOME directory run:

```
>$ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

After the download of the installer finished we can in run it.
We will use the `-b` and `-s` switches for *silent mode* and *no post install scripts* respectively.

```
>$ sh Miniconda3-latest-Linux-x86_64.sh -b -s
```

We can watch it install and print the following to the screen:

```
PREFIX=/home/test/miniconda3
Unpacking payload ...
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /home/test/miniconda3

  added / updated specs:
    - _libgcc_mutex==0.1=main
    - _openmp_mutex==4.5=1_gnu
    - brotlipy==0.7.0=py39h27cfd23_1003
    - ca-certificates==2021.10.26=h06a4308_2
    - certifi==2021.10.8=py39h06a4308_2
    - cffi==1.15.0=py39hd667e15_1
    - charset-normalizer==2.0.4=pyhd3eb1b0_0
    - conda-content-trust==0.1.1=pyhd3eb1b0_0
    - conda-package-handling==1.7.3=py39h27cfd23_1
    - conda==4.11.0=py39h06a4308_0
    - cryptography==36.0.0=py39h9ce1e76_0
    - idna==3.3=pyhd3eb1b0_0
    - ld_impl_linux-64==2.35.1=h7274673_9
    - libffi==3.3=he6710b0_2
    - libgcc-ng==9.3.0=h5101ec6_17
    - libgomp==9.3.0=h5101ec6_17
    - libstdcxx-ng==9.3.0=hd4cf53a_17
    - ncurses==6.3=h7f8727e_2
    - openssl==1.1.1m=h7f8727e_0
    - pip==21.2.4=py39h06a4308_0
    - pycosat==0.6.3=py39h27cfd23_0
    - pycparser==2.21=pyhd3eb1b0_0
    - pyopenssl==21.0.0=pyhd3eb1b0_1
    - pysocks==1.7.1=py39h06a4308_0
    - python==3.9.7=h12debd9_1
    - readline==8.1.2=h7f8727e_1
    - requests==2.27.1=pyhd3eb1b0_0
    - ruamel_yaml==0.15.100=py39h27cfd23_0
    - setuptools==58.0.4=py39h06a4308_0
    - six==1.16.0=pyhd3eb1b0_0
    - sqlite==3.37.0=hc218d9a_0
    - tk==8.6.11=h1ccaba5_0
    - tqdm==4.62.3=pyhd3eb1b0_1
    - tzdata==2021e=hda174b7_0
    - urllib3==1.26.7=pyhd3eb1b0_0
    - wheel==0.37.1=pyhd3eb1b0_0
    - xz==5.2.5=h7b6447c_0
    - yaml==0.2.5=h7b6447c_0
    - zlib==1.2.11=h7f8727e_4


The following NEW packages will be INSTALLED:

  _libgcc_mutex      pkgs/main/linux-64::_libgcc_mutex-0.1-main
  _openmp_mutex      pkgs/main/linux-64::_openmp_mutex-4.5-1_gnu
  brotlipy           pkgs/main/linux-64::brotlipy-0.7.0-py39h27cfd23_1003
  ca-certificates    pkgs/main/linux-64::ca-certificates-2021.10.26-h06a4308_2
  certifi            pkgs/main/linux-64::certifi-2021.10.8-py39h06a4308_2
  cffi               pkgs/main/linux-64::cffi-1.15.0-py39hd667e15_1
  charset-normalizer pkgs/main/noarch::charset-normalizer-2.0.4-pyhd3eb1b0_0
  conda              pkgs/main/linux-64::conda-4.11.0-py39h06a4308_0
  conda-content-tru~ pkgs/main/noarch::conda-content-trust-0.1.1-pyhd3eb1b0_0
  conda-package-han~ pkgs/main/linux-64::conda-package-handling-1.7.3-py39h27cfd23_1
  cryptography       pkgs/main/linux-64::cryptography-36.0.0-py39h9ce1e76_0
  idna               pkgs/main/noarch::idna-3.3-pyhd3eb1b0_0
  ld_impl_linux-64   pkgs/main/linux-64::ld_impl_linux-64-2.35.1-h7274673_9
  libffi             pkgs/main/linux-64::libffi-3.3-he6710b0_2
  libgcc-ng          pkgs/main/linux-64::libgcc-ng-9.3.0-h5101ec6_17
  libgomp            pkgs/main/linux-64::libgomp-9.3.0-h5101ec6_17
  libstdcxx-ng       pkgs/main/linux-64::libstdcxx-ng-9.3.0-hd4cf53a_17
  ncurses            pkgs/main/linux-64::ncurses-6.3-h7f8727e_2
  openssl            pkgs/main/linux-64::openssl-1.1.1m-h7f8727e_0
  pip                pkgs/main/linux-64::pip-21.2.4-py39h06a4308_0
  pycosat            pkgs/main/linux-64::pycosat-0.6.3-py39h27cfd23_0
  pycparser          pkgs/main/noarch::pycparser-2.21-pyhd3eb1b0_0
  pyopenssl          pkgs/main/noarch::pyopenssl-21.0.0-pyhd3eb1b0_1
  pysocks            pkgs/main/linux-64::pysocks-1.7.1-py39h06a4308_0
  python             pkgs/main/linux-64::python-3.9.7-h12debd9_1
  readline           pkgs/main/linux-64::readline-8.1.2-h7f8727e_1
  requests           pkgs/main/noarch::requests-2.27.1-pyhd3eb1b0_0
  ruamel_yaml        pkgs/main/linux-64::ruamel_yaml-0.15.100-py39h27cfd23_0
  setuptools         pkgs/main/linux-64::setuptools-58.0.4-py39h06a4308_0
  six                pkgs/main/noarch::six-1.16.0-pyhd3eb1b0_0
  sqlite             pkgs/main/linux-64::sqlite-3.37.0-hc218d9a_0
  tk                 pkgs/main/linux-64::tk-8.6.11-h1ccaba5_0
  tqdm               pkgs/main/noarch::tqdm-4.62.3-pyhd3eb1b0_1
  tzdata             pkgs/main/noarch::tzdata-2021e-hda174b7_0
  urllib3            pkgs/main/noarch::urllib3-1.26.7-pyhd3eb1b0_0
  wheel              pkgs/main/noarch::wheel-0.37.1-pyhd3eb1b0_0
  xz                 pkgs/main/linux-64::xz-5.2.5-h7b6447c_0
  yaml               pkgs/main/linux-64::yaml-0.2.5-h7b6447c_0
  zlib               pkgs/main/linux-64::zlib-1.2.11-h7f8727e_4


Preparing transaction: done
Executing transaction: done
installation finished.
```

We can remove the installer with:

```
>$ rm Miniconda3-latest-Linux-x86_64.sh
```

## 2. Activate the Conda base environment

Now that we have installed Conda we need to be able to use it.

In a very similar way to activating a Python virtual environment, we need to ***activate*** Conda before we can use it.

We do this by pointing the `source` command at the Conda activate script.

**NOTE: You need to do this EVERY time you login to your server if you want to use tools you installed with Conda.**

```
>$ source ~/miniconda3/bin/activate
```

You'll notice that your command line now has an added `(base)` in front of it. This lets you know that Conda has been activated and you can use it. Later on, when we start using other conda environments it will tell us which one we are in!

In case you want to automatically activate conda whenever you log into your server: you can add the activation line to your .bash_profile file with:

```
>$ echo 'source ~/miniconda3/bin/activate' >> ~/.bash_profile    
```
Unintuitively, the Conda installer did not install the latest version. Start by updating Conda by running:

```
conda update -n base -c defaults conda
```

## 3. Configure Conda with software "Channels"

Now we want to configure Conda with some extra software channels. We especially want to tell Conda where it can find all those awesome Bioinformatics tools we really want to use are.

So we need to add three "Channels" to conda's configuration. They are:

* defaults - base packages for Conda
* bioconda - >7000 Bioinformatics tools and growing daily
* conda-forge - has most of the dependencies for all our favorite tools

We add the channels with the `conda config` command. The order in which we add the channels is very important. It will determine the order in which conda will search them for the appropriate packages. It's unintuitive but the last one we add will have the highest priority. We want `conda-forge` to have the highest priority so we add it last...

```
(base) >$ conda config --add channels defaults
(base) >$ conda config --add channels bioconda
(base) >$ conda config --add channels conda-forge
```
To check that it worked you can look at the Conda configuration using:

```
(base) >$ conda config --show
```

It will print out the full configuration of your Conda install. If you see:

```
channels:
  - conda-forge
  - bioconda
  - defaults
 ```

amongst the rest of the output, you are all set!

## 4. How to:

### 4.1 Install a tool

Lets install `samtools` as an example.

To do that we use the `conda install <package-name>` command.

```
(base) >$ conda install samtools
```

Conda will work out all the things it needs to install as well as `samtools` to make sure it works.

You'll see a whole lot of stuff, but then Conda will ask you if you REALLY want to install `samtools`. Take note of all the other things it has to install.. Look closely, sometimes it may tell you it has to REMOVE things to be able to install what you want due to an incompatibility. We will look at how to get around these things later in section 5.

This is what you should see:

```
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /home/test/miniconda3

  added / updated specs:
    - samtools


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    bzip2-1.0.8                |       h7f98852_4         484 KB  conda-forge
    c-ares-1.18.1              |       h7f8727e_0         114 KB
    ca-certificates-2021.10.8  |       ha878542_0         139 KB  conda-forge
    certifi-2021.10.8          |   py39hf3d152e_2         145 KB  conda-forge
    conda-4.12.0               |   py39hf3d152e_0        1014 KB  conda-forge
    curl-7.82.0                |       h7f8727e_0          95 KB
    krb5-1.19.2                |       hac12032_0         1.2 MB
    libcurl-7.82.0             |       h0b77cf5_0         342 KB
    libedit-3.1.20210910       |       h7f8727e_0         166 KB
    libev-4.33                 |       h516909a_1         104 KB  conda-forge
    libnghttp2-1.46.0          |       hce63b2e_0         680 KB
    libssh2-1.9.0              |       h1ba5d50_1         269 KB
    python_abi-3.9             |           2_cp39           4 KB  conda-forge
    samtools-1.6               |       hb116620_7         514 KB  bioconda
    ------------------------------------------------------------
                                           Total:         5.2 MB

The following NEW packages will be INSTALLED:

  bzip2              conda-forge/linux-64::bzip2-1.0.8-h7f98852_4
  c-ares             pkgs/main/linux-64::c-ares-1.18.1-h7f8727e_0
  curl               pkgs/main/linux-64::curl-7.82.0-h7f8727e_0
  krb5               pkgs/main/linux-64::krb5-1.19.2-hac12032_0
  libcurl            pkgs/main/linux-64::libcurl-7.82.0-h0b77cf5_0
  libedit            pkgs/main/linux-64::libedit-3.1.20210910-h7f8727e_0
  libev              conda-forge/linux-64::libev-4.33-h516909a_1
  libnghttp2         pkgs/main/linux-64::libnghttp2-1.46.0-hce63b2e_0
  libssh2            pkgs/main/linux-64::libssh2-1.9.0-h1ba5d50_1
  python_abi         conda-forge/linux-64::python_abi-3.9-2_cp39
  samtools           bioconda/linux-64::samtools-1.6-hb116620_7

The following packages will be SUPERSEDED by a higher-priority channel:

  ca-certificates    pkgs/main::ca-certificates-2022.3.29-~ --> conda-forge::ca-certificates-2021.10.8-ha878542_0
  certifi            pkgs/main::certifi-2021.10.8-py39h06a~ --> conda-forge::certifi-2021.10.8-py39hf3d152e_2
  conda              pkgs/main::conda-4.12.0-py39h06a4308_0 --> conda-forge::conda-4.12.0-py39hf3d152e_0


Proceed ([y]/n)? y


Downloading and Extracting Packages
bzip2-1.0.8          | 484 KB    | ################################################################################################################################################### | 100%
certifi-2021.10.8    | 145 KB    | ################################################################################################################################################### | 100%
curl-7.82.0          | 95 KB     | ################################################################################################################################################### | 100%
libev-4.33           | 104 KB    | ################################################################################################################################################### | 100%
krb5-1.19.2          | 1.2 MB    | ################################################################################################################################################### | 100%
python_abi-3.9       | 4 KB      | ################################################################################################################################################### | 100%
ca-certificates-2021 | 139 KB    | ################################################################################################################################################### | 100%
libnghttp2-1.46.0    | 680 KB    | ################################################################################################################################################### | 100%
libedit-3.1.20210910 | 166 KB    | ################################################################################################################################################### | 100%
libssh2-1.9.0        | 269 KB    | ################################################################################################################################################### | 100%
samtools-1.6         | 514 KB    | ################################################################################################################################################### | 100%
conda-4.12.0         | 1014 KB   | ################################################################################################################################################### | 100%
c-ares-1.18.1        | 114 KB    | ################################################################################################################################################### | 100%
libcurl-7.82.0       | 342 KB    | ################################################################################################################################################### | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
```


Ok, so now we can check it out..

```
(base) >$ samtools --version

samtools 1.6
Using htslib 1.6
Copyright (C) 2017 Genome Research Ltd.
```

Sweet!

Ok so now we can install `bwa` with the following:

```
(base) >$ conda install bwa
```
Now lets check this one.

```
(base) >$ bwa

Program: bwa (alignment via Burrows-Wheeler transformation)
Version: 0.7.17-r1188
Contact: Heng Li <lh3@sanger.ac.uk>

Usage:   bwa <command> [options]

Command: index         index sequences in the FASTA format
         mem           BWA-MEM algorithm
         fastmap       identify super-maximal exact matches
         pemerge       merge overlapping paired ends (EXPERIMENTAL)
         aln           gapped/ungapped alignment
         samse         generate alignment (single ended)
         sampe         generate alignment (paired ended)
         bwasw         BWA-SW for long queries

         shm           manage indices in shared memory
         fa2pac        convert FASTA to PAC format
         pac2bwt       generate BWT from PAC
         pac2bwtgen    alternative algorithm for generating BWT
         bwtupdate     update .bwt to the new format
         bwt2sa        generate SA from BWT and Occ

Note: To use BWA, you need to first index the genome with `bwa index'.
      There are three alignment algorithms in BWA: `mem', `bwasw', and
      `aln/samse/sampe'. If you are not sure which to use, try `bwa mem'
      first. Please `man ./bwa.1' for the manual.
```

### 4.2 Remove a tool

To remove a tool is simple. Just use the `conda remove` instruction.

```
(base) >$ conda remove samtools

```

You'll have to acknowledge that you actually want to remove `samtools`.

Now try to run `samtools`..

```
(base) >$ samtools

Command 'samtools' not found, but can be installed with:

sudo apt install samtools
```
It's not there anymore. 

### 4.3 Install a particular **version** of a tool.

Last time we installed `samtools` we got version 1.6 but imagine we really wanted version 1.10 for some reason. We can install version 1.10 pretty easily. We just tell Conda what version we want when we install it.

```
(base) >$ conda install samtools==1.10
```
notice the `==1.10`? This tells Conda to install a particular version and in this case 1.10.

This is really handy sometimes.

When you install this version, notice that Conda has to re-jig some of it's other installed programs? This happens a lot and it could effect other things you might have installed. That's where conda environments come in and we'll talk about them soon.

For now run `samtools --version` again.

```
(base) >$ samtools --version

samtools 1.10
Using htslib 1.10.2
Copyright (C) 2019 Genome Research Ltd.
```
Cool.

### 4.4 Update a tool

Sometimes we want to upgrade a tool that is already installed. We can do this by using the `conda update <package-name>` command.

Lets update `samtools` to it's latest version.

```
(base) >$ conda update samtools
```

Once it's done, check it's version again.

```
(base) >$ samtools --version

samtools 1.15.1
Using htslib 1.15.1
Copyright (C) 2022 Genome Research Ltd.
```

### 4.5 Find out what tools are installed.

It's pretty easy, we just use the `conda list` command. It will tell you what is installed, which versions, build numbers and which channel it came from.

```
(base) >$ conda list
# packages in environment at /home/test/miniconda3:
#
# Name                    Version                   Build  Channel
_libgcc_mutex             0.1                 conda_forge    conda-forge
_openmp_mutex             4.5                       1_gnu    conda-forge
brotlipy                  0.7.0           py39hb9d737c_1004    conda-forge
bwa                       0.7.17               h7132678_9    bioconda
bzip2                     1.0.8                h7f98852_4    conda-forge
c-ares                    1.18.1               h7f98852_0    conda-forge
ca-certificates           2021.10.8            ha878542_0    conda-forge
certifi                   2021.10.8        py39hf3d152e_2    conda-forge
cffi                      1.15.0           py39h4bc2ebd_0    conda-forge
charset-normalizer        2.0.12             pyhd8ed1ab_0    conda-forge
colorama                  0.4.4              pyh9f0ad1d_0    conda-forge
conda                     4.12.0           py39hf3d152e_0    conda-forge
conda-package-handling    1.8.1            py39hb9d737c_1    conda-forge
cryptography              36.0.2           py39hd97740a_1    conda-forge
htslib                    1.15.1               h9753748_0    bioconda
idna                      3.3                pyhd8ed1ab_0    conda-forge
keyutils                  1.6.1                h166bdaf_0    conda-forge
krb5                      1.19.3               h3790be6_0    conda-forge
ld_impl_linux-64          2.36.1               hea4e1c9_2    conda-forge
libcurl                   7.82.0               h7bff187_0    conda-forge
libdeflate                1.10                 h7f98852_0    conda-forge
libedit                   3.1.20191231         he28a2e2_2    conda-forge
libev                     4.33                 h516909a_1    conda-forge
libffi                    3.4.2                h7f98852_5    conda-forge
libgcc-ng                 11.2.0              h1d223b6_15    conda-forge
libgomp                   11.2.0              h1d223b6_15    conda-forge
libnghttp2                1.47.0               h727a467_0    conda-forge
libnsl                    2.0.0                h7f98852_0    conda-forge
libssh2                   1.10.0               ha56f1ee_2    conda-forge
libstdcxx-ng              11.2.0              he4da1e4_15    conda-forge
libuuid                   2.32.1            h7f98852_1000    conda-forge
libzlib                   1.2.11            h166bdaf_1014    conda-forge
ncurses                   6.3                  h27087fc_1    conda-forge
openssl                   1.1.1n               h166bdaf_0    conda-forge
perl                      5.32.1          2_h7f98852_perl5    conda-forge
pip                       22.0.4             pyhd8ed1ab_0    conda-forge
pycosat                   0.6.3           py39hb9d737c_1010    conda-forge
pycparser                 2.21               pyhd8ed1ab_0    conda-forge
pyopenssl                 22.0.0             pyhd8ed1ab_0    conda-forge
pysocks                   1.7.1            py39hf3d152e_5    conda-forge
python                    3.9.9           h62f1059_0_cpython    conda-forge
python_abi                3.9                      2_cp39    conda-forge
readline                  8.1                  h46c0cb4_0    conda-forge
requests                  2.27.1             pyhd8ed1ab_0    conda-forge
ruamel_yaml               0.15.80         py39h3811e60_1006    conda-forge
samtools                  1.15.1               h1170115_0    bioconda
setuptools                62.1.0           py39hf3d152e_0    conda-forge
six                       1.16.0             pyh6c4a22f_0    conda-forge
sqlite                    3.37.0               h9cd32fc_0    conda-forge
tk                        8.6.12               h27826a3_0    conda-forge
tqdm                      4.64.0             pyhd8ed1ab_0    conda-forge
tzdata                    2022a                h191b570_0    conda-forge
urllib3                   1.26.9             pyhd8ed1ab_0    conda-forge
wheel                     0.37.1             pyhd8ed1ab_0    conda-forge
xz                        5.2.5                h516909a_1    conda-forge
yaml                      0.2.5                h7f98852_2    conda-forge
zlib                      1.2.11            h166bdaf_1014    conda-forge
```

## 5. How to handle version conflicts

Just say you need two versions of `samtools` installed. `samtools` is used a lot as a dependency in a lot of other tools and they sometimes need particular versions. What if you want to use `tool-a` which has a dependency for `samtools 1.6` and you also want to use `tool-b` which needs `samtools 1.10`. What do you do? You can't have `samtools` 1.6 & 1.10 at the same time can you?

**Yes you can!** You just need to install `tool-a` and `tool-b` in different and separate **environments** with their own set of dependencies that do not interact with one another!

To create a Conda environment we use the `conda create` command. We will now look at how to use environments!

## 6. How to use Conda environments

Lets install a tool called `mlst` into it's own environment. It has a lot of dependencies and is quite a complex tool.

First thing we need to do is create an *environment* for it.

### 6.1 Create a Conda environment

We can create an environment for mlst as follows:

```
(base) >$ conda create -n mlst_env
```

This will create an environment space called `mlst_env` that we can now **activate** and install tools into.

Before we can use our new environment, we have to **activate** it. We use the activate command.

If we have already activated a Conda environment (including the `base` environment) we just have to use the `conda activate <environment-name>` command.

However, if we haven't activated Conda yet and we know the name of the environment we want to activate then we have to source it just like we did for the original (base) environment. `source ~/miniconda3/bin/activate <environment-name>`.

We have already got Conda activated so we switch to `mlst_env` as follows:

```
(base) >$ conda activate mlst_env
```

Immediately, you'll notice that the prompt has changed and now we have it prepended with `(mlst_env)` instead of `(base)`.

**NOTE: You can only have one environment activated at a time (for each SSH session.)**

If we run `conda list` now it should be empty..

```
(mlst_env) >$ conda list
# packages in environment at /home/ubuntu/miniconda3/envs/mlst_env:
#
# Name                    Version                   Build  Channel
```

Now that we have activated our environment, we can install tools into it.

```
(mlst_env) >$ conda install mlst
```
It will take quite a long time to install it as it has a LOT of dependencies.

But it will happen and when it's finished we can check it out.

```
(mlst_env) >$ mlst --help
SYNOPSIS
  Automatic MLST calling from assembled contigs
USAGE
  % mlst --list                                            # list known schemes
  % mlst [options] <contigs.{fasta,gbk,embl}[.gz]          # auto-detect scheme
  % mlst --scheme <scheme> <contigs.{fasta,gbk,embl}[.gz]> # force a scheme
GENERAL
  --help            This help
  --version         Print version and exit(default ON)
  --check           Just check dependencies and exit (default OFF)
  --quiet           Quiet - no stderr output (default OFF)
  --threads [N]     Number of BLAST threads (suggest GNU Parallel instead) (default '1')
  --debug           Verbose debug output to stderr (default OFF)
SCHEME
  --scheme [X]      Don't autodetect, force this scheme on all inputs (default '')
  --list            List available MLST scheme names (default OFF)
  --longlist        List allelles for all MLST schemes (default OFF)
  --exclude [X]     Ignore these schemes (comma sep. list) (default 'ecoli_2,abaumannii')
OUTPUT
  --csv             Output CSV instead of TSV (default OFF)
  --json [X]        Also write results to this file in JSON format (default '')
  --label [X]       Replace FILE with this name instead (default '')
  --nopath          Strip filename paths from FILE column (default OFF)
  --novel [X]       Save novel alleles to this FASTA file (default '')
  --legacy          Use old legacy output with allele header row (requires --scheme) (default OFF)
SCORING
  --minid [n.n]     DNA %identity of full allelle to consider 'similar' [~] (default '95')
  --mincov [n.n]    DNA %cov to report partial allele at all [?] (default '10')
  --minscore [n.n]  Minumum score out of 100 to match a scheme (when auto --scheme) (default '50')
PATHS
  --blastdb [X]     BLAST database (default '/home/gauthier/miniconda3/envs/mlst_env/db/blast/mlst.fa')
  --datadir [X]     PubMLST data (default '/home/gauthier/miniconda3/envs/mlst_env/db/pubmlst')
HOMEPAGE
  https://github.com/tseemann/mlst - Torsten Seemann
```

We can manipulate what is installed in this environment in the same manner we did for the `(base)` environment.

### 6.2 Create an environment and install tools into it at the same time

The above was a few too many steps and as computer scientists we like typing less... So...

Lets create a new environment for version 1.10 of `samtools` as well as `bwa` and install them too.

We can do it all with one command.

First we need to exit the `(mlst_env)` environment and go back to the `(base)` environment.

```
(mlst_env) >$ conda deactivate
```

Now we can create our new environment for `samtools` and install it.

```
(base) >$ conda create -n bwa_samtools_1_10 samtools==1.10 bwa
```

This command will create an environment called `bwa_samtools_1_10` and then install `samtools` version 1.10 and a compatible version of `bwa` into it.

Cool huh?

### 6.3 Remove an environment and install tools in it

You can remove an entire environment an all it's software in a single command. 

Run the following to remove the environments you created during this tutorial.

```
(base) >$ conda remove -n bwa_samtools_1_10 --all
(base) >$ conda remove -n mlst_env --all
```

## 7. Cleaning up after yourself

Running the following command will remove index cache, lock files, unused cache packages, and tarballs. 

```
(base) >$ conda clean --all -y
```

This will free up a lot of storage space and won't affect your software environments in any way.

## 8. Wiping Conda from your system

Did you mess up badly and feel like its better to reinstall Conda from scratch? Then there is always the nuclear option: exit conda completely and wipe the conda folders clean. 

**!!!WARNING!!! You will have to reinstall Conda and all your software and software environments after chosing this nuclear option.**

```
(base) >$ conda deactivate
>$ cd
>$ rm -fr ~/anaconda* ~/miniconda* ~/.conda*
```

## 9. Credit
This tutorial is based on a tutorial written up by <a href="https://github.com/Slugger70">Slugger70</a>.
