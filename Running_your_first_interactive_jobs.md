# Running your first interactive jobs

In this tutorial we will cover the basics of running your first interactive jobs on the server while effectively utilizing the computational resources available. Rather than falling back on typical tutorial examples that usually amount to printing `hello world` to your screen we will use a real world example. Here we will *de novo* assemble a *Salmonella enterica subsp. enterica* serovar Paratyphi A Illumina paired-end read-set to contigs using the <a href="https://github.com/tseemann/shovill">Shovill</a> pipeline. While you might not be working on bacterial AMR genomics the basic principles we illustrate here apply to most programs that are relevant in your field. 


**1. Setting up software environments and copying over tutorial data**

We will start by installing the required software environments using `conda` as in the previous tutorial:

```
conda create -n shovill_env shovill -y
```

Notice we added the `-y` argument to the conda installation command. This will skip the confirmation step during installation. We use this approach to win time during the tutorial. I normally would suggest you not to pass the confirmation step as it always has important info regarding the installation like which dependency will be installed or downgraded.

We also need to copy over a *real* *Salmonella enterica subsp. enterica* serovar Paratyphi A paired-end read-set to play with. To save time during the tutorial we also produced artificial *fake* reads of a small fragment of the *real* Paratyphi genome. Let's first create a tutorial folder in your personal `/srv/` space and copy the `fake_R1.fq.gz`, `fake_R2.fq.gz`, `real_R1.fq.gz`, and `real_R2.fq.gz`files into it:

```
mkdir /srv/$USER/tutorial_intro_to_comp_biology
cd /srv/$USER/tutorial_intro_to_comp_biology
cp /srv/koen_vdl/Into_To_Comp_Bio_Tutorial_Files/*.fq.gz .
```

Notice we use the `$USER` environment variable here to make the tutorial work for every participant. This environment variable contains your user name. You can print the value of the environment variable to the screen with:

```
echo $USER
```

You can check if creating the folder and copying the files into it worked out with:

```
ls -l /srv/$USER/tutorial_intro_to_comp_biology
```

This will print the contents of the folder in the long listing format `-l`:

```
total 220248
-rw-rw-r-- 1 test test   9316486 May 12 23:11 fake_R1.fq.gz
-rw-rw-r-- 1 test test   9597932 May 12 23:11 fake_R2.fq.gz
-rw-rw-r-- 1 test test 187692588 May 12 23:11 real_R1.fq.gz
-rw-rw-r-- 1 test test   9316486 May 11 14:30 test_R1.fq.gz
-rw-rw-r-- 1 test test   9597932 May 11 14:30 test_R2.fq.gz

```

Alternatively, you can check the contents of the folder by navigating to it in your FileZilla sftp-client.

**2. Running your first interactive jobs**

Activate the conda `shovill` environment with:

```
conda activate shovill_env 
```

Before you run any software/pipeline, it's important to first read through the software documentation to ensure you understand the different inputs, outputs, and analysis options. Many bioinformatics programs have extensive documentation online, either through their GitHub or another website. The basic documentation for most tools can be accessed using the command-line help options which you can print to the screen with the `--help` option. You could take a quick look at the `shovill` documentation with:

```
shovill --help
```

We will *attempt* to assemble `fake_R1.fq.gz` and `fake_R2.fq.gz` to contigs using 1 CPU thread and 1 GB of RAM memory with:

```
shovill --R1 fake_R1.fq.gz --R2 fake_R2.fq.gz --outdir assembly_job_1 --force --cpus 1 --ram 1  
```

Wait, something went wrong, `shovill` is *smart* enough to realize we haven't allocated enough RAM memory to it and prints the following error to the screen: 

```
[shovill] Invalid --ram 1 - need at least 2
```

You won't always be this lucky. In most cases programs running out of memory just crash while spitting out a cryptic error message which usually mentions the word *memory*. In case you run into these errors the best course of action is taking a look at the basic documentation with the `--help` option to check if the default memory value can be manually increased. Notice the `shovill` documentation mentions the pipeline uses 16 GB of memory by default when `--ram` is not specified by the user.

Lets make another attempt to assemble `fake_R1.fq.gz` and `fake_R2.fq.gz` with 20GB of RAM:

```
shovill --R1 fake_R1.fq.gz --R2 fake_R2.fq.gz --outdir assembly_job_1  --force --cpus 1 --ram 20 
```

The job should finish to completion this time. Take note of the amount of time `shovill` took to finish by writing down the *walltime* (7'th last line printed to the screen).

Let's take a look at the `shovill` output folder:

```
ls -l assembly_job_1
```

Here we find (among other files):a .log file (containing the progress information that was printed to the screen), a .gfa file (the assembly graph) and a file containing the assembly contigs (contigs.fa):

```
total 3596
-rw-rw-r-- 1 test test 1165470 May 13 15:22 contigs.fa
-rw-rw-r-- 1 test test 1147165 May 13 15:22 contigs.gfa
-rw-rw-r-- 1 test test     730 May 13 15:22 shovill.corrections
-rw-rw-r-- 1 test test  194001 May 13 15:22 shovill.log
-rw-rw-r-- 1 test test 1160028 May 13 15:22 spades.fasta
```

Lets now allocate more memory to `shovill` by giving it 50GB of RAM:

```
shovill --R1 fake_R1.fq.gz --R2 fake_R2.fq.gz --outdir assembly_job_1  --force --cpus 1 --ram 50 
```

Take note of the new walltime. Notice any difference? Adding excessive amounts of RAM doesn't speed programs up, it only does so when you didn't give a program enough memory to begin with. In that case the program will have to *swap* enormous amounts of code and data to the hard drive (which slows things down) to free up RAM for the executing code.

As the `shovill` pipeline includes steps that can use multiple threads we will try to speed up the program by running it a third time with 10 threads. 

```
shovill --R1 fake_R1.fq.gz --R2 fake_R2.fq.gz --outdir assembly_job_1  --force --cpus 10 --ram 20 
```

Write down the walltime again to compare the time-gains made by allocating more computer threads to your job. Impressive right?

**3. Remotely Displaying GUI Applications with X11 forwarding**

*X11 forwarding* provides a lightweight solution to remotely displaying graphical user interfaces (GUIs). The system on which the GUIs are to appear must be running a X11 server. `MobaXterm` comes with an X11 server baked into it so we don't need to configure anything to display GUIs. Let's illustrate this by visualising the assembly graph of our shovill assembly using <a href="https://rrwick.github.io/Bandage/">Bandage</a>. Installing `Bandage` is a PITA as it doesn't have a `conda` recipe. You can add a version of `Bandage` that koen_vdl built from the source code to your `$PATH` with the following code (the details of this are not important now):

```
echo '# Add koen_vdl software to your $PATH' >> ~/.bash_profile
echo 'PATH=/home/koen_vdl/bin/:$PATH' >> ~/.bash_profile
source ~/.bash_profile
```

Now start `Bandage` and load the assembly graph of your `shovill` job with:

```
Bandage_Ubuntu-x86-64_v0.9.0.AppImage load assembly_job_1/contigs.gfa
```

Click on the "Draw graph button" to visualise the assembly. The *spaghetti* represents assembled contigs connected by edges that usually represent problematic repeat regions the assembler couldn't resolve.


**4. Understanding the job control commands**

Up until now we have been running `shovill` in the *foreground*: we executed the command in the terminal window and the command occupied that terminal window until it completed. This is a foreground job. During this time the terminal window was *locked*: we couldn't enter anything in the shell prompt anymore. To illustrate this lets run `shovill` once more but this time while disabling distracting progress information being printed to the screen with `> /dev/null 2>&1` (which gets rid of any messages printed to the screen). Try writing something random (like your name) in the terminal while the below command is running and hitting the *Return* key:

```
shovill --R1 fake_R1.fq.gz --R2 fake_R2.fq.gz --outdir assembly_job_1  --force --cpus 10 --ram 20 > /dev/null 2>&1
```

Notice that during the time the shovill job was running your terminal was *locked*. Your next command (your name) was only executed after the shovill job finished!

To prevent locking up your terminal you can run a job in the *background* by entering an ampersand `&` symbol at the end of the command. This runs the command without occupying or locking the terminal window. The shell prompt is displayed immediately after you press the *Return* key.


To illustrate this lets run `shovill` once more but this time in the background:

```
shovill --R1 fake_R1.fq.gz --R2 fake_R2.fq.gz --outdir assembly_job_1 --force --cpus 10 --ram 20 > /dev/null 2>&1 &
```

Kinda nice not to lock up your terminal anymore right?

We can use what we just learned to run multiple jobs simultaneously. Let's execute the `shovill` job 2 times simultaneously in the background. We will follow up all jobs running in the background with the command `jobs`. 

```
shovill --R1 fake_R1.fq.gz --R2 fake_R2.fq.gz --outdir assembly_job_1  --force --cpus 10 --ram 20 > /dev/null 2>&1 &
shovill --R1 fake_R1.fq.gz --R2 fake_R2.fq.gz --outdir assembly_job_2  --force --cpus 10 --ram 20 > /dev/null 2>&1 &
jobs

```

Pretty sweet right? Let's now imagine the shovill job would take 24h to finish and we just started it with a wrong option! Luckily we can stop a running job with the key combination `CTRL + C`. Try that now with the below command: run it and kill immediately it with `CTRL + C`.

```
shovill --R1 fake_R1.fq.gz --R2 fake_R2.fq.gz --outdir assembly_job_1  --force --cpus 10 --ram 20 > /dev/null 2>&1
```

**5. Monitoring your jobs**

Once you have an analysis running, it's important to monitor your software to determine whether it is effectively utilizing the computational resources you have allocated to it. Understanding what resources your pipeline utilizes can help you scale up/down your compute so that you are not wasting resources or hitting resource limits that may slow down your pipeline. Many bioinformatic pipelines are “bursty” in nature, meaning different steps in a single pipeline may have vastly different computing requirements. Some steps/tools may have high memory requirements but only utilize a small number of threads, while others may multithread quite well across a large number of threads but require minimal memory.

A great way to monitor the computational resources your jobs use are simple programs like <a href="https://hisham.hm/htop/">htop</a> that allow fast real-time monitoring of basic metrics like CPU thread and RAM usage using graphs made of Unicode characters.

Take a look at `htop` now: you might spot colleagues running their `shovill` jobs. 
The program shows a frequently updated list of all the processes currently running on the server, normally ordered by the amount of CPU usage.

```
htop
```
`htop` is interactive via mouse (!) and keyboard. Try sorting on memory usage by clicking on the `MEM%` column header. 

Press the `q` key or `F10` to quit htop.

You can inspect your own running jobs with:

```
htop -u $USER
```

Notice the PR (Priority) and NI (Niceness) columns in `htop`. A nice value is user-specified and determines a job's priority. Why worry about the priority of your processes? Some of your long CPU-intensive jobs may be not as important as those of other users. You can be *nice* by adding `nice` in front of all your non-important jobs to give them lower priority. 


To run the `shovill` assembly job again in *low-priority mode* using `nice` you would run it like:

```
nice shovill --R1 fake_R1.fq.gz --R2 fake_R2.fq.gz --outdir assembly_job_2  --force --cpus 10 --ram 20 > /dev/null 2>&1 &
```

**6. Running your analysis in SCREEN**

A loss of connection or closing of the terminal unintentionally terminates the process you were working on. This can be disastrous when you are in the middle of a long job or when you were moving or coping large files. The `screen` command is very useful to avoid such situations.  Once the terminal is activated using `screen` anything in that terminal stays running even if you log out. This is a perfect solution for running long jobs on the server. 

Let's configure `screen` first by running the following two cryptic commands (a single time) to: instruct `screen` to always treat your shell as a login shell and not display a credit page when using the software (the details of this are not important now).

```
echo 'shell -$SHELL' >> ~/.screenrc
echo 'startup_message off' >> ~/.screenrc
```

Start your first *screen session* with the name 'tutorial' with:

```
screen -S tutorial
```

Notice that when you start a screen session it's like you just opened a new terminal window so you're back in your base conda environment: you will need to activate the `shovill` environment again with:

```
conda activate shovill_env 
```

You can check your `screen` sessions with:

```
screen -list
```

This will print a list of all your screen sessions to the screen. You should only see one session to which you are currently *attached* to named -------.tutorial

```
There is a screen on:
        1893068.tutorial        (05/12/2022 10:34:36 PM)        (Attached)
1 Socket in /run/screen/S-test.
```

You must be tired running the same `shovill` job again but let's run it one last time with the *real* data. Notice the real read-set is about ten times the size of the fake set so this job will take longer to finish.

```
shovill --R1 real_R1.fq.gz --R2 real_R2.fq.gz --outdir assembly_job_3  --force --cpus 10 --ram 20 &
```

This is job should take about 6 minutes. You want shut down your laptop and go back to your office now right? Well just press the key combination `CTRL + A` followed by `CTRL + D`. This will detach you from your screen (your job remains running). Now go back to your office and check on your job in 10 min or so by powering on your laptop, opening a new terminal window and *re-attaching* your screen with:

```
screen -r
```

When your job finishes you can kill your screen session with `exit`

```
exit
```

When you now check on your screen sessions there should be non listed:

```
screen -list
```

**7. Cleaning up after yourself**

Running the following command will remove index cache, lock files, unused cache packages, and tarballs. This will free up a lot of storage space and won't affect your software environments in any way.

```
conda clean --all -y
```

In case you won't be needing `shovill` in the future you can delete the `shovill` environment with:

```
conda remove -n shovill_env --all -y 
```

