## A. Principal Component Analysis (PCA) of GLs
One important aspect in population genomics is the understanding of how multiple individuals from diverse ancestries are rleated to each other. One approximation to such understanding is by inferring how the genetic diversity of individuals is structured in populations. A popular approach to discern structure amongst populations is by using reduction dimension methods, such as Principal Component Analysis (PCA). Here, genetic variation is summarised through covariance matrices which are then depicted through coordinates in a dotplot graph. 

In this tutorial, we will conduct PCA analyses through the package `PCAngsd`, an associated software to `angsd`. `PCangsd` is basically a python script, which all that requires to run is a \*.beagle file as input. To execute PCAngsd, run the following command:

```bash
python pcangsd.py -beagle genolike.beagle.gz -threads 10 -o covariance.outfile -iter 10000
```

where

```bash
-beagle # This parameter specifies the input beagle file (GLs)
-iter # This paremeter indicates the number of maximum iterations to estimate allele frequencies (is good to increase it since the default is 100 and might not be enough whenever large number of individuals are being analysed). The analysis will stop once the pogram reaches a level of confidence on the estimation of allele frequences.
-threads # This parameter specifies the number of threads (or CPU cores) to use for parallel processing. Increase this parameter whenever working with large genomes and many individuals.
```

As an output, the command will produce a \*.cov file, which can be used as input in R to produce a dotplot and visualise genetic variation of individuals in two dimensions (see Figure 1 for an example on the plot that can be produced in R through the `ggplot2` package).

![Figure 1](https://github.com/siriusb-nox/PopGen_DARWIN_2024/blob/main/IMG/32S_PCAngsd.CP.jpg)
**Figure 1**. Principal component analysis derived from plastid GLs calculated from 32 individuals. Dots have been coloured following species names assigned to samples. 

>[!CAUTION]
>**PCA analyses are not meant to be used as tools for clasification of individuals into discrete populations. They are rather a convenient mean to help understanding how much genetic variation there is between individuals and populations along axes.**


## B. Admixture/Structure analysis from GLs
Another important aspect of population genomics the understanding of how populations exchange genetic material and at which proportions. Admixture analysis is then useful to trave the ancestry of admixed proportions of a genome in a given individual, thus allowing to clasify indivduals of unknown ancestry into discrete populations. Such discrete ancestral populations can be modeled by the user, and often multiple numbers of ancestral populations are used as input, with the best fit model assessed by comparing log-likelihood values produced by the analysis. 

In this tutorial, we will infer admixture through the package `NGSadmix`. The program requires as input a \*.beagle file and a number of assumed ancestral populations. To automatise the analysis, we can code a `while` loop in bash and iterate through the different ancestral population models we want to test. The `while` loop will iterate across a tab -delimited text file (i.e., `clusters.k` file). To execute `NGSadmix`, run the following command:

```bash
while read K; do NGSadmix -likes genolike.beagle.gz -K $K -outfiles ${K}.NGSadmix.out -minMaf 0.05 -P 8; done < clusters.k
```

where

```bash
-likes # This parameter specifies the input beagle file (GLs)
-K # This paremeter indicates the number of assumed ancestral populations that will be modeled. It can range from 1 to any number of your choice.,
-minMaf # This parameter indicates the minimum minor allele frequency (is a filtering criterium).
-P # This parameter specifies the number of threads to use for parallel processing. Increase this parameter whenever working with large genomes and many individuals.
-outfiles # This parameter indicates a prefix that will be assigned to the output files.
```

the `clusters.k` file should look like this:

```bash
1
2
3
4
5
6
...
```

As an output, the command will produce (per analysed ancestral population): 

**a)** a \*.log file where the number of individuals, sites analysed and filtered out, and likelihoods are provided. 
**b)** a \*.fopt file, where allele frequences on the assumed ancestral populations are provided
**c)** a \*.qopt file, with the estimated admixture proportions per individuals. These files are needes as input for the bar plot in R. 

A traditional admixture plot presents the proportions (from 0-100, y-axis) of genomic ancestry for each individual (x-axis) across multiple assumed ancestral populations (see Figure 2 for an example).  

![Figure 2](https://github.com/siriusb-nox/PopGen_DARWIN_2024/blob/main/IMG/Admix_32S_NUC.jpg)

**Figure 2**. Population structure analyses of 32 individuals of date palms as inferred from nuclear GLs and 2-6 assumed ancestral populatuions. 

>[!CAUTION]
>**Admixture models assume that GLs have been computed from recombinant genomic regions (i.e., not plastids!). Here, we are estimating Amdixture on GLs derived from a plastid dataset for simplicity.**

## $\color{orange}{\textsf{C. ACTIVITY}}$
1. Conduct a PCA and an admixture analysis on the `genolike.32s.CP.beagle.gz` file provided at the directory `/home/ontasia*/Documents/ONT-workshop-March-2024/ANGSD/CP_SRA_26796/NGSADMIX/`.
2. Plot the results of PCA and NGSadmix in R.


## Selected references
1. Meisner J., Albrechtsen A. 2018. Inferring population structure and admixture proportions in low-depth NGS data. [_Genetics 210:719_](https://academic.oup.com/genetics/article/210/2/719/5931101)
2. Skotte L., Korneliussen T., Albrechtsen A. 2013. Estimating individual admixture proportions from NGS data. [_Genetics 195:693_](https://academic.oup.com/genetics/article/195/3/693/5935455)
3. Li H., Raplh P. 2019. Local PCA shows how the effect of population structure differs along the genome. [_Genetics 211:289_](https://academic.oup.com/genetics/article/211/1/289/5931130)
