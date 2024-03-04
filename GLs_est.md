## A. Genotype Likelihood estimation - brief background

Having the outputs from read mapping for every sample (\*.bam files) we can then proceed with the estimation of variants or genotype likelihoods (GLs). On population genetics, there are a number of differences when it comes to choose working with variants or GLs, summarised in the table as follows:

| GLs  | Variants |
| ------------- | ------------- |
| GLs represent the probability of observing different genotypes at a specific genomic position given the sequencing data and a model of sequencing error  | Variants refer to specific differences or mutations in DNA sequences compared to a reference genome |
| Instead of working with a specific genotype assigned to each individual, GLs provide a probabilistic framework that incorporates uncertainty about the true genotype  | Variants are often single nucleotide polymorphisms (SNPs), insertions, deletions, or other types of genomic variation  |
| GLs are often estimated using statistical models that take into account factors such as base quality scores, read mapping quality, and sequencing depth | Variants are typically characterized by their genomic position, the reference allele (the allele found in the reference genome), and the alternative allele (the observed variant allele) |
| GLs are especially useful in situations where sequencing data may be noisy or ambiguous, such as in low-coverage or ancient DNA sequencing experiments | Variants are commonly represented in variant call format (VCF) files, which provide information about variant type, genomic coordinates, allele frequencies, and other annotations |

In general, given the nature of most datasets (i.e., low sequencing coverage for most individuals amongst populations), it is best to work with probabilistic frameworks such as GLs which are inferred **for a collection of individuals (as opposed to inferences on variants/GLs conducted on single individuals)**. As such, in this workshop, we will work with GLs from which we will then derive analyses that can reveal relatedness and ancestry of individuals sampled from different species. 

>[!NOTE]
>**GLs (or variants) can be calculated from any set of individuals and different sequencing depths. The same can be told of almost any population genomics metric (e.g., Heterozygosity (Hz), Effective population size (FSt), etc). What determines how biased your estimations are will depend on how robust your sampling is how well sampled your populations are. Here, a good sampling is defined by how well represented the genetic diversity of your populations are, instead of how well sequenced each individula is. In other words, sequencing many more individuals across the spectrum of a population at lower depths is better than sequencing few individuals at deeper representations (see Figure 1 below)**
>
>![Figure 1](https://github.com/siriusb-nox/PopGen_DARWIN_2024/blob/main/IMG/pone.0079667.g001.png)
>**Figure 1**: Bias in the estimation of segregating sites and expected _Hz_ under different sampling levels. Notice how less bias estimations are whenever many individuals are sampled at lower sequencing depths (1x) (taken from Fumagalli, 2013)


## B. Genotype Likelihood estimation - analysis

We will use `angsd` to infer GLs from a the BAM files produced by `paleomix`. `angsd` (abbreviation of Analysis of Next Generation Sequencing Data) is a versatile software widely used for analyzing genomic data generated from HTS technologies. It can estimate GLs and produce different metrics, including Fst, Site Frequency Spectrum (SFS), Tajima's D, amongst others. 

>[!IMPORTANT]
>`angsd` will always depart from the assumption that individuals included in a panel for GLs estimation are **diploid**.  

## Selected references
1. Fumagalli M. 2013. Assessing the Effect of Sequencing Depth and Sample Size in Population Genetics Inferences. [_PLoS ONE 8:e79667_](https://journals.plos.org/plosone/article/file?id=10.1371/journal.pone.0079667&type=printable)
