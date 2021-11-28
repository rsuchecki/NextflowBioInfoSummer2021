# bioinfosummer2021

## Hello world example

```sh
nextflow run rsuchecki/hello -revision master 
# compare
nextflow run rsuchecki/hello -revision slurm
```

Want a closer look? `$NXF_HOME/assets/rsuchecki/hello/main.nf`.

For editing and trying things out it is better to get a local copy: 

```sh
git clone --branch slurm  https://github.com/rsuchecki/hello.git 
cd hello
# view/edit (e.g. remove the  "executor 'slurm'" directive, then run
nextflow run main.nf
```

## Example workflow 

https://github.com/nathanhaigh/snakemake_template/blob/final/analysis.sh

### Data prep

```sh
git clone https://github.com/rsuchecki/nextflow-walkthrough.git:
cd nextflow-walkthrough
git checkout bis2021  #TODO - replace with semi-blank-branch
# skipping data download
#module load nextflow 
#nextflow run setup_data.nf 
```

In case of any issues with `setup_data.nf` above, 
we can copy or symlink to the data on the group drive

```sh
ln -s /group/courses01/amsi/data/
```

We can also get the local copy of the Singularity image we will be using. 

```sh
ln -s /group/courses01/amsi/singularity ./singularity-images
```

Normaly nextflow would pull the image from  the remote, 
but we want to avoid any issues with multiple concurrent pulls in the context of this workshop. 

### Instructions/steps

We will be covering the following steps, 
note that there are multiple ways to go about it
so the contents on the day may differ...

#### Input channels

1. Use a channel factory to get FASTQ files from `data/raw_reads`
2. Apply an operator to limit the number of files e.g. or `take()`
3. Use `view()` operator to display the names of files travelling through the channel. 
4. Use `set` operator to assign the channel to a variable name `ReadsForQcChannel`

Execute `nextflow run main.nf`

#### FASTQC & MULIQC

1. Add process definitions for FASTQC and MULTIQC
2. Combine them in a workflow, reading from `ReadsForQcChannel`

Execute `nextflow run main.nf -profile singularity,slurm -resume`

#### BWA_INDEX

1. (Optional) Use a channel factory to get FASTQ files from `data/raw_reads`
2. Add process definition for BWA_INDEX, 
3. Add BWA_INDEX call to workflow

Execute `nextflow run main.nf -profile singularity,slurm -resume`

#### TRIM_PE

1.  Use a channel factory to get FASTQ files as **pairs** from `data/raw_reads`
2.  (Optional) Use a channel factory to get the adapters file `data/misc/TruSeq3-PE.fa`
3.  

Execute `nextflow run main.nf -profile singularity,slurm -resume`

#### BWA_ALIGN

Execute `nextflow run main.nf -profile singularity,slurm -resume`

#### MERGE_BAMS (bonus task)

If the above tasks caused you some un-recoverable issues you can rename or delete your
`main.nf` and check-out a revision where the above steps have been captured.

```sh
mv main.nf [?].nf
git checkout ??
```



Checkpoints 

1. Input channels
2. FASTQC & MULIQC
3. BWA_INDEX
4. TRIM_PE
5. BWA_ALIGN
6. MERGE_BAMS
