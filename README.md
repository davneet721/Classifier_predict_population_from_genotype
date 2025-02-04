Building a classifier that can predict sample superpopulation from snps-genotypes data from the thousand genomes project. Previously used PCA [here](https://github.com/davneet721/PCA_Genotypes) inspired by [this paper](https://www.nature.com/articles/nature07331) to show that the most data clusters by superpopulation groups -- except there's significant overlap between the European and American ancestry groups.

## Preview VCF on 42basepairs

* [Preview variants](https://42basepairs.com/browse/s3/1000genomes/release/20110521?file=ALL.chr22.phase1_release_v3.20101123.snps_indels_svs.genotypes.vcf.gz&preview=variants)
* [Subsample variants](https://42basepairs.com/browse/s3/1000genomes/release/20110521?file=ALL.chr22.phase1_release_v3.20101123.snps_indels_svs.genotypes.vcf.gz&preview=subsample&loci=22%3A16000000-16100000)


## Data files used:

```bash
# Download from 42basepairs
wget "https://42basepairs.com/download/s3/1000genomes/release/20110521/ALL.chr22.phase1_release_v3.20101123.snps_indels_svs.genotypes.vcf.gz"

wget "https://42basepairs.com/download/s3/1000genomes/release/20110521/ALL.chr22.phase1_release_v3.20101123.snps_indels_svs.genotypes.vcf.gz.tbi"

wget "https://42basepairs.com/download/s3/1000genomes/release/20110521/phase1_integrated_calls.20101123.ALL.panel"
```
## Results of PCA that Inspired Building a Classifier:
Principal component analysis (PCA) results in the samples clustering by superpopulation. The European, East Asian and African samples form unique clusters. Whereas, the American samples are spread across the PC1-PC2 space, with significant overlap with the European Cluster. This is expected as Europeans contribute signifcantly to American ancestry and snps distributions are commonly used to determine sample ancestry.

![PCA clusters samples by Superpopulation](https://github.com/davneet721/PCA_Genotypes/blob/main/Figures/Superpopulation_PCA.png)
