# Basic-computing-jobs-tutorial

This tutorial will get you up and running with Shovill (https://github.com/tseemann/shovill), Assemble bacterial isolate genomes from Illumina paired-end reads. In this tutorial, you will get acquaninted with the basic workflow starting from a scratch (installation and setup your environment with conda. 
After Shovill installation successfully, you will have to activate your environment in order to use your software. Perhaps you may have more questions after. Let's start setting up Shovill! 

**1. Setting up Shovill environment**
```
conda create -n shovill_env shovill --all -y
```
This command line will generate a working environment as following the name 'shovill_env' and install Shovill to your shovill_env environment.

Now you have created shovill_env environment and install Shovill. Let's check if you have done it correctly. Bear in mind that you need to enter the environment name exactly the same as what you create (if you create your environment in CAPITAL LETTER then don't forget!)

```
conda activate shovill_env 
```

If you see your path as below, you are now ready for assembling your bacterial isolate genomes with Shovill.

```
(shovill_env) USER@srvlxhpc2:~$
```
Remember to activate your environment everytime when you need to run Shovill. It is recommnended that you should create the name of your environment for easy access and not too long. 

**2. Getting the data**

Before we can start running this genome assembly, we need to create a folder to keep our Illumina reads. Hopefully, you know by now from the previous tutorial what command that we can use to create a folder? First, we'll go to your /srv directory, and select on 'username = your name' folder then use 'mkdir' to create a folder named 'illumina_reads'. 

```
cd /srv/username
mkdir illumina_reads
```
You can check if you have created this folder in your directory by using Filezilla program and go to /srv/username or you can check them on your terminal by typing a command below. You should see 'illumina_reads' folder in your directory now and go into this illumina_reads folder.

```
ls /srv/username
cd illumina_reads
``` 
Next step, we'll download Illumina reads. Remember what command we used to download something from the previous tutorial?

```
wget ////
```

To check if you have successfully copy those files to your illumina_reads folder, type a command below. You should see your test_R1.fastq.gz and test_R2.fastq.gz in the folder.

```
ls /srv/username/illumina_reads
```

Now you are ready to run Shovill. Remember to activate your shovill_env environment first before running the command to construct your contig.
This shovill is De novo assembly pipeline for Illumina paired reads. Before we run the command, let's have a look at shovill options by a command below.

```
shovill --help
```

**3. Construct your contig with Shovill**

The command above showing you all the options with Shovill. First, we need to activate your shovill_env environment.

```
conda activate shovill_env
```
Let's go to your illumina_reads folder by typing a command below. 

```
cd /srv/username/illumina_reads
```
Now we'll use shovill to construct a contig. The command line below asking you to input reads and create output folder name where you want to locate, cpus is refered to the number of CPUs to use for running this shovill software. Notice: Here we also input 'nice' in front of our command to adjust or lower prioritize our job which affect to process scheduling.

```
nice shovill --R1 test_R1.fq.gz --R2 test_R2.fq.gz --outdir contigs --cpus 1 --ram 3.5 --trim 
```
After running that command line, do you notice any error? Shovill should report an error due to the minimum requirement of memory for this software is at least 2GB but this pipeline is intelligent enough to detect how many RAM required for your process. Therefore, it reports you this error. Next, we will try to increase --ram and let it do the job. Write down the finish time and compare !!

```
nice shovill --R1 test_R1.fq.gz --R2 test_R2.fq.gz --outdir contigs --cpus 1 --ram 4 --trim 
```
Did you notice any report of time when Shovill finish? It should report you 'walltime used: xx min xx sec'. After finish, check what directory you are by typing 'pwd' (pwd = Print Working Directory) and check if output folder named 'contigs' is created?

```
pwd
```
Your working directory should show up in your terminal and now we will check if you have your contigs in the output folder named 'contigs'. Type the command below to see files and folders in your path directory.

```
ls
```
You can check if your job has been completely finished by running a command below. Now it should show on your terminal that no such job submitted, shovill is already finished. Or you can run shovill again and open a new terminal window and type a command below to see what is happening.    

```
htop -u $USER
```

This htop command in linux system allows  you to interactively monitor the server's process in real time. If you type your username after '-u', it will show only your process. Type only 'htop' again to see the difference. If you want to return to your base screen, press 'F10'.

```
htop
```

**4. Running your analysis in SCREEN**

Sometimes you may experience a long-running process and you may not want to leave your computer on for days or weeks only for running your analysis. It is recommended that you submit your analysis to the server with a command below and leave it running alone until finish. This also allows you to resume the sessions at some points whenever you want.  

```
screen
```

Press 'Enter' again to the terminal. You will now enter to your terminal and ready to activate your conda environment with a command below.

```
source .bash_profile
```

This time we will run Shovill with thread 10 (change --cpus 1 to --cpus 10). You will use more thread than before to construct your contig with Shovill and expect to the job to finish about 20 seconds. However, it depends on how busy of the server as other users may use the server at the same time. DO NOT FORGET TO ACTIVATE YOUR SHOVILL ENVIRONMENT. Notice: --outdir contigs_2 and --cpus 10 !!

```
cd /srv/username/illumina_reads
conda activate shovill_env
nice shovill --R1 test_R1.fq.gz --R2 test_R2.fq.gz --outdir contigs_2 --cpus 10 --ram 200 --trim 
```
Now you can close your terminal window. Do not be panic about losing your job when closing your terminal. You have input your analysis in SCREEN mode, remember? You can resume to your session anytime you wish. Enter your terminal again and run a command below.

```
screen -list
```
The messages below show you how many screen you detached. 
```
There is a screen on:
        1307374.pts-0.srvlxhpc2 (04/25/22 10:43:11)     (Detached)
1 Socket in /run/screen/S-$USER.
```

This command will show you how many screen you input. You can resume to the session that you previously run your shovill before. Session name will be different for every users so you need to enter the one you see on your screen. 

```
screen -r $session_name
```

Now you reattach to your previous analysis. When the analysis is ended, you should deactivate your shovill_env environment before leaving the terminal. If you want to exit the screen mode, just type 'exit'. 

```
conda deactivate
exit
```
Your screen mode is terminated and you are now returned to the base environment. You can exit your terminal now or stay and do another practice for analysis !!!
