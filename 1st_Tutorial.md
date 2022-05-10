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

Before we can start running this genome assembly, we need to copy Illumina reads (In this tutorial, we will try to construct a contig of unknown bacteria. Please go to this address /srv/rutaiwan/shred/tutorial/ and copy the reads to your path.
When you are ready, type the command line below to check if you have the files in your directory.

```
cp /srv/rutaiwan/shred/tutorial/test_R1.fq.gz /srv/rutaiwan/shred/tutorial/test_R2.fq.gz /path/to/your_reads
```
To check if you have successfully copy those files to your path, type a command below. You should see your test_R1.fastq.gz and test_R2.fastq.gz in your directory.

```
ls
```

Now you are ready to run Shovill. Remember to activate your shovill_env environment first before running the command to construct your contig.
This shovill is De novo assembly pipeline for Illumina paired reads. Before we run the command, let's have a look at shovill options by a command below.

```
shovill -help
```

**3. Construct your contig with Shovill**

The command above showing you all the options with Shovill. We need to input our reads (R1-forward read or Read 1 FASTQ and R2-reversed read or Read 2 FASTQ), create the name of output folder (output folder name is contigs), input the number of CPUs to use (the maximum CPUs is 64, you can choose below this number of maximum CPUs), RAM usage is set as default at 16 but we want to use more RAM (200), enable adaptor trimming by adding --trim (default: OFF), force overwrite of existing output folder (--force) and no sub-sample (--depth 0).

```
conda activate shovill_env
nice shovill --R1 test_R1.fq.gz --R2 test_R2.fq.gz --outdir contigs --cpus 1 --ram 200 --trim --force --depth 0
```

It will take time about 1 -2 mins to constructing your unknown bacterial contig. Wait until it finishes and check what directory you are by typing 'pwd' (pwd = Print Working Directory) and check if output folder named 'contigs' is created in your path?

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
cd /path/to/your_reads
conda activate shovill_env
nice shovill --R1 test_R1.fq.gz --R2 test_R2.fq.gz --outdir contigs_2 --cpus 10 --ram 200 --trim --force --depth 0
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
