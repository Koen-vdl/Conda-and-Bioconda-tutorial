# Automating tasks with loops and Bash scripts

The Bash shell is one of the most compelling reasons to use Linux. Users can interact with Bash through the command line, and write scripts to automate tasks. Although this may sound intimidating to beginning users, it is not hard to get started with Bash scripting. In this tutorial, we will take you through an example of a Bash script to show you what it is capable of.

In this tutorial you will learn:

1. How to write your first Bash script
2. How to use variables in Bash
3. How to use for loops in Bash

As usual, we will use a real-world scenario. In this tutorial we will produce SARS-CoV-2 consensus sequences from Oxford Nanopore Technology (ONT) demultiplexed barcoded reads using the ARTIC analysis pipeline. While the details of the ARTIC pipeline are not important to understand the tutorial, I briefly explain the basics here. The ARTIC Midnight protocol relies on direct PCR amplification of the SARS-CoV-2 virus using tiled, multiplexed primer sets. In the wet-lab (for up to 96 cDNA samples) 29 amplicons of approx. 1200bp are produced that span the entire SARS-CoV-2 genome (approx. 30kb). The pipeline will process the ONT reads of these 29 amplicons for each sequenced virus. The pipeline will: map ONT reads to the Wuhan-Hu-1 reference sequence, filter this alignment, call variants, filter variants, mask variation discovered in low coverage regions, create a consensus sequence, calculate some metrics, and produce some plots. 

**1. Setting up software environments and copying over tutorial data**

We will start by installing the required software environment as in the previous tutorials. We will however illustrate yet another installation approach by "recreating" an environment from a *file* which contains a list of packages required in the environment. Let's recreate Koen's artic-ncov2019 environment by running:

```
conda create -n artic-ncov2019_env --file /srv/koen_vdl/Into_To_Comp_Bio_Tutorial_Files/artic-ncov2019_env_spec-file.txt -y
```

Tip: this installation approach represents a convenient way to share a custom environment with collaborators. Simply activate the environment you wish to share, run `conda list --explicit > spec-file.txt`, and share the `spec-file.txt` with your collaborator.

We also need to copy over some data for the tutorial. The `GridION_SCV2_MN_fastq_pass` folder contains 5 demultiplexed barcoded ONT read-sets straight off the GridION. While in a normal run The GridION can sequence up to 96 samples, we limit the tutorial to 5 samples. The `ARTIC_primer_schemes` folder contains the tiled, multiplexed primer scheme of the ARTIC Midnight protocol. 

```
mkdir /srv/$USER/tutorial_intro_to_comp_biology
cd /srv/$USER/tutorial_intro_to_comp_biology
cp -r /srv/koen_vdl/Into_To_Comp_Bio_Tutorial_Files/GridION_SCV2_MN_fastq_pass/ GridION_SCV2_MN_fastq_pass
cp -r /srv/koen_vdl/Into_To_Comp_Bio_Tutorial_Files/ARTIC_primer_schemes/ ARTIC_primer_schemes
```

Take a look at the GridION data folder with 

```
ls -R /srv/$USER/tutorial_intro_to_comp_biology/GridION_SCV2_MN_fastq_pass/
```

This will print the contents of the `GridION_SCV2_MN_fastq_pass` folder recursively `-R`. Alternatively, you can check the contents of the folder by navigating to it in your `FileZilla` ftp-client. Notice each barcode folder contains a number gzipped fastq files. By default the GridION writes 1000 reads per fastq file.


**2. Running the ARTIC pipeline the hard way**

Activate the conda `artic-ncov2019` environment with:

```
conda activate artic-ncov2019_env
```

You could take a quick look at the `artic` basic documentation with:

```
artic --help
```

In the tutorial we will limit ourselves to the `artic` subcommands `artic guppyplex` and `artic minion`. `artic guppyplex` will aggregate pre-demultiplexed reads from MinKNOW/Guppy while `artic minion` will run the alignment/variant-call/consensus pipeline. You could take a quick look at the basic documentation of the subcommands with:

```
artic guppyplex --help
artic minion --help
```

Let's illustrate now how tedious it is to run `artic` analyses manually. Let's create a folder for the output of `barcode01`, navigate to that folder and run `artic guppyplex` and `artic minion`.

```
mkdir -p /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/barcode01
cd /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/barcode01
artic guppyplex --skip-quality-check --min-length 200 --max-length 1100 --directory ../../GridION_SCV2_MN_fastq_pass/barcode01 --output barcode01_processed.fastq
artic minion --strict --normalise 200 --medaka --medaka-model r941_min_high_g360 --threads 10 --scheme-directory /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_primer_schemes/ --read-file barcode01_processed.fastq nCoV-2019/V1200 barcode01
```

Notice the `artic minion` pipeline produced 31 files for `barcode01`.

```
ls -l
```

One of the files the pipeline produced is `barcode01.consensus.fasta`, the consensus sequence we are after. Have a look at the file using `less`. To scroll down/up in `less` use the `PgDn`/`PgUp`key. To quit `less` press the `q` key.

```
less barcode01.consensus.fasta
```

Let's repeat these steps for `barcode02`. Notice the below commands are exactly the same ones we ran before: we just replaced `1` with `2` for `barcode01` and `barcode02` respectively.

```
mkdir -p /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/barcode02
cd /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/barcode02
artic guppyplex --skip-quality-check --min-length 200 --max-length 1100 --directory ../../GridION_SCV2_MN_fastq_pass/barcode02 --output barcode02_processed.fastq
artic minion --strict --normalise 200 --medaka --medaka-model r941_min_high_g360 --threads 10 --scheme-directory /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_primer_schemes/ --read-file barcode02_processed.fastq nCoV-2019/V1200 barcode02
ls -l
```

In this tutorial you can just copy & paste my commands but imagine having to manually enter and tweak these commands 96 times for every barcode after every sequencing run. This is clearly not the way. Let's get rid of the analyses we did for barcode01 and barcode02 and learn how to automate in the next section!

```
rm -r /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/barcode01
rm -r /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/barcode02
cd /srv/$USER/tutorial_intro_to_comp_biology
```

**2. Learning about variables and for loops**

Remember how the manual `artic` commands we ran before only differed by `1` and `2` for `barcode01` and `barcode02` respectively? We will use this to our advantage when automating the analysis pipeline. We will treat `1`, `2`, `3`, `4`, and `5` as a *variable*. A variable is a character string to which we assign a value. The value assigned could be a number, text, filename, or any other type of data. The name of a variable can contain only letters (a to z or A to Z), numbers (0 to 9) or the underscore character (_).You cannot use other characters such as `!`, `*`, or `-` as these characters have a special meaning for the shell. The shell enables you to create, assign, and delete variables.

You can define a variable `KING` as follows:

```
KING=Jayavarman_VII
```

The above example defines the variable KING and assigns the value `Jayavarman_VII` to it. Variables of this type are called *scalar* variables. A scalar variable can hold only one value at a time. To access the value stored in a variable, prefix its name with the dollar sign ($). To print the value of `KING` run:

```
echo $KING
```

Now we know how to define and access a variable, we need a way to execute a set of commands repeatedly. A `for` loop is a bash programming language statement which allows code to be repeatedly executed. If you already have a programming or scripting background, you're probably familiar with what for loops do. If you're not, lets break it down in plain English: `FOR` a given set of items, `DO` a thing, until you're `DONE`. Let's take a look at the below shell script: we use a variable `id` that changes over a range from 1 to 5. All the script does is print the current value of `id` to the screeb after which it waits 2 seconds with `sleep`. 

```
for id in 1 2 3 4 5
do
   echo "The variable is currently:" $id 
   sleep 2
done
```

A better way of executing scripts like the one above is to write them to a *script file* as running our commands directly in the terminal leaves no trace of what we executed. Let's produce such a script file `my_first_script.bash` using the text editor `nano`.

```
nano my_first_script.bash
```

Let's paste the script we ran above into the text editor. Exit and save `nano` with `CTRL+x`, confirm the save with `y` followed by pressing the `return` key. 

Let's now execute our script with:

```
bash my_first_script.bash
```

We see the same output is being printed to the screen. Unlike before though, we hold on to the commands we used to produce the output (in the form of the `my_first_script.bash` script).

**3. Running the ARTIC pipeline the easy way**

Now we know how to repeatedly execute code, running the 5 `artic` jobs should be easy enough. Use `nano` again create a script called `my_artic_script.bash`.

```
nano my_artic_script.bash

```
Let's paste the below script in the text editor. Exit and save with `CTRL+x` and confirm the save with `y` followed by pressing the `return` key. 
 
```
for id in 1 2 3 4 5
do
	mkdir -p /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/barcode0${id}
	cd /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/barcode0${id}

	artic guppyplex --skip-quality-check --min-length 200 --max-length 1100 --directory ../../GridION_SCV2_MN_fastq_pass/barcode0${id} --output barcode0${id}_processed.fastq

	artic minion --strict --normalise 200 --medaka --medaka-model r941_min_high_g360 --threads 10 --scheme-directory /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_primer_schemes/ --read-file barcode0${id}_processed.fastq nCoV-2019/V1200 barcode0${id}

done

#concatenate 5 consensus sequences:
cat /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/barcode*/*.consensus.fasta > /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/all_5_consensus-sequences.fasta
```

Let's now execute our script with:

```
bash my_artic_script.bash
```

Notice the last line of the script concatenates all 5 consensus sequences into a multi-fasta file. Have a look at the file using `less`. To scroll down/up in `less` use the `PgDn`/`PgUp`key. To quit `less` press the `q` key.

```
less /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/all_5_consensus-sequences.fasta
```

Hopefully, this tutorial has demonstrated the power of a for loop. You really can save a lot of time and perform tasks in a less error-prone way with loops. I hope it serves you well!


**4. Cleaning up after yourself**

You can remove files we used in this tutorial from your storage with:

```
rm -r /srv/$USER/Into_To_Comp_Bio_Tutorial_Files/GridION_SCV2_MN_fastq_pass/
rm -r /srv/$USER/Into_To_Comp_Bio_Tutorial_Files/ARTIC_primer_schemes/
rm -r /srv/$USER/tutorial_intro_to_comp_biology/ARTIC_output/
```

In case you won't be needing `artic` in the future you can delete the environment with:

```
conda deactivate
conda remove -n artic-ncov2019_env --all -y 
```

Running the below command will remove index cache, lock files, unused cache packages, and tarballs. This will free up a lot of storage space and won't affect your software environments in any way.

```
conda clean --all -y
```
