# Workshop "Principles on population genomic analyses" - 4-8 March 2024
## Instructors: [Sidonie Bellot](https://www.kew.org/science/our-science/people/sidonie-bellot) [(Royal Botanic Gardens, Kew)](https://scholar.google.com/citations?user=KREJ2JsAAAAJ) & [Jeronimo Cid (Bangor University - Royal Botanic Gardens, Kew)](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwi26um3rMqEAxXNna8BHabsCMUQFnoECBIQAQ&url=https%3A%2F%2Fwww.bangor.ac.uk%2Fnatural-sciences%2Fresearch-students%2Fjeronimo-cid-65d6abf0-d6ad-46ca-99d2-23dde9e862ff%2Fen&usg=AOvVaw2KMzf2RiEUtnKgZcXyFjOy&opi=89978449)
## 
### Organizers: [Liam Trethowan](https://www.kew.org/science/our-science/people/liam-trethowan) [(Royal Botanic Gardens, Kew)](https://scholar.google.com/citations?user=FgqqcMMAAAAJ), [Himmah Rustiami](https://scholar.google.com/citations?user=sluGEjEAAAAJ&hl=en) [(Herbarium Bogoriense-BRIN)](https://brin.go.id/en), [Oscar A. Pérez-Escobar](https://scholar.google.co.uk/citations?user=tSzyp6QAAAAJ&hl=en), Sidonie Bellot.
##### This workshop is funded and supported by: [The Darwin Innitiative UK](https://www.darwininitiative.org.uk) - [Royal Botanic Gardens, Kew (RBG Kew)](https://www.kew.org) - [Herbarium Bogoriense-BRIN)](https://brin.go.id/en)

## 1. Introduction
This repository contains a tutorial guide to basic population genetic analysis, using as input data illumina read sequencing derived from population level sampling. The tutorial is based upon the study of Pérez-Escobar et al., [(2021)](https://academic.oup.com/mbe/article/38/10/4475/6311667), and will make use of read sequence data obtained from 32 date palm and their close wild living relativces (_Phoenix dactylifera, P. atlantica, P. canariensis_). It also includes data produced in intermediary steps to ensure that the tutorial can be executed completely.   

This tutorial is intended for users with a basic knowledge in programming and is designed to run in UNIX environments. The participant should ideally have experience using shell and text file manipulation (e.g., using **awk, sed, grep, among others**), but also some experience coding in R (for plotting figures). The workshop will be run on pre-configured laptops (Ubuntu 22.04). _A basic introduction to the UNIX enviroment with some useful commands is available [here](https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/bash_tutorial.md)_. 

This tutorial requires the following programs/dependencies (it is highly recommended to have these installed before starting the tutorial). **Please make sure that the dependencies on which these programs run are also available**:

1. [**PALEOMIX:**](https://paleomix.readthedocs.io/en/stable/)
2. [**Adapterremoval:**](https://adapterremoval.readthedocs.io/en/stable/) 
3. [**BWA**](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwiJw4ClmtCEAxW-mlYBHcqRAawQFnoECBoQAQ&url=https%3A%2F%2Fgithub.com%2Flh3%2Fbwa&usg=AOvVaw2UQDwSP6x4_7vvSFTzRZGr&opi=89978449)[**/Bowtie2:**](https://bowtie-bio.sourceforge.net/bowtie2/index.shtml):
4. [**ANGSD:**](https://www.popgen.dk/angsd/index.php/ANGSD)

