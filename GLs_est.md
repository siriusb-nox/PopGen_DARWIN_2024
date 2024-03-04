## A. Genotype Likelihood estimation

Having the outputs from read mapping for every sample (\*.bam files) we can then proceed with the estimation of variants or genotype likelihoods (GLs). On population genetics, there are a number of differences when it comes to choose working with variants or GLs, summarised in the table as follows:

| GLs  | Variants |
| ------------- | ------------- |
| GLs represent the probability of observing different genotypes at a specific genomic position given the sequencing data and a model of sequencing error  | Variants refer to specific differences or mutations in DNA sequences compared to a reference genome |
| Instead of working with a specific genotype assigned to each individual, GLs provide a probabilistic framework that incorporates uncertainty about the true genotype  | Variants are often single nucleotide polymorphisms (SNPs), insertions, deletions, or other types of genomic variation  |
| GLs are often estimated using statistical models that take into account factors such as base quality scores, read mapping quality, and sequencing depth | Variants are typically characterized by their genomic position, the reference allele (the allele found in the reference genome), and the alternative allele (the observed variant allele) |
| GLs are especially useful in situations where sequencing data may be noisy or ambiguous, such as in low-coverage or ancient DNA sequencing experiments | Variants are commonly represented in variant call format (VCF) files, which provide information about variant type, genomic coordinates, allele frequencies, and other annotations |

In general, given the nature of most datasets (i.e., low sequencing coverage for most individuals amongst populations), it is best to work with probabilistic frameworks such as GLs which are inferred **for a collection of individuals (as opposed to inferences on variants/GLs conducted on single individuals)**. As such, in this workshop, we will work with GLs from which we will then derive analyses that can reveal relatedness and ancestry of individuals sampled from different species. One very important aspect to remember is that...

## Selected references
1. Fumagalli M. 2013. Assessing the Effect of Sequencing Depth and Sample Size in Population Genetics Inferences. [PLoS ONE 8:e79667](https://journals.plos.org/plosone/article/file?id=10.1371/journal.pone.0079667&type=printable)
