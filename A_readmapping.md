## Read trimming, mapping, and validation

Read filtering/trimming, mapping and realigning are essential steps towards the estimation of vartiants/Genotype Likelihoods. 
  Read filtering and trimming do remove low quaility bases (as defined by a quality threshold set by the user) and sequencer adapters and barcodes that, should they remain on the read data, could bias subsequent scaffold assembly and mapping analyses. The quality of a base/nucleotide on a fastq file is defined by the [Phred Quality Score](https://en.wikipedia.org/wiki/Phred_quality_score), which ranges from 0 to 60. A minimum threshold for quality trimming is _Phred20_ (99% base call accuracy), but the usual standard is to use _Phred30_ (99.9% accuracy). Several programs can conduct read trimming/filtering (e.g., [**cutadapt**](https://cutadapt.readthedocs.io/en/stable/), [**trimmomatic**](http://www.usadellab.org/cms/?page=trimmomatic)), but my program of choice is [**trim_galore**](https://github.com/FelixKrueger/TrimGalore), for it offers automatised detection of adapters/barcodes (which usually need to be specified) and the sintaxis is simple. Since we previously covered how to trim and quality-filter reads, we will focus on read mapping and validation.
  
In this workshop, we will rely on the pipeline `paleomix` for read mapping and realigning. `paloemix` is an easy-to-use pipeline specilaised on the analysis of high-throughput sequencing (HTS) data derived from Illumina platforms. It conducts read trimming/filtering and mapping against a reference genome, producing as output BAM files. 

`paloemix` requires as input \*.fastq files and a reference genome (in fasta format). Two great advantages of `paleomix` are:
**a** easy automation: hundreds of read files can easily be set up for analysis through a configuration file (*.yaml format) 
**b)** statistics generation: 

Here is a brief description of the different steps that PALEOMIX executes and dependencies it relies upon:

1. Reference genome indexing:
2. Read trimming:
3. Read mapping:
4. BAM file validation:
5. Statistics:

## Selected references
1. Schubert M., et al. 2014. Characterisatyion of ancient DNA and modern genomes by SNP detection and phylogenomic and metagenomic analysis using PALEOMIX. [_Nat. Protoc_. 9: 1056](https://pubmed.ncbi.nlm.nih.gov/24722405/) 
