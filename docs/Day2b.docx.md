# Advanced Polygenic Risk Score Analyses

## Day 2 - Afternoon Practical: Pathway PRS Analyses

## Table of Contents

  1. [Introduction to gene set (pathway) PRS analysis](#gene-set-analysis)
  2. [Inputs required for gene-set PRS analysis](#prset-inputs)
     1. [Molecular Signatures Database MSigDB](#molecular-signatures-Database-msigdb)
     2. [General Transfer Format file](#general-transfer-format-file)
     3. [Other inputs for gene-set PRS using PRSet](#other-inputs)
  3. [Undestanding the outcome of gene-set PRS](#gene-set-enrichment-analysis)
  4. [Additional Considerations](#considerations)
  5. [Exercise: Calculate gene-set PRS analysis](#exercise-4-gene-set-based-prs-analysis)

## Key Learning Outcomes

After completing this practical, you should be able to:
  1. Understand the motivation and rationale for calculating gene-set PRS.
  2. Identify the additional inputs required for gene-set PRS analysis.
  3. Differentiate between self-contained and competitive tests in gene-set PRS analyses.
  4. Interpret the outcomes of gene-set PRSs and how they differ from genome-wide PRS.
  5. Calculate gene-set based PRSs using PRSet.

## Resources you will be using 

Similar to standard genome-wide PRS analyses, summary statistics from Genome-Wide Association Studies (GWAS) and individual level genotype and phenotype data are required to perform gene-set PRS analyses. In this session, the following datasets are used:

|**Phenotype**|**Provider**|**Description**|**Download Link**|
|:---:|:---:|:---:|:---:|
|Height|[GIANT Consortium](https://portals.broadinstitute.org/collaboration/giant/index.php/GIANT_consortium_data_files)|GWAS of height on 253,288 individuals| [Link](https://portals.broadinstitute.org/collaboration/giant/images/0/01/GIANT_HEIGHT_Wood_et_al_2014_publicrelease_HapMapCeuFreq.txt.gz)|
|Coronary artery disease (CAD)|[CARDIoGRAMplusC4D Consortium](http://www.cardiogramplusc4d.org/)|GWAS on 60,801 CAD cases and 123,504 controls| [Link](http://www.cardiogramplusc4d.org/media/cardiogramplusc4d-consortium/data-downloads/cad.additive.Oct2015.pub.zip)|

Additionally, to perform gene-set level analyses, information about the genomic regions (pathways, gene-sets, lists of SNPs) for which we want to calculate the PRSs are required. This information can be obtained from the following database:

|**Data Set**|**Description**|**Download Link**|
|:---:|:---:|:---:|
|Ensembl Human Genome GTF file|A file containing the coordinates for genes in the human genome. Used by PRSet to map the SNPs onto genic regions| [Link](https://ftp.ensembl.org/pub/release-109/gtf/homo_sapiens/) |
|MSigDB Gene Sets | File containing the gene-set information. *Free registration required.*| [Download here after registration](http://software.broadinstitute.org/gsea/msigdb/download_file.jsp?filePath=/resources/msigdb/6.1/h.all.v6.1.symbols.gmt)|

<a href="#top">[Back to Top](#table-of-contents)</a>

  3. [Exercise: Calculate gene-set PRS analysis](#exercise-4-gene-set-based-prs-analysis)
  4. [Undestanding the outcome of gene-set PRS](#gene-set-enrichment-analysis)
  5. [Additional Considerations](#considerations)

## Introduction to gene set (pathway) PRS analysis
Currently, most PRS analyses have been performed on a genome-wide scale, disregarding the underlying biological pathways. Here we will learn how to use a gene set (or pathway) based PRS analyses. The key difference between genome-wide PRS and gene set or pathway-based PRSs analyses is that, instead of aggregating the estimated effects of risk alleles across the entire genome, pathway-based PRSs aggregate risk alleles across k pathways (or gene sets) separately.

Gene-set PRS analyses may account for genomic sub-structure, constitute an extension to the classic polygenic model of disease, and may better reflect disease heterogeneity. For more information about the rationale and the software that we are going to use, please see the PRSet publication [PRSet: Pathway-based polygenic risk score analyses and software](https://doi.org/10.1371/journal.pgen.1010624). 

In this practical, we will go through some common file formats for gene-set analysis and will then calculate some gene-set (or pathway) based PRS.

## Inputs required for gene-set PRS analysis
### Molecular Signatures Database MSigDB
The Molecular Signatures Database (MSigDB) oﬀers an excellent source of gene-sets, including the hallmark genes, gene-sets of diﬀerent biological processes, gene-sets of diﬀerent oncogenic signatures etc. All gene-sets from MSigDB follows the Gene Matrix Transposed file format (GMT), which consists of one line per gene-set, each containing at least 3 column of data:

| | | | | |
|:---:|:---:|:---:|:---:|:---:|
|Set A| Description | Gene 1 | Gene 2 | ...
|Set A| Description | Gene 1 | Gene 2 | ...

---
> ** Have a look at the Reference/Sets.gmt file. **
>
> ❓ How many gene-sets are there in the Reference/Sets.gmt file? 
>
> ❓ How many genes does the largest gene-set contain?
>
---
>
> 💬 While you can read the GMT file using Excel. You should be aware that Excel has a tendency to convert gene names into dates (e.g. SEPT9 to Sep-9)
>
---

As GMT format does not contain the chromosomal location for each individual gene, an additional file is required to provide the chromosoaml location such that SNPs can be map to genes.

### General Transfer Format file
The General Transfer Format (GTF) file contains the chromosomal coordinates for each gene. It is a **tab** separated file and all but the final field in each feature line must contain a value. "Empty" columns should be denoted with a ‘.’. You can read the full format specification here. One column that might be of particular interest is column 3: **feature**, which indicates what feature that line of GTF represents. This allows us to select or ignore features that are of interest.

You can find the description of each feature [here](http://www.sequenceontology.org/browser/obob.cgi).

### Other inputs for gene-set PRS using PRSet

#### Browser Extensible Data BED
Browser Extensible Data (BED) file (diﬀerent to the binary ped file from PLINK) is a file format to define genetic regions. It contains 3 required fields per line (chromosome, start coordinate and end coordinate) together with 9 additional optional field. A special property of BED is that it is a 0-based format, i.e. chromosome starts at 0, as opposed to the usual 1-based format such as the PLINK format. For example, a SNP on chr1:10000 will be represented as:

| | | |
|:---:|:---:|:---:|
|**1**|**9999**|**10000**|

---
>
> ❓ How should we represent the coordinate of rs2980300 (chr1:785989) in BED format?
>
---

#### List of SNPs

<a href="#top">[Back to Top](#table-of-contents)</a>

## Understand diﬀerence between self-contained and competitive gene-set analyses
     
When calculating gene-set based PRSs, we should consider an important aspect when calculating association in only one region of the genome

### Self-Contained vs Competitive Testing
The null-hypothesis of self-contained and competitive test statistics is diﬀerent:
  – **Self-Contained** - None of the genes within the gene-set are associated with the phenotype
  – **Competitive** - Genes within the gene-set are no more associated with the phenotype than genes outside the gene-set
Therefore, a bigger gene-set will have a higher likelihood of having a significant P -value from self-contained test, which is not desirable.

## Other consideration when analysisng and interpreting gene-set PRSs

<a href="#top">[Back to Top](#table-of-contents)</a>

## Exercise: Calculate gene-set PRS analysis

We are now ready to perform gene-set association analyses using PRSet.

To perform the PRSet analysis and obtain the set based PRS and competitive P-value, simply provide the GTF file and the GMT file to PRSice and specify the number of permutation for competitive P-value calculation using the --set-perm option.

```
Rscript ./Software/PRSice.R \
    --prsice Software/PRSice_linux  \
    --base Base_Data/GIANT_Height.txt \
    --target Target_Data/TAR \
    --A1 Allele1 \
    --A2 Allele2 \
    --snp MarkerName \
    --pvalue p \
    --stat b \
    --beta \
    --binary-target F \
    --pheno Target_Data/TAR.height \
    --cov Target_Data/TAR.covariate \
    --out Results/Height.set \
    --gtf Reference/Homo_sapiens.GRCh38.86.gtf \
    --wind-5 5kb \
    --wind-3 1kb \
    --msigdb Reference/Sets.gmt \
    --multi-plot 10 \
    --set-perm 1000
```

>
  **Figure 1.8: An example of the multi-set plot. Sets are sorted based on their self-contained R2 . Base is the genome wide PRS**
>
![Figure 1.8](/images/day3/Height.set_MULTISET_BARPLOT_2023-06-30.png)
---
>
> 📌 If the --wind-5 and --wind-3 flag is not specified, PRSet will use the exact coordinates of each gene as the boundary. By specifying eg. --wind-5 5kb and --wind-3 1kb then the boundary of each gene will be extended 5 kb towards the 5’ end and 1 kb towards the 3’ end so that regulatory elements of the gene can be included.
>
> 📍 By default, when calculating set based PRS, PRSet will not perform P -value thresholding. This is because the aim in gene-set analyses is to assess the overall signal in each gene-set, and compare which is most enriched for signal, rather than optimise predictive power as typically desirable for genome-wide PRS. Providing any of the following commands will activate P -value thresholding for set based PRS calculation: --lower, --upper, --inter, --bar-levels, --fastscore
>
---

>
> ❓ Can you plot the relationship between the gene-set R2 and the number of SNPs in each gene-set? What general trend can be seen?
>
> ❓ Considering the plot, what gene-sets do you think are most interesting and why?
>
> ❓ Why is it useful to have polygenic scores measured across gene-sets (or pathways) for individuals? Isn’t it suﬃcient to just obtain a ranking of gene-sets according to GWAS-signal enrichment?
>
---
