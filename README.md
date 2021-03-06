 ![](images/logo.png#crop=0&crop=0&crop=1&crop=1&id=Oiact&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=700) 

annotation_smk is a simple process of genome structure annotation, which can be parallelized and automated. 

 ![](images/pipeline.png#crop=0&crop=0&crop=1&crop=1&id=ayEia&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=700) 

## Dependencies

- [conda](https://conda.io/en/latest/index.html) 

Conda can be downloaded as part of the [Anaconda](https://www.anaconda.com/) or the [Miniconda](https://conda.io/en/latest/miniconda.html) plattforms (Python 3.7). 

## Install

```bash
$ git clone https://github.com/yanhui-k/annotation_smk.git
```

## Getting Started

You can use the accompanying `config/environment.yaml` to creat a general conda envirment and use `config/requirements.txt` to download the necessary python packages.

```bash
$ mamba env create -f config/environment.yaml
$ pip install -r config/requirements.txt
```

This will create a conda environment containing all dependencies for Snakemake itself.

Activate the environment

```bash
$ conda activate annotation
```

## Tutorial

### Prerequisites: dataset

The FASTQ files of RNA-seq need to follow a specific naming convention:

 ![](images/fastq.png#crop=0&crop=0&crop=1&crop=1&id=JMq0R&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=) 

For example, under the folder `/tora`

```
samplename1_1.fastq.gz  
samplename1_2.fastq.gz
samplename2_subreads.fastq.gz
```

Note: All RNA-Seq data should be stored in a folder named base name (freely named, and will be used in the command).

The FASTA of protein evidence is also required to improve annotation accuracy. You can download homologous protein sequence from the website [uniprot](https://www.uniprot.org/), and merge them into a FASTA format file and provide it to annotation_smk.

### Running annotation with the "annotation_smk.sh" script

Frist, you need to add executable permission to the "annotation_smk.sh" script and activate the snakemake environment:

```bash
$ chmod +x annotation_smk.sh
$ conda activate annotation
```

Then you can start the pipeline by typing in the following commands into your terminal emulator:

```bash
$ ./annotation.sh -c <core> -b <base> -g <genome> -p <protein>
```

For example, gene annotation for _tora/tora.fa_, there is homologous protein evidence _arath_med_sprot.pep_ and RNA-Seq evidence stored in the _tora_ folder, if you want to use 10 cores, submit it to the cluster with bsub for execution, you can use the following ??order:

```bash
$ ./annotation.sh -c 10 -b tora -g tora/tora.fa -p tora/arath_med_sprot.pep
```

### Running annotation_smk manuall

You have to start by activating the snakemake environment:

```bash
$ conda activate annotation
```

Use the following code to modify the path of the input file 

```bash
$ vim config/config.yaml
```

Modify the value of PREFIX to the base name, modify the value of REF to the indirect path of the genome file, modify the value of PEP to the indirect path of the homologous protein evidence, and then save the file.

To start the main pipeline, type in: 

```bash
$ nohup snakemake --cluster "bsub -o output -e error -q Q104C512G_X4 -m yi02" -j 10 -p --use-conda &
```

If you want to terminate a running pipline, please type in: 

```bash
$ killall -TERM snakemake
```

## Output

After the workflow is finished, the files created during the workflow can be found under result/_base_name_/.

## Steps of the Pipeline
