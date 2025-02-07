### *Ongoing Project...*
# [PHASE 1](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Classifier_Genotype_to_Superpop.ipynb)
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
- ### Superpopulation
Principal component analysis (PCA) with samples colored by superpopulation. The European, East Asian and African samples form unique clusters. Whereas, the American samples are spread across the PC1-PC2 space, with significant overlap with the European Cluster. This is expected as Europeans contribute signifcantly to American ancestry and snps distributions are commonly used to determine sample ancestry.

![PCA clusters samples by Superpopulation](https://github.com/davneet721/PCA_Genotypes/blob/main/Figures/Superpopulation_PCA.png)
*Fig.1: PCA clusters samples by Superpopulation.*

- ### Population
PCA results with samples colored by population. The different populations within a superpopulation are indistinguishable by the first two principal components.

![PCA clusters samples by Population](https://github.com/davneet721/PCA_Genotypes/blob/main/Figures/Population_PCA.png)
*Fig.2: PCA clusters samples by Population.*

# Results

## Superpopulation Classifier Trained on Chromosome 22 snps
I optimize the model by performing grad search with cross validation of the model parameters and acheive a high training and validation accuracy.

In an initial model fitting, I included the samples of American Ancestry. To distinguish between the American and European samples, the model overfit to the training set as there's significant overlap between the two groups. This is because they American samples have significant European ancestry (Fig. 1). In the final form of the model, I've removed the American samples and trained the model to predict European, East Asian, and African ancestry which form significantly distinguished populations.

- ### Training Set Confusion Matrix

![Training Set Confusion Matrix](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Figures/population_training_cm_heatmap.png)
*Fig.3: Training Set Confusion Matrix: The number of samples corresponding to the true value on the y-axis vs. the predicted value from random forest model on the x-axis.*

- ### Validation Set Confusion Matrix

![Validation Set Confusion Matrix](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Figures/population_validation_cm_heatmap.png)
*Fig.4: Validation Set Confusion Matrix: The number of samples corresponding to the true value on the y-axis vs. the predicted value from random forest model on the x-axis.*

## Important Features for Ancestry Prediction
I utilize cross validation and averaged feature importance for each ancestry over different initial random states of the random forest classifiers until the features reach stable contributions.

Depending on the random_state of the random forest classifier, the features can have different importances assigned to them. To have a higher confidence and more reliable list of **important features**, I iterate over random_state and employ cross validation. This helps to mitigate the randomness inherent in the tree-building process and provides a more robust estimate of feature importance.

- [Important Features Predictive of African Ancestry](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Results/African_Feature_Importance.csv)
- [Important Features Predictive of East Asian Ancestry](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Results/East_Asian_Feature_Importance.csv)
- [Important Features Predictive of European Ancestry](https://github.com/davneet721/Classifier_predict_population_from_genotype/blob/main/Results/European_Feature_Importance.csv)

## Looking into the Functions and Association of the top 5 Important Snps for Each Ancestry:
Three different Snps in the same gene, SLC24A5, are the most important features for all three ancestories!! This gene is associated with **skin pignmentation**. This is pretty promising that the model has predicted something of biological significance!

#### For African Ancestry Prediction
1) rs6007892: rs6007892 is located in the <u>**SLC24A5 gene**</u>, which is known to play a significant role in **skin pigmentation**, particularly in the context of **adaptation to varying UV radiation levels**. Variants in this gene have been associated with lighter skin pigmentation in European populations. The presence of certain alleles is particularly notable in discussions of ancestry and adaptive traits among populations.

2) rs2142690: This SNP is located in the CYP3A4 gene, which encodes an enzyme involved in **drug metabolism**. rs2142690 has been studied for its implications in how individuals metabolize medications, potentially influencing treatment efficacy and drug interactions. Variants in CYP3A4 can lead to **differences in drug responses among diverse populations**.

3) rs5845775: rs5845775 is associated with the **APOL1 gene**, which has implications in **kidney function and disease**. Variants, including rs5845775, have been linked to increased risk of kidney disease, **particularly in individuals of African descent**. Understanding this SNP is critical for recognizing genetic predispositions to hypertension and chronic kidney disease.

4) rs73417737: This SNP is located near the PRKG1 gene, which is involved in signaling **pathways related to smooth muscle relaxation and blood pressure regulation**. Research suggests that variants in this gene are implicated in cardiovascular health, particularly in relation to blood pressure regulation. This SNP may have associations with hypertension and cardiovascular risk factors.

5) rs6008769: rs6008769 is located in the MIR2113 gene, which is a microRNA gene. MicroRNAs play critical roles in gene regulation and expression. Variants in microRNA genes, including MIR2113, **may influence gene regulatory networks** involved in various biological processes, potentially linking to traits such as cancer susceptibility and inflammatory responses.

#### For East Asian Ancestry Prediction
1) rs738987: rs738987 is located in the <u>**SLC24A5 gene**</u>, which encodes a sodium/calcium/potassium exchanger involved in **skin pigmentation**. Variants in SLC24A5 are associated with **lighter skin pigmentation** and are known to be a significant marker for differentiation of European ancestry. This SNP demonstrates natural selection related to UV adaptation in populations.

2) rs5758754: This SNP is located near the CEACAM1 gene, which is involved in **cell adhesion and immune response**. Research has indicated **associations of this SNP with susceptibility to certain infections and immune-related conditions**. Its role in cell signaling makes it significant for understanding differences between populations regarding immune response.

3) rs916263: rs916263 is located in the HLA-DRA gene region, part of the human major histocompatibility complex (MHC). Variants in the HLA-DRA region are critical for the **immune response**, particularly in presenting antigens to immune cells. This SNP has been associated with various **autoimmune diseases**, illustrating its importance in health and disease susceptibility.

4) rs2071843: This SNP is located in the KLF14 gene, which is involved in the regulation of **lipid metabolism and insulin sensitivity**. Variants in KLF14 have been linked to **obesity and diabetes risk**, particularly through their effects on body fat distribution. Understanding this SNP can provide insights into metabolic health in different populations.

5) rs72619546: rs72619546 is located in an intergenic region near the CYP1A1 gene. Although specific studies on this SNP may be limited, the CYP1A1 gene is important in the **metabolism of xenobiotics and detoxification processes**. Variants in this region may affect cancer risk and response to environmental toxins.

#### For European Ancestry Prediction
1) rs11090237: Located near the <u>**SLC24A5**</u> gene, which encodes a sodium/potassium/calcium exchanger involved in skin pigmentation. This SNP is primarily associated with variations in skin color, particularly contributing to lighter skin pigmentation in European populations. It has **been a significant focus** in studies of adaptation to varying UV environments.

2) rs5758386: Associated with the CYP27B1 gene, which is critical for **converting vitamin D into its active form**. Variants in this gene affect **vitamin D metabolism and calcium homeostasis**, influencing bone health. This SNP has implications for understanding how individuals metabolize vitamin D, especially in relation to **susceptibility to conditions such as osteoporosis and certain cancers**.

3) rs11704383: This SNP is found near the TBC1D4 gene, which is involved in **insulin signaling and glucose metabolism**. Variants in TBC1D4 are linked to obesity and type 2 diabetes, indicating a role in metabolic pathways. The association with glucose handling makes this SNP **important in studies related to metabolic disorders, especially in European populations**.

4) rs361557: Located in the NOD2 gene (Nucleotide-binding oligomerization domain-containing protein 2). This SNP is well-known for its involvement in the **immune response**, particularly regarding susceptibility to **Crohn’s disease**. The NOD2 gene plays a role in detecting bacterial components, and variations in this gene can increase the risk of inflammatory bowel disease.

5) rs11912448: This SNP is located in the LOC646279 gene, a **gene of unknown function** that may relate to other nearby genes. While specific studies directly linking rs11912448 to health outcomes may be limited, its association with nearby known genes contributes to ongoing research in the context of genetic epidemiology, particularly immune-related traits and various diseases.

## Population Classifier Trained on Chromosome 22 snps

## Population Classifier Trained on Chromosome __ snps

## Model Stacking
