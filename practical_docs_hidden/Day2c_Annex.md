# Advanced Polygenic Risk Score Analyses

## Annex 

## Table of Contents

  1. [Key Learning Outcomes](#key-learning-outcomes)
  2. [Resources you will be using](#resources-you-will-be-using)
  3. [Datasets](#data-sets) 
  4. [Exercise 1 Estimating R<sup>2</sup> in case and control studies](#exercise-1-estimating-r2-in-case-and-control-studies)
  5. [Exercise 2 Overfitting caused by model optimisation](#exercise-2-Overfitting-caused-by-model-optimisation)
     1. [Out of Sample Validation](#out-of-sample-validation)
  7. [Exercise 3 Distribution of PRS](#exercise-3-distribution-of-prs)

## Key Learning Outcomes
After completing this practical, you should be able to:
  1. know how to adjust for ascertainment bias in case-control analysis
  2. Know how over-fitting aﬀects PRS results and how to handle it 
  3. understand distribution of PRS

## Resources you will be using 
To perform PRS analyses, summary statistics from Genome-Wide Association Studies (GWAS) are required. In this workshop, the following summary statistics are used:

|**Phenotype**|**Provider**|**Description**|**Download Link**|
|:---:|:---:|:---:|:---:|
|Height|[GIANT Consortium](https://portals.broadinstitute.org/collaboration/giant/index.php/GIANT_consortium_data_files)|GWAS of height on 253,288 individuals| [Link](https://portals.broadinstitute.org/collaboration/giant/images/0/01/GIANT_HEIGHT_Wood_et_al_2014_publicrelease_HapMapCeuFreq.txt.gz)|
|Coronary artery disease (CAD)|[CARDIoGRAMplusC4D Consortium](http://www.cardiogramplusc4d.org/)|GWAS on 60,801 CAD cases and 123,504 controls| [Link](http://www.cardiogramplusc4d.org/media/cardiogramplusc4d-consortium/data-downloads/cad.additive.Oct2015.pub.zip)|


## Data Sets

You will need to download the required files for this tutorial.       
    

You will find all practical materials in the [following link](https://drive.google.com/drive/folders/1s41DR0TnzknnY3-I89knlwQcjJGlzudo?usp=sharing). Relevant materials that you should see there at the start of the practical are as follows:

 📂: Base_Data
  - GIANT_Height.txt,
  - Cardio_CAD.txt,

 📂: Target_Data
  - TAR.fam
  - TAR.bim
  - TAR.bed
  - TAR.height
  - TAR.cad 
  - TAR.covariate

  - VAL.bed
  - VAL.bim
  - VAL.fam
  - VAL.covariate
  - VAL.height
    
 🛠️: Scripts
  - nagelkerke.R
  - Quantile.R
    
---

!!! Warning
  Note All target phenotype data in this worshop are **simulated**. They have no specific biological meaning and are for demonstration purposes only. 

---
<a href="#top">[Back to Top](#table-of-contents)</a>


## Exercise 1 Estimating R<sup>2</sup> in case and control studies
Bias in R<sup>2</sup> estimation caused by ascertained case/control samples can be adjusted using the equation proposed by **Lee et al (2011)**, which requires the sample prevalence (case/control ratio) and population prevalence as parameters. This function is implemented in PRSice and the adjustment can be performed by providing the population prevalence to the command **--prevalence**.

Residuals of logistic regression is not well defined, and in PRS analyses, Nagelkerke R<sup>2</sup> is usually used to represent the model R<sup>2</sup> (this is the default of PRSice). However, this R<sup>2</sup> does not account for the diﬀerence between sample prevalence (i.e. case-control ratio) and population prevalence, which can lead to bias in the reported R<sup>2</sup> (Figure 1.1a). 
>
  **Figure 1.1: Performance of diﬀerent R<sup>2</sup> when the study contains equal portion of cases and controls**
>
  **(a) Nagelkerke R<sup>2</sup>** 
![Figure 1.1a](https://drive.google.com/uc?id=1r3LV442RQT3CWGSAnZsU6QT1DqHtRKkd)
---

Bias in R<sup>2</sup> estimation caused by ascertained case/control samples can be adjusted using the equation proposed by **Lee et al. 2011 (Figure 1.1b)**, which requires the sample
prevalence (case/control ratio) and population prevalence as parameters. This function is implemented in PRSice and the adjustment can be performed by providing the population prevalence to the command **--prevalence**.
>
  **Figure 1.1: Performance of diﬀerent R<sup>2</sup> when the study contains equal portion of cases and controls**
>
  **(b) Lee adjusted R<sup>2</sup>** 
![Figure 1.1b](https://drive.google.com/uc?id=19ACkFb2gr7EU4dfcsTDPtprwtYPDQufo)
---


Now, account for the ascertainment of the case/control sample by including the population prevalence (let’s assume e.g. 5% here) in the PRSice command to obtain the adjusted (Lee) R<sup>2</sup> :

```
Rscript ./Software/PRSice.R \
--prsice Software/PRSice_mac \
--base  Base_Data/Cardio_CAD.txt  \
--target Target_Data/TAR \
--snp markername \
--A1 effect_allele \
--A2 noneffect_allele \
--chr chr \
--bp bp_hg19 \
--stat beta \
--beta \
--pvalue p_dgc \
--pheno Target_Data/TAR.cad \
--prevalence 0.05 \
--binary-target T \
--out Results/CAD.highres.LEER2
```
The results are written to the "Results" directory. Examine the results folder and each file that was generated. For more information about each file type, see  [here](https://choishingwan.github.io/PRSice/step_by_step/).

---
>
> ⭐ Check the *.summary file in the Results folder where you will find the usual (Nagelkerke) R<sup>2</sup> and the adjusted (Lee) R<sup>2</sup>.
>

>
  **Figure 1.2: Barplot of CAD Lee R<sup>2</sup>** 
![Figure 1.2](https://drive.google.com/uc?id=1ktv_UydzcZXUdup9oTB1LKkFyhCT9WdY)
---

---
>
> 
> 📌 To speed up the practical, we have generated a smaller gene-set file. If you want the full gene-set file, you can download it from the link above.
> 
> 📌 All target phenotype data in this workshop are simulated. While they reflect the corresponding trait data, they have no specific biological meaning and are for demonstration purposes only.
---

---
>
> 
>❓Has accounting for the population prevalence aﬀected the R<sup>2</sup>?
><details>
> <summary>Solution</summary>     
>
> Yes, the adjusted R<sup>2</sup> = 0.0521524 and default R<sup>2</sup> = 0.0442664      
>
></details>
>
> 
---
>
> 
>❓Would you expect a diﬀerence between the Nagelkerke R<sup>2</sup> and the Lee adjusted R<sup>2</sup> if the case/control ratio in the target sample reflects the disease prevalence in the population?
>
><details>
> <summary>Solution</summary>     
>
> No, the R<sup>2</sup> will be the same because the prevalence of the target is a true representation of the population prevalence.      
>
></details>
---
<a href="#top">[Back to Top](#table-of-contents)</a>

## Exercise 2 Overfitting caused by model optimisation

In PRS analyses, the shrinkage or tuning parameter is usually optimized across a wide range of parametric space (e.g. P -value threshold, proportion of causal SNPs). When both optimisation and association testing are performed on the target data, over-fitted results will be obtained. The accuracy and predictive power of over-fitted results are likely to diminish when replicated in an independent data set.

A simple solution is to perform permutation to obtain an empirical P -value for the association model, which is implemented in PRSice. Briefly, permutation is performed as follows:
1) Compute the P -value in your original data, denoted as obs.p, at the "best" threshold.
2) Then shuﬄe the phenotype and obtain the P -value of the "best" threshold for this null phenotype, denoted as null.p
3) Repeat 2) N times
4) Calculate the empirical P-value as:

 $` Pemp = (\sum(obs.p > null.pi + 1) / (N + 1) `$
---

You will have to specify the number of permutation (N ) to perform by providing --perm N as a parameter to PRSice.

```
Rscript ./Software/PRSice.R \
    --prsice Software/PRSice_mac \
    --base  Base_Data/GIANT_Height.txt \
    --target Target_Data/TAR \
    --snp MarkerName \
    --A1 Allele1 \
    --A2 Allele2 \
    --stat b \
    --beta \
    --pvalue p \
    --pheno Target_Data/TAR.height \
    --binary-target F \
    --cov Target_Data/TAR.covariate \
    --cov-col Sex \
    --perm 1000 \
    --out Results/Height.perm
```
---
>
  **Figure 1.3: Barplot of Height using 1000 permutations**
>
![Figure 1.3](https://github.com/tadesouaiaia/prsWorkshop-website/blob/main/practical_docs_hidden/practical_images/day2c_annex/Height.perm_BARPLOT_2023-06-30.png)
>
---

---
>
> 📝 **10000 permutations typically provide empirical P-values with high accuracy to the second decimal place (eg. 0.05), but smaller empirical P-values should be considered approximate.**
>
---
>
> ❓ What is the smallest possible empirical P-value when 10000 permutation are performed? 
>
><details>
> <summary>Solution</summary>     
>
> 1.5 X 10<sup>-34</sup>.      
>
></details>
>
> ❓ Is the height PRS significantly associated with height after accounting for the over-fitting implicit in identifying the best-fit PRS? How about CAD?
>
><details>
> <summary>Solution</summary>     
>
> Yes, the height PRS is significantly associated with height. After accounting for the over-fitting implicit in identifying the best-fit PRS, the emprical p-value is 0.000999001.
>
></details>
---
### Out of Sample Validation

The best way to avoid having results that are over-fit is to perform validation on an independent validation data set. We can perform validation of the previous height + covariate analysis with PRSice, using the independent VAL target sample as validation data and the "best" P-value threshold predicted in the VAL samples:

```
Rscript ./Software/PRSice.R \
    --prsice Software/PRSice_mac \
    --base  Base_Data/GIANT_Height.txt \
    --target Target_Data/VAL \
    --snp MarkerName \
    --A1 Allele1 \
    --A2 Allele2 \
    --stat b \
    --beta \
    --pvalue p \
    --pheno Target_Data/VAL.height \
    --binary-target F \
    --no-full \
    --bar-levels 0.0680001 \
    --fastscore \
    --cov Target_Data/VAL.covariate \
    --cov-col Sex \
    --out Results/Height.val
```
--- 
>
  **Figure 1.4: Barplot of Height validation dataset** 
![Figure 1.4](https://github.com/tadesouaiaia/prsWorkshop-website/blob/main/practical_docs_hidden/practical_images/day2c_annex/Height.val_BARPLOT_2023-06-30.png)
---
---
>
> ❓ Why do we use --bar-levels 0.0680001 --no-full and --fastscore in this script?
>
> ❓ How does the PRS R2 and P -value for the validation data set compare to the analysis on the TAR target data? Is this what you would expect? Why?
>
---

<a href="#top">[Back to Top](#table-of-contents)</a>

## Exercise 3 Distribution of PRS

Many PRS study publications include quantile plots that show an exponential increase in phenotypic value or / Odd Ratios (OR) among the top quantiles (e.g. an S-shaped quantile plot, e.g. Figure 1.6). 
>
  **Figure 1.5: An example of density plot for PRS**
>
![Figure 1.5](https://github.com/tadesouaiaia/prsWorkshop-website/blob/main/practical_docs_hidden/practical_images/day2c_annex/images020.png)
---
>
  **Figure 1.6: An example of a S-shaped quantile plot**
>
![Figure 1.6](https://github.com/tadesouaiaia/prsWorkshop-website/blob/main/practical_docs_hidden/practical_images/day2c_annex/images021.png)
---

This might lead us to believe that individuals with PRS values in the top quantiles have a distinctly diﬀerent genetic aetiology compared to the rest of the sample, or that there is epistasis/interactions causing there substantially higher risk. However, when we plot a normally distributed variable (e.g. a PRS) as quantiles on the X-axis then we expect to observe this exponential pattern even when the X variable only has a linear eﬀect on the Y variable. This is because the top (and bottom) quantiles are further away from each other on the absolute scale of the variable and so the diﬀerences in their eﬀects are larger than between quantiles in the middle of the distribution.

To understand this more, we will perform a simple simulation using R:
```
R
# First, we define some simulation parameters
n.sample <- 10000
PRS.r2 <- 0.01
# Then, we simulate PRS that follow a random normal distribution
prs <- rnorm(n.sample)
# We can then simulate the phenotype using the following script
pheno <- prs + rnorm(n.sample,mean=0, sd=sqrt(var(prs)*(1-PRS.r2)/(PRS.r2)))
# We can examine the relationship between the phenotype and prs 
# using linear regression
summary(lm(pheno~prs))
# Which shows that we have the expected PRS R2
# Group the phenotype and PRS into a data.frame
info <- data.frame(SampleID=1:n.sample, PRS=prs, Phenotype=pheno)
# Then we can generate the quantile plot. 
# To save time, we will load in the quantile plot script from Software
source("./Software/Quantile.R")
# Then we can plot the quantile plot using quantile_plot function
quantile_plot(info, "Results/Height", 100)
```
>
  **Figure 1.7: The resulting quantile plot**
>
![Figure 1.7](https://github.com/tadesouaiaia/prsWorkshop-website/blob/main/practical_docs_hidden/practical_images/day2c_annex/Height_QUANTILES_PLOT_2023-07-05.png)
---

---
>
> ❓ What is the shape of the resulting quantile plot?
>
> ❓ Try plotting the densities of the height or CAD PRS in R * - do they look normally distributed? Why? (*Hint: You can generate a density plot for the
PRS in R using plot(density(x)) where x is a vector of the PRS values in the sample).
>
---

<a href="#top">[Back to Top](#table-of-contents)</a>
