## A. Genotype Likelihood estimation

Having the outputs from read mapping for every sample (\*.bam files) we can then proceed with the estimation of variants or genotype likelihoods (GLs). On population genetics, there are a number of differences when it comes to choose working with variants or GLs, summarised in the table as follows:


| GLs  | Variants |
| ------------- | ------------- |
| GLs represent the probability of observing different genotypes at a specific genomic position given the sequencing data and a model of sequencing error  | Variants refer to specific differences or mutations in DNA sequences compared to a reference genome |
| Instead of working with a specific genotype assigned to each individual, GLs provide a probabilistic framework that incorporates uncertainty about the true genotype  | Variants are often single nucleotide polymorphisms (SNPs), insertions, deletions, or other types of genomic variation  |
