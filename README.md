# Workshop "Principles on population genomic analyses" - 4-8 March 2024
## Instructors: [Sidonie Bellot](https://www.kew.org/science/our-science/people/sidonie-bellot) [(Royal Botanic Gardens, Kew)](https://scholar.google.com/citations?user=KREJ2JsAAAAJ) & [Jeronimo Cid (Bangor University - Royal Botanic Gardens, Kew)](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwi26um3rMqEAxXNna8BHabsCMUQFnoECBIQAQ&url=https%3A%2F%2Fwww.bangor.ac.uk%2Fnatural-sciences%2Fresearch-students%2Fjeronimo-cid-65d6abf0-d6ad-46ca-99d2-23dde9e862ff%2Fen&usg=AOvVaw2KMzf2RiEUtnKgZcXyFjOy&opi=89978449)
## 
### Organizers: [Liam Trethowan](https://www.kew.org/science/our-science/people/liam-trethowan) [(Royal Botanic Gardens, Kew)](https://scholar.google.com/citations?user=FgqqcMMAAAAJ), [Himmah Rustiami](https://scholar.google.com/citations?user=sluGEjEAAAAJ&hl=en) [(Herbarium Bogoriense-BRIN)](https://brin.go.id/en), [Oscar A. Pérez-Escobar](https://scholar.google.co.uk/citations?user=tSzyp6QAAAAJ&hl=en), Sidonie Bellot.
##### This workshop is funded and supported by: [The Darwin Innitiative UK](https://www.darwininitiative.org.uk) - [Royal Botanic Gardens, Kew (RBG Kew)](https://www.kew.org) - [Herbarium Bogoriense-BRIN)](https://brin.go.id/en)

## 1. Introduction
This repository contains a tutorial guide to basic population genetic analysis, using as input data illumina read sequencing derived from population level sampling. The tutorial is based upon the study of Pérez-Escobar et al., [(2021)](https://academic.oup.com/mbe/article/38/10/4475/6311667), and will make use of read sequence data obtained from 32 date palm and their close wild living relativces (_Phoenix dactylifera, P. atlantica, P. canariensis_). It also includes data produced in intermediary steps to ensure that the tutorial can be executed completely.   

This tutorial is intended for users with a basic knowledge in programming and is designed to run in UNIX environments. The participant should ideally have experience using shell and text file manipulation (e.g., using **awk, sed, grep, among others**), but also some experience coding in R (for plotting figures). The workshop will be run on pre-configured laptops (Ubuntu 22.04). _A basic introduction to the UNIX enviroment with some useful commands is available [here](https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/bash_tutorial.md)_. 

This tutorial requires the following programs/dependencies (it is highly recommended to have these installed before starting the tutorial). **Please make sure that the dependencies on which these programs run are also available**:

1. [**PALEOMIX:**](https://paleomix.readthedocs.io/en/stable/) PALEOMIX is a bioinformatics pipeline designed to analyzing contermporary and ancient DNA (aDNA) sequencing data in a population genomics framework. The pipeline has three modules but here we will work only with the "_bam_pipeline_" module. It begins by adapter trimming and quality filtering read data. Then the filtered read data is aligned to a reference genome using BWA/Bowtie2 algorithms. The output are [BAM files](https://en.wikipedia.org/wiki/Binary_Alignment_Map#:~:text=Binary%20Alignment%20Map%20(BAM)%20is,the%20Sequence%20Alignment%20Map%2Dfiles.). Read trimming and quality filtering as well as read mapping are all set and controlled through a *.yaml file. 
2. [**Adapterremoval:**](https://adapterremoval.readthedocs.io/en/stable/) AdapterRemoval is a tool  designed for the removal of adapter sequences and low-quality bases from high-throughput sequencing data.
3. [**BWA**](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwiJw4ClmtCEAxW-mlYBHcqRAawQFnoECBoQAQ&url=https%3A%2F%2Fgithub.com%2Flh3%2Fbwa&usg=AOvVaw2UQDwSP6x4_7vvSFTzRZGr&opi=89978449)[**/Bowtie2:**](https://bowtie-bio.sourceforge.net/bowtie2/index.shtml) These software align short read sequence data against a reference genome. BWA relies on the Burrows-Wheeler Transform (BWT) algorithm to index the reference genome and align (map) short reads in a fast manner to said reference. Bowtie2 in contrast uses the FM-index (an approach based on the BWT algorithm) to align short reads. Both tools can map reads using different sensitivity thresholds, thus resulting on less accurate (faster) or more comprehensive (slower) aligning strategies.   
5. [**ANGSD:**](https://www.popgen.dk/angsd/index.php/ANGSD) This program calculate Genotype Likelihoods ([GLs](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3593722/)) and several other metrics from GLs, using as input a series of BAM files. The software also include a series of utilities that we will explore in this workshop, including NGSadmix ([to conduct admixture/structure analysis](https://www.popgen.dk/software/index.php/NgsAdmix)) and PCAngsd ([principal component analysis from Genotype Likelihoods](http://www.popgen.dk/software/index.php/PCAngsd)). The outcome of both NSGadmix and PCAngds is visualised in R (through the package [_ggplot2_](https://ggplot2.tidyverse.org)).

## 2. Workshop structure
This tutorial is divided into three steps:

A. [**Read trimming, mapping and validation**](https://github.com/siriusb-nox/PopGen_DARWIN_2024/blob/main/A_readmapping.md)

B. **Genotype Likelihood analisys**

C. **Principal component and Admixture/structure**

![Figure 1](https://github.com/siriusb-nox/PopGen_DARWIN_2024/blob/main/IMG/pipeline_OP_v0.JPG)
**Figure 1**: Simplified view of tutorial/pipeline

>[!IMPORTANT]
>**The base data needed to run this tutorial is available in the different subfolders of this repo (e.g., `XX` and `YY`), which will be copied in your local machine.**

## 2.1. Pipeline configuration
In any bioinformatics pipeline, it is essential to relate which programs the pipeline depends on. All the files needed to execute this tutorial are available at `/home/ontasia*/Documents`.


**For users with programs installed in a UNIX environment on personal computers**, these can be entered in the current session (terminal) using the following command, for example:

`PATH=$PATH:/directory/of/the/folder/programx`
