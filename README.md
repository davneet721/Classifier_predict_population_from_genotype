### *Ongoing Project...*
# PHASE 1
# Introduction

Building a classifier that can predict sample superpopulation and population (higher resolution) from snps-genotypes data from the thousand genomes project. Previously used PCA [here](https://github.com/davneet721/PCA_Genotypes) on chromosome 22 snps data from thousands of samples inspired by [this paper](https://www.nature.com/articles/nature07331) to show that the most data clusters by superpopulation groups (Fig.1) -- except there's significant overlap between the European and American ancestry groups. Superpopulations are comprised of populations. PCA on snps from the single chromosome don't result in clear population clusters (Fig.2)

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
# Background

## PCA on Chromosome 22 Snps that Inspired Building a Classifier:
### Principal component analysis (PCA) with samples colored by superpopulation. The European, East Asian and African samples form unique clusters. Whereas, the American samples are spread across the PC1-PC2 space, with significant overlap with the European Cluster. This is expected as Europeans contribute signifcantly to American ancestry and snps distributions are commonly used to determine sample ancestry.

![PCA clusters samples by Superpopulation](https://github.com/davneet721/PCA_Genotypes/blob/main/Figures/Superpopulation_PCA.png)
*Fig.1: PCA clusters samples by Superpopulation.*

### PCA results with samples colored by population. The different populations within a superpopulation are indistinguishable by the first two principal components.

![PCA clusters samples by Population](https://github.com/davneet721/PCA_Genotypes/blob/main/Figures/Population_PCA.png)
*Fig.2: PCA clusters samples by Population.*

# Results

## Superpopulation Classifier Trained on Chromosome 22 snps
I optimize the model by performing grad search with cross validation of the model parameters and acheive a high training and validation accuracy.

In an initial model fitting, I included the samples of American Ancestry. To distinguish between the American and European samples, the model overfit to the training set as there's significant overlap between the two groups. This is because they American samples have significant European ancestry (Fig. 1). In the final form of the model, I've removed the American samples and trained the model to predict European, East Asian, and African ancestry which form significantly distinguished populations.

![Training Set Confusion Matrix](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Figures/population_training_cm_heatmap.png)
*Fig.3: Training Set Confusion Matrix: The number of samples corresponding to the true value on the y-axis vs. the predicted value from random forest model on the x-axis.*

![Validation Set Confusion Matrix](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Figures/population_validation_cm_heatmap.png)
*Fig.4: Validation Set Confusion Matrix: The number of samples corresponding to the true value on the y-axis vs. the predicted value from random forest model on the x-axis.*

## Important Features for Ancestry Prediction
I utilize cross validation and averaged feature importance for each ancestry over different initial random states of the random forest classifiers until the features reach stable contributions.

Depending on the random_state of the random forest classifier, the features can have different importances assigned to them. To have a higher confidence and more reliable list of **important features**, I iterate over random_state and employ cross validation. This helps to mitigate the randomness inherent in the tree-building process and provides a more robust estimate of feature importance.

- [Important Features Predictive of African Ancestry](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Results/African_Feature_Importance.csv)
- [Important Features Predictive of East Asian Ancestry](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Results/East_Asian_Feature_Importance.csv)
- [Important Features Predictive of European Ancestry](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Results/European_Feature_Importance.csv)

## Population Classifier Trained on Chromosome 22 snps

## Population Classifier Trained on Chromosome __ snps

## Model Stacking
