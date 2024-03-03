## Read trimming, mapping, and realigning

Read filtering/trimming, mapping and realigning are essential steps towards the estimation of vartiants/Genotype Likelihoods. 
Read filtering and trimming do remove low quaility bases (as defined by a quality threshold set by the user) and sequencer adapters and barcodes that, should they remain on the read data, could bias subsequent scaffold assembly and mapping analyses. The quality of a base/nucleotide on a fastq file is defined by the [Phred Quality Score](https://en.wikipedia.org/wiki/Phred_quality_score), which ranges from 0 to 60. A minimum threshold for quality trimming is Phred20 (99% base call accuracy), but the usual standard is to use Phred30 (99.9% accuracy).

Since we previously covered how to trim and quality-filter reads, we will focus on read mapping and realigning.

## Selected references
1. 
