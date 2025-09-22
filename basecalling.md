## sequencingTechs ###

***Basecalling***

Scripts for basecalling your pod5 files on the FIU Roary Cluster and UW Madison's OSPool. The software used is [dorado](https://github.com/nanoporetech/dorado/). The workflow for OSPool is copied from documentation provided by Daniel Morales at https://github.com/dmora127/genomics-on-ospool. 

Input is nanopore pod5 files. Output is basecalled fastq files.


<details>
<summary><b>HPC</b></summary>

<b>1. open your terminal</b>

<b>2. ssh [username]@hpclogin.fiu.edu</b>

<b>3. cd to your working directory</b>

<b>4. move the data to this directory if it isn't already there</b>

<b>5. Download dorado</b>

please navigate to https://github.com/nanoporetech/dorado/ to ensure that you are downloading the most uptodate version.

`wget https://cdn.oxfordnanoportal.com/software/analysis/dorado-1.1.1-linux-x64.tar.gz`

`gunzip dorado-1.1.1-linux-x64.tar.gz`

`tar -xvf dorado-1.1.1-linux-x64.tar`

`rm dorado-1.1.1-linux-x64.tar`

<b>6. Make and submit your script for basecalling</b>

```
vi dorado.sh
```

Hit [i] for insert and then copy/paste the following:

```
#!/bin/bash

#SBATCH --account acc_jfierst
#SBATCH --partition gpu-a100-zen2
#SBATCH --qos gpu1
#SBATCH --gres=gpu:1
#SBATCH --job-name=basecalling
#SBATCH --output=basecalling_log.%x.job_%j
#SBATCH --mail-type=ALL
#SBATCH --mail-user=[username]@fiu.edu

module load proxy

./dorado-1.1.1-linux-x64/bin/dorado basecaller hac pod5/ > [sample].bam
```

Hit [esc] and type `:wq` and then hit [enter] 

Run the job:
```
sbatch dorado.sh
```

To check if your job is running, type `squeue --me`

You should also get emails when your job starts and finishes.

Note that the output is a bam file. Bam files are a bit more difficult to understand and some programs may not take them. If you need fastq file output proceed with the next step.

<b>7. Make and submit your script if you need fastq instead of a bam</b>

Please note that I have not actually run this script on the HPC and so you may get errors, but theoretically it works.

```
vi bamTOfastq.sh
```

Hit [i] for insert and then copy/paste the following:

```
#!/bin/bash

#SBATCH --account acc_jfierst
#SBATCH --partition highmem1-sapphirerapids
#SBATCH --qos standard
#SBATCH --job-name=bamTOfastq
#SBATCH --output=bamTOfastq_log.%x.job_%j
#SBATCH --mail-type=ALL
#SBATCH --mail-user=[username]@fiu.edu

module load samtools/1.19.2-gcc-13.3.0-tveqsmv

samtools fastq [sample].bam > [sample].fastq
```

Hit [esc] and type `:wq` and then hit [enter] 

Run the job:
```
sbatch bamTOfastq.sh
```

To check if your job is running, type `squeue --me`

You should also get emails when your job starts and finishes.
  
</details>

<details>
<summary><b>HTC</b></summary>
  
</details>
