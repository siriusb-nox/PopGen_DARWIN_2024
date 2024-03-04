## A. Read trimming, mapping, and validation

Read filtering/trimming, mapping and realigning are essential steps towards the estimation of vartiants/Genotype Likelihoods. 
Read filtering and trimming do remove low quaility bases (as defined by a quality threshold set by the user) and sequencer adapters and barcodes that, should they remain on the read data, could bias subsequent scaffold assembly and mapping analyses. The quality of a base/nucleotide on a fastq file is defined by the [Phred Quality Score](https://en.wikipedia.org/wiki/Phred_quality_score), which ranges from 0 to 60. A minimum threshold for quality trimming is _Phred20_ (99% base call accuracy), but the usual standard is to use _Phred30_ (99.9% accuracy). Several programs can conduct read trimming/filtering (e.g., [**cutadapt**](https://cutadapt.readthedocs.io/en/stable/), [**trimmomatic**](http://www.usadellab.org/cms/?page=trimmomatic)), but my program of choice is [**trim_galore**](https://github.com/FelixKrueger/TrimGalore), for it offers automatised detection of adapters/barcodes (which usually need to be specified) and the sintaxis is simple. Since we previously covered how to trim and quality-filter reads, we will focus on read mapping and validation.
  
In this workshop, we will rely on the pipeline `paleomix` for read mapping and realigning. `paloemix` is an easy-to-use pipeline specilaised on the analysis of high-throughput sequencing (HTS) data derived from Illumina platforms. While this wrapper is tailored to work with data derived from ancient DNA samples, it can also handle high-quality sequencing data. It conducts read trimming/filtering and mapping against a reference genome, producing as output BAM files. 

`paleomix` requires as input \*.fastq files and a reference genome (in fasta format). Two great advantages of `paleomix` are:

**a) easy automation:** hundreds of read files can easily be set up for analysis through a configuration file (*.yaml format). The config file also offers multiple options for read mapping, including software and sensitivity thresholds. An example of a \*.yaml file can be found in the [BAM_CP](https://github.com/siriusb-nox/PopGen_DARWIN_2024/tree/main/BAM_CP) folder of this repo.

**b) statistics generation:** summary files with detailed information on number of reads and bases processed, coverage, duplicated reads and more are provided by default, for every read file analysed.


## B. YAML config file setting

By default, `paleomix` should be already available in your `PATH`. To generate a blank \*.yaml config filem type the following command:

```bash
paleomix bam_pipeline makefile > blank_makefile.yaml
```

>[!IMPORTANT]
>**Please read carefully the comments on the file (marked with "#"), which it can open in any text editor.**

Here is a brief description of the different steps that `paleomix` executes and dependencies it relies upon, and how to configure the \*.yaml file for each step accordingly:

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

To set the reference genome, all that is needed is to specify a path where the reference genome (\*.fasta format, sequential) is located in the corresponding section of the \*.yaml file (see below). 

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
For simplicity, in this tutorial we will work with a plastid reference genome of _Phoenix dactylifera_ (available in NCBI with accession number NC013991, also available in the `RefGenomes` folder of this repo and in your local machine). Replace "PATH_TO_PREFIX" with the absolute path and namefile as follows:

```yaml
    Path: /home/ontasia*/Documents/ONT-workshop-March-2024/RefGenomes/P_dactylifera_NC013991cp.fasta
```

**2. Read trimming and mapping:** `paleomix` relies on `adapterremoval` to filter out low quality nucleotides and adapters from \*.fastq files. When using raw \*.fastq files as input, the adapter sequences need to be provided, as well as the quality offset (i.e., [Phred33, or Phred64].(https://www.drive5.com/usearch/manual/quality_score.html)). 

```yaml
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

  # Settings for trimming of reads, see AdapterRemoval man-page
  AdapterRemoval:
     # Adapter sequences, set and uncomment to override defaults
#     --adapter1: AGATCGGAAGAGCACACGTCTGAACTCCAGTCACNNNNNNATCTCGTATGCCGTCTTCTGCTTG
#     --adapter2: AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGTAGATCTCGGTGGTCGCCGTATCATT
     # Some BAM pipeline defaults differ from AR defaults;
     # To override, change these value(s):
     --mm: 3
     --minlength: 25
     # Extra features enabled by default; change 'yes' to 'no' to disable
     --collapse: yes
     --trimns: yes
     --trimqualities: yes
```
We won't go into much detail on read trimming/filtered since we will be working with trimmed read data (be aware that up to this date, skipping the trimming in `paleomix` is not allowed). Now, the mapping relies on two programs, `bwa` or `bowtie2`. Whichever program is chosen, different parameters can be set to define the sensitivity of the mapping, but first the input \*.fastq files and samples names need to be specified. This is done as the last section of the file. 

```yaml
# Mapping targets are specified using the following structure. Uncomment and
# replace 'NAME_OF_TARGET' with the desired prefix for filenames.
NAME_OF_TARGET:
   #  Uncomment and replace 'NAME_OF_SAMPLE' with the name of this sample.
  NAME_OF_SAMPLE:
     #  Uncomment and replace 'NAME_OF_LIBRARY' with the name of this sample.
    NAME_OF_LIBRARY:
       # Uncomment and replace 'NAME_OF_LANE' with the name of this lane,
       # and replace 'PATH_WITH_WILDCARDS' with the path to the FASTQ files
       # to be trimmed and mapped for this lane (may include wildcards).
      NAME_OF_LANE: PATH_WITH_WILDCARDS
```

Here, repalce 'NAME_OF_TARGET' by a unique sample name. Usually I choose a mix between the sample name and the reference genome I am using as a target, e.g., `SRR106852_NC013991cp`. This is the most important parameter to set since the output files will be stored in a folder with such a name. The parameters `NAME_OF_SAMPLE`, `NAME_OF_LIBRARY`, `NAME_OF_LANE` can also be set at your own choice. I usually use `MySample` for `NAME_OF_SAMPLE`, and `SRR106852` for `NAME_OF_LIBRARY`. Lastly, on `NAME_OF_LANE`, specify the place where the trimmed fastq files are. For this workshop, we will rely on the files stored at `/home/ontasia*/Document/ONT-workshop-March-2024/fastq` The file should look like this:

```yaml
# Mapping targets are specified using the following structure. Uncomment and
# replace 'NAME_OF_TARGET' with the desired prefix for filenames.
SRR106852_NC013991cp:
   #  Uncomment and replace 'NAME_OF_SAMPLE' with the name of this sample.
  MySample:
     #  Uncomment and replace 'NAME_OF_LIBRARY' with the name of this sample.
    SRR106852:
       # Uncomment and replace 'NAME_OF_LANE' with the name of this lane,
       # and replace 'PATH_WITH_WILDCARDS' with the path to the FASTQ files
       # to be trimmed and mapped for this lane (may include wildcards).
      NAME_OF_LANE: /home/ontasia*/Document/ONT-workshop-March-2024/fastq/SRR106852_1.fastq
```
For simplicity, we will a use single mate from a paired-end file. Paired files can also be set, using the following notation:

```yaml
      NAME_OF_LANE: /home/ontasia*/Document/ONT-workshop-March-2024/fastq/SRR106852_{Pair}.fastq
```

Where `{Pair}` will be interpreted as the characters `1` or `2`.

For read mapping, we will rely on the software `bwa`. `bwa` has three strategies to map reads: MEM, backtrack and bwa-sw. We will use the MEM strategy which is tailored for reads that are longer > 70 bp (i.e., reads produced from freshly sequenced samples). A mapping quality can also be set at `MinQuality`, which is here defined by in how many different positions a read can align in a genome. The higher the threshold, the more unique positions the read will map (i.e., reads won't be mapped in repeated positions). Here, we will use a threshold of 0 in order to avoid excluding the inverted repeats from the analysis.

```yaml
# Settings for aligners supported by the pipeline
  Aligners:
    # Choice of aligner software to use, either "BWA" or "Bowtie2"
    Program: bwa

    # Settings for mappings performed using BWA
    BWA:
      # One of "backtrack", "bwasw", or "mem"; see the BWA documentation
      # for a description of each algorithm (defaults to 'backtrack')
      Algorithm: mem
      # Filter aligned reads with a mapping quality (Phred) below this value
      MinQuality: 0
      # Filter reads that did not map to the reference sequence
      FilterUnmappedReads: yes
      # May be disabled ("no") for aDNA alignments with the 'aln' algorithm.
      # Post-mortem damage localizes to the seed region, which BWA expects to
      # have few errors (sets "-l"). See http://pmid.us/22574660
      UseSeed: yes
      # Additional command-line options may be specified for the "aln"
      # call(s), as described below for Bowtie2 below.
```

Following read mapping with `bwa`, `samtools` will sort the \*.bam files (arrange the coordinates of the reads mapped against a reference). This a default step and there is no need to set it up in the \*.yaml file.

**3. BAM file validation:**
The last step towards the production of a \*.bam file is the validation step, conducted by the software `picard` and `samtools`. Here, the file gets indexed and then assessed for any fault in the formatting of the file. Again, it is conducted by default and there is no need to set it up in the \*.yaml file. 

**4. Statistics:**
The last step in the analysis consist on the generation of several report *.txt files, which provide useful information on the the number of read data filtered out, mapped, and coverage attained, among other statistics. The most important file is the one bearing the extension *.summary. Here is a sneak-preview of how this file looks like:

```bash
# Command:
#     /usr/local/bin/paleomix bam_pipeline run mypaloemixfile.yaml
#
# Directory:
#     /path/to/your/own/dir/
#
# Makefile:
#     Filename: mypaloemixfile.yaml
#     SHA1Sum:  30df0b7cd0263f6c483b2946deab70109a19b0d1
#     MTime:    2020-10-30 19:16:40
#
# Genomes:
#     Name               Label    Contigs    Size      Prefix
#    P_dactylifera_NC013991cp.fasta    -        1          156905    /home/ontasia*/Documents/ONT-workshop-March-2024/RefGenomes/P_dactylifera_NC013991cp.fasta
#
# Regions Of Interest:
#     Genome    ROI    Size    NFeatures    NIntervals    Path
#
#
Target  Sample  Library Measure Value   # Description
SRR106852_NC013991cp   *       *       lib_type        SE      # SE, PE, or * (for both)
SRR106852_NC013991cp   *       *       seq_reads_se    158090383       # Total number of single-ended reads
SRR106852_NC013991cp   *       *       seq_trash_se    68670252        # Total number of trashed reads
SRR106852_NC013991cp   *       *       seq_trash_se_frac       0.434373367291  # Fraction of SE reads trashed
SRR106852_NC013991cp   *       *       seq_retained_reads      89420131        # Total number of retained reads
SRR106852_NC013991cp   *       *       seq_retained_nts        3251987190      # Total number of NTs in retained reads
SRR106852_NC013991cp   *       *       seq_retained_length     36.3675064399   # Average number of NTs in retained reads

SRR106852_NC013991cp   *       *       hits_raw(Clan_NC032008cp)       43229   # Total number of hits (prior to PCR duplicate filtering)
SRR106852_NC013991cp   *       *       hits_raw_frac(Clan_NC032008cp)  0.000483437001451       # Total number of hits vs. total number of reads retained
SRR106852_NC013991cp   *       *       hits_clonality(Clan_NC032008cp) 0.359943556409  # Fraction of hits that were PCR duplicates
SRR106852_NC013991cp   *       *       hits_unique(Clan_NC032008cp)    27669   # Total number of hits (excluding any PCR duplicates)
SRR106852_NC013991cp   *       *       hits_unique_frac(Clan_NC032008cp)       0.000309426967849       # Total number of unique hits vs. total number of reads retained
SRR106852_NC013991cp   *       *       hits_coverage(Clan_NC032008cp)  5.67808546573   # Estimated coverage from unique hits
SRR106852_NC013991cp   *       *       hits_length(Clan_NC032008cp)    32.1992121146   # Average number of aligned bases per unique hit
...
```

The BAM outpout files (and their \*.bai indexes) should be stored in `/home/ontasia*/Documents/ONT-workshop-March-2024/BAM_CP` folder.

## $\color{orange}{\textsf{C. ACTIVITY}}$


## Selected references
1. Schubert M., et al. 2014. Characterisatyion of ancient DNA and modern genomes by SNP detection and phylogenomic and metagenomic analysis using PALEOMIX. [_Nat. Protoc_. 9: 1056](https://pubmed.ncbi.nlm.nih.gov/24722405/) 
