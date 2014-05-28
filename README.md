# Pipeline for ChIP-seq preprocessing

### Overview

Here is the pipeline I used for ChIP-seq preprocessing, including:

* align the fastq data to reference genome by bowtie or bowtie2.
* run FastQC to check the sequencing quality.
* remove all reads duplications of the aligned data.
* generate TDF files for browsing in IGV.
* run PhantomPeak to check the quality of ChIP.
* run ngs.plot to investigate the enrichment of ChIP-seq data at TSS, TES, and genebody.

The pipeline work flow is:

![work flow](https://raw.githubusercontent.com/ny-shao/chip-seq_preprocess/master/all_flowchart.png)

### Requirement

The softwares used in this pipeline are:

* [ruffus](https://code.google.com/p/ruffus/)
* [Bowtie](http://bowtie-bio.sourceforge.net/index.shtml)
* [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
* [samtools](http://samtools.sourceforge.net/)
* [IGVTools](http://www.broadinstitute.org/igv/igvtools)
* [PhantomPeak](http://code.google.com/p/phantompeakqualtools/) __In fact, the script **run_spp_nodups.R** is from PhantomPeak, but PhantomPeak still need to be installed in R.__
* [ngs.plot](https://code.google.com/p/ngsplot/)

Install above softwares and make sure they are in $PATH.

### Installation

Put the scripts in ./bin to a place in $PATH or add ./bin to $PATH.

### Usage

```bash
python pipline.py config.yaml
```

For the organization of projects, I generally follow this paper: [A Quick Guide to Organizing Computational Biology Projects](http://www.ploscompbiol.org/article/info%3Adoi%2F10.1371%2Fjournal.pcbi.1000424). Here because it is preprocessing, and real analysis will be peak calling, chromatin segmentation, and differential enrichment detection, so I just put the results of the preprocess in the data folder.

For the configuration yaml file, __project_dir: `~/projects/test_ChIP-seq`__ and __data_dir: "data"__ mean the data folder is `~/projects/test_ChIP-seq/data`, and the results will be put in the same folder. Fastq files should be under `~/projects/test_ChIP-seq/data/fastq` folder. `aligner` now could be `bowtie` or `bowtie2`, if not assigned, then default aligner is `bowtie`. For `bowtie2`, the system variable `$BOWTIE2_INDEXES` should be set before running.

The position of pipeline.py and config.yaml doesn't matter at all. But I prefer to put them under project/script/preprocess folder.

**Important:**

> To make ngs.plot part work, please name the fastq files in this way:

> Say condition A, B, each with 2 replicates, and one DNA input per condition.

> Name the files as A_rep1.fastq, A_rep2.fastq, A_input.fastq, B_rep1.fastq, B_rep2.fastq, and B_input.fastq.

> The key point is to make the same condition samples with common letters and input samples contain "input" or "Input" strings.

### Notes

The parameters of bowtie were just adopted from this [paper](http://www.nature.com/nprot/journal/v7/n1/full/nprot.2011.420.html).

In Bowtie2, default parameters are used.

### ToDos

+ A parser for log files to summary the QC results.