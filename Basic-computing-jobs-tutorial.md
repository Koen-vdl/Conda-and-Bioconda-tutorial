# Basic-computing-jobs-tutorial

This tutorial will get you up and running with BEAST2 (Bayesian evolutionary analysis by sampling trees), a free software package for Bayesian evolutionary analysis of molecular sequences (https://www.beast2.org/). In this tutorial, you will get acquaninted with the basic workflow starting from a scratch (installation and setup your environment with conda) and visualization your results at the end of the tutorial. However, you can also install this software as a graphical user interface tool on your computer as BEAST2 is Java program. After BEAST2 installation successfully, you will have BEAST2 and BEAUTi2 (used for generating BEAST2 XML configuration files). At the end, you may need another program to visualize your time-tree, for example, FigTree but we will not go into details to interpret your tree because our objective for this tutorial is to help you get familiar yourself with bioinformatics tools. Perhaps you may have more questions after. Let's start setting up BEAST2! 

**1. Setting up BEAST2 environment**
```
conda create -n beast2_env beast2
```
This command line will generate a working environment as following the name 'beast2_env' and install Beast2 to your beast2_env environment.

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
Beast2 is running and let's do 'CTRL+Z' to see what happens to the job. You will see after you press CTRL+Z, your job is stopped on the way, CTRL+Z will submit your job to the background. Let's check if there is anything running in your background by typing 'bg' in your terminal.

```
bg
```
Now it will return to your screen, beast2 is running and wait until it finishes.   

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

Ready to have a look at your tree? If you want to run figtree, you need to run a command below. FigTree window will be popped up then click on 'File' and 'Open' to select your ouput file (primate-mtDNA.tree). Finally, you can visualise your tree !! 

```
figtree
```
After finish viewing your tree, you can now deactivate your beast2_env environment, it will return to your base environment.

```
conda deactivate
```


**5. Running your analysis in SCREEN**

Sometimes you may experience a long-running process and you may not want to leave your computer on for days or weeks only for running your analysis. It is recommended that you submit your analysis to the server with a command below and leave it running alone until finish. This also allows you to resume the sessions at some points whenever you want.  

```
screen
```

Press 'Enter' again to the terminal. You will now enter to your terminal and ready to activate your conda environment with a command below.

```
source .bash_profile
```

This time we will run an analysis with thread 3. You will use more thread than before to run your analysis and expect to the job to finish earlier than before. However, it depends on how busy of the server as other users may use the server at the same time. Please go to the folder that contains your xml file before first and DO NOT FORGET TO ACTIVATE YOUR BEAST2 ENVIRONMENT. 

```
cd beast2
conda activate beast2_env
nice beast -threads 3 Primates.xml
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


This command will show you how many screen you input. You can resume to the session that you previously run your Beast analysis before. Session name will be different for every users so you need to enter the one you see on your screen. 

```
screen -r $session_name
```

Now you reattach to your previous analysis. When the analysis is ended, you should deactivate your conda environment before leaving the terminal. If you want to exit the screen mode, just type 'exit'. 

```
conda deactivate
exit
```
Your screen mode is terminated and you are now returned to the base environment. You can exit your terminal now or stay and do another practice for analysis !!!
























