## A. Read trimming, mapping, and validation

Read filtering/trimming, mapping and realigning are essential steps towards the estimation of vartiants/Genotype Likelihoods. 
Read filtering and trimming do remove low quaility bases (as defined by a quality threshold set by the user) and sequencer adapters and barcodes that, should they remain on the read data, could bias subsequent scaffold assembly and mapping analyses. The quality of a base/nucleotide on a fastq file is defined by the [Phred Quality Score](https://en.wikipedia.org/wiki/Phred_quality_score), which ranges from 0 to 60. A minimum threshold for quality trimming is _Phred20_ (99% base call accuracy), but the usual standard is to use _Phred30_ (99.9% accuracy). Several programs can conduct read trimming/filtering (e.g., [**cutadapt**](https://cutadapt.readthedocs.io/en/stable/), [**trimmomatic**](http://www.usadellab.org/cms/?page=trimmomatic)), but my program of choice is [**trim_galore**](https://github.com/FelixKrueger/TrimGalore), for it offers automatised detection of adapters/barcodes (which usually need to be specified) and the sintaxis is simple. Since we previously covered how to trim and quality-filter reads, we will focus on read mapping and validation.
  
In this workshop, we will rely on the pipeline `paleomix` for read mapping and realigning. `paloemix` is an easy-to-use pipeline specilaised on the analysis of high-throughput sequencing (HTS) data derived from Illumina platforms. While this wrapper is tailored to work with data derived from ancient DNA samples, it can also handle high-quality sequencing data. It conducts read trimming/filtering and mapping against a reference genome, producing as output BAM files. 

`paloemix` requires as input \*.fastq files and a reference genome (in fasta format). Two great advantages of `paleomix` are:

**a) easy automation:** hundreds of read files can easily be set up for analysis through a configuration file (*.yaml format). The config file also offers multiple options for read mapping, including software and sensitivity thresholds. An example of a \*.yaml file can be found in the [BAM_CP](https://github.com/siriusb-nox/PopGen_DARWIN_2024/tree/main/BAM_CP) folder of this repo.

**b) statistics generation:** summary files with detailed information on number of reads and bases processed, coverage, duplicated reads and more are provided by default, for every read file analysed.


## B. YAML config file setting

By default, `paleomix` should be already available in your `PATH`. To generate a blank \*.yaml config filem type the following command:

```bash
paleomix bam_pipeline makefile > blank_makefile.yaml
```

The file should look like this (**please read carefully the comments on the file [marked with "#"], which you can open in any text editor)**:

```yaml
# -*- mode: Yaml; -*-
# Timestamp: 2024-03-03T15:29:36.611911
#
# Default options.
# Can also be specific for a set of samples, libraries, and lanes,
# by including the "Options" hierarchy at the same level as those
# samples, libraries, or lanes below. This does not include
# "Features", which may only be specific globally.
Options:
  # Sequencing platform, see SAM/BAM reference for valid values
  Platform: Illumina
  # Quality offset for Phred scores, either 33 (Sanger/Illumina 1.8+)
  # or 64 (Illumina 1.3+ / 1.5+). For Bowtie2 it is also possible to
  # specify 'Solexa', to handle reads on the Solexa scale. This is
  # used during adapter-trimming and sequence alignment
  QualityOffset: 33
  # Split a lane into multiple entries, one for each (pair of) file(s)
  # found using the search-string specified for a given lane. Each
  # lane is named by adding a number to the end of the given barcode.
  SplitLanesByFilenames: yes
  # Compression format for FASTQ reads; 'gz' for GZip, 'bz2' for BZip2
  CompressionFormat: bz2
...
```

Here is a brief description of the different steps that `paleomix` executes and dependencies it relies upon, amnd how to configure the \*.yaml file for each step accordingly:

**1. Reference genome indexing:** in this step, `paleomix` produces a index (\*.fai file) of the reference genome (\*.fasta format) through the program `samtools`. A genome index \*.fai file is a tab delimited file and looks like this:

```
gi|394055774|gb|AGTA02000001.1|    5516    93    70    71
gi|394055773|gb|AGTA02000002.1|    2292    5781    70    71
gi|394055772|gb|AGTA02000003.1|    4668    8199    70    71
gi|394055771|gb|AGTA02000004.1|    1190    13027    70    71
```

Where:
```
Column one: indicates the contig name in the reference genome.
Column two: number of nucleotides in the contig.
Column three: Byte index of the document where the contig sequence begins.
Column four: Nucleotides per line in the reference genome (this number will be equal to the number of nucleotides in the contig when the fasta file is sequential (as opposed to interleaved).
Column five: Bytes per line the reference genome.
```

To set the reference genome, all that is needed is to specify a path where the reference genome (\*.fasta format, sequential) is located. 

```yaml
# Map of prefixes by name, each having a Path key, which specifies the
# location of the BWA/Bowtie2 index, and optional label, and an option
# set of regions for which additional statistics are produced.
Prefixes:
  # Replace 'NAME_OF_PREFIX' with name of the prefix; this name
  # is used in summary statistics and as part of output filenames.
  NAME_OF_PREFIX:
    # Replace 'PATH_TO_PREFIX' with the path to .fasta file containing the
    # references against which reads are to be mapped. Using the same name
    # as filename is strongly recommended (e.g. /path/to/Human_g1k_v37.fasta
    # should be named 'Human_g1k_v37').
    Path: PATH_TO_PREFIX
```
For simplicity, in this tutorial we will work with a plastid reference genome of _Phoenix dactylifera_ (available in NCBI ewith accession number NC013991, also available in the `RefGenomes` folder of this repo and in your local machine). Replace "PATH_TO_PREFIX" with the absolute path and namefile as follows:

```yaml
    Path: /home/ontasia*/Documents/ONT-workshop-March-2024/RefGenomes/P_dactylifera_NC013991cp.fasta
```

**2. Read trimming:** `paleomix` relies on `adapterremoval` to filter out low quality nucleotides and adapters from \*.fastq files. 

3. Read mapping:
4. BAM file validation:
5. Statistics:

## Selected references
1. Schubert M., et al. 2014. Characterisatyion of ancient DNA and modern genomes by SNP detection and phylogenomic and metagenomic analysis using PALEOMIX. [_Nat. Protoc_. 9: 1056](https://pubmed.ncbi.nlm.nih.gov/24722405/) 
