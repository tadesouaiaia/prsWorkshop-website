# Introduction to PLINK  (Part I) 

PLINK is the most popular software program for performing genome-wide association analyses, it is extremely extensive allowing a huge
number of analyses to be performed. It also includes many options for reformatting your data and provides useful data summaries. Software
packages are usually best learnt by having a go at running some of their basic applications and progressing from there (rather than
reading the entire user manual first!) - so we begin by running some basic PLINK commands and then work steadily towards performing more
sophisticated analyses through these PLINK tutorials.

## Sample Data 

This tutorial runs on the data in the pre-workshop materials downloaded [here](prep_testing.md).  If you have followed the previous directions, this data should be in: 

`~/prsworkshop/preworkshop_materials_("your OS")/Plink/tutorial/sample_data`.  

⚠️ All data used in this workshop are **simulated**. They have no specific biological meaning and are for demonstration purposes only.
After completing this practical, you should be able to:

1.  Explore and generate genetic data sets needed for GWAS
2.  Recode and reorder allelic data
3.  Use the PLINK website
4.  Select and exclude lists of samples and SNPs





In all of the instructions below:
- Anything in between the symbols *\<\>* needs to be changed in some way. For example, \<file_name\>
  indicates that you should replace that entire statement (including the *\<\>* symbols) with the appropriate file name. 
- **Bold** indicates non- command-line instructions (e.g. **right-click**)


### Exploring Data

To begin the tutorial please navigate to: 
    
    cd ~/prsworkshop/preworkshop_materials_("yourOS")/Plink/tutorial/sample_data 

First let's make sure we can call plink from this directory: 

    ../../code/plink

Next lets observe the files in the sample data directory:  

    ls 

We should see the following four files: **D1D.ped, D1D.map,
D1D.pcs1234, D1D.pheno1**. We first convert the "old" format ped/map
files to the more memory efficient binary format using the following command:
```
 ../../code/plink --file D1D --make-bed --out D1D
```
This generates three new files, **D1D.bim, D1D.fam, D1D.bed**. Type
`ls -l`, compare how much disk space the bim/fam/bed and ped/map files use.
    

Let's look at the following files by typing the following commands and **pressing q to quit** after each one: 

    less D1D.bim      # Marker / SNP information 
    less D1D.fam      # Individual information: IDs
    less D1D.pcs1234  # A PCA file that lists individuals first four prinicipal components
    less D1D.pheno1   # A phenotype file that lists individuals phenotypes 
`D1D.bed` is a binary file and stores the genotype **do not open this file**.

Investigate the format of the bim and fam files here
https://zzz.bwh.harvard.edu/plink/data.shtml#bed, scroll up for details.

What do you observe?
- What are columns 1, 2, 4, 5, 6 of the bim file?
- What are the columns of the fam file?
  
## Recoding alleles as counts

Genotype data in allele count format is very useful, for example to use in regression modelling in statistical software such as R.
Generate the D1D data in allele count format:

       ../../code/plink --bfile D1D --recodeA --out D1D_AC

📝 There are several options for recoding SNPs in different ways - more information on the PLINK website (see next section).
    Again note that a log file was created - skim the log file or screen output

Look inside the .raw file. 

- What do you think the 0/1/2 represent?
- Do there appear to be more 0s or 2s?
- Why might this be?
    
## PLINK website

Go to the [plink website](https://www.cog-genomics.org/plink/1.9/) and
skim through the front page to get an idea of PLINK's
functionality. Note the list of clickable links on the left side of
the website.

Under 'Data Management' (click the heading on the left) and read the
list of the diﬀerent ways you may want to recode and reorder data
sets. Don't attempt to read much further as this is a very large and
detailed section - a useful future resource but too much for today.

Under 'Input filtering', read the different ways SNPs can be filtered.

## Write SNP list and extract SNPs
The `--write-snplist` writes a list of SNPs (penultimate argument in
'Data Management'). Use this command along with the information that
you found on the PLINK website to create a command to extract a list
of SNPs. Below is a list of requirements - try to do this before you
go to the end of this section, where the full command is given and
explained.

1.  Set the D1D binary file as input

2.  Set MAF threshold to 0.05

3.  Set SNP missingness threshold to 0.05

4.  Add the appropriate command to write out a snp list containing only
    those SNPs with MAF above 0.05 and missingness below 0.05

5.  Use 'D1D_snps' as the output file name

6.  After the command has run, check the output for your SNP list and
    look at it with the default viewer.

You will now use the SNP list that you have created to extract those SNPs and create a new set of data files in a single command.

1.  Use the D1D binary file set as input

2.  Find the command for extracting a set of SNPs listed in a file (hint: Data Management section) and combine it with a command that
    you learned above to create binary files

3.  Use the output file name 'D1D_MAF_MISS'

----
📝 Log files are uselful to check that the number of SNPs and samples is as expected. Always check your your log files to ensure that they are sensible.
SNP lists can also be used to EXCLUDE SNPs - select 'exclude' above instead of 'extract'. 
Sample ID lists can also be used to 'keep' or 'remove' individuals in the same 'filter' window. Note that both sample IDs (FID IID,separated by a space are required in the sample file list.

----

**Solution 1:**

TO BE REVEALED LATER!!

**Solution 2:**

TO BE REVEALED LATER!!
       
# Performing QC & GWAS (Part II) 

Here we will work on the following skills: 

1.  Generate summaries of the data needed for QC
2.  Apply QC thresholds
3.  Perform GWAS

## Generate summaries to perform QC

There are many kinds of summaries of the data that can be generated by
PLINK in order to perform particular quality control (QC) steps, which
help to make our data more reliable. Some of these involve summaries
in relation to the individuals (e.g. individual missingness,
sex-check) and some relate to summaries of SNP data (e.g. MAF,
Hardy-Weinburg Equilibrium). Over the next few sub-sections you will
go through some examples of generating summary statistics that can be
used to perform QC.

### Individual missingness

1.  Use the D1D binary files to generate files containing missingness information (--missing). Use the output file name 'D1D_miss'

2.  Open the 2 files that were generated (lmiss & imiss).

- What do the two output files contain?
- In the imiss file, what is the meaning of the data in the column headed "F_MISS"?


### SNP Missingness

Look inside the file containing SNP missingness information: D1D_miss.lmiss.

- What is the meaning of the value under F_MISS?
- What does the command --test-missing do and why might it be useful?

### Hardy-Weinberg Equilibrium

1.  Generate HWE statistics using the --hardy option. Use output file name D1D_hardy.

2.  Open and examine results.

- Why are there multiple rows for each SNP and what does each mean?
- Which of the rows do you think should be used to exclude SNPs from the subsequent analysis (if any) for failing the HWE test? Why?

### Allele frequencies

1.  Generate allele frequencies using the command *\--*freq. Use
    D1D_freq as the output name.

2.  Examine the output.

- What is the heading of the column that tells you which nucleotide is the minor allele?
  
 📝 **This information is important to remember as many PLINK files use this notation. The minor allele is always labeled the same way** 


## Apply QC filters

#### There are diﬀerent strategies for performing QC on your data:

(a) Create lists of SNPs and individuals and use --remove, --extract, --exclude, --include to create new file sets (good for documentation, collaboration)

(b) Apply thresholds one at a time and generate new bed/bim/fam file (good for applying sequential filters)

(c) Use options (e.g. --maf ) in other commands (e.g. --assoc) to remove SNPs or samples at required QC thresholds during analysis.

----
📝 We have already seen how to select or exclude individuals or SNPs by first creating lists (a), so in this section we will set thresholds to generate new files 
   sets in a single command. However, it is useful to have lists of all SNPs and individuals excluded pre-analysis, according to the reason for exclusion, so 
   generating and retaining such files using the techniques that we used before for good practice.

----

### Apply individual missingness thresholds

1.  Generate new binary file sets (--make-bed) from the 'D1D' binary file set, removing individuals with missingness greater than 3% using a single command 
    (hint: In the 'Inclusion thresholds' section, see the 'Missing/person' sub-section). Use the output file name 'D1D_imiss3pc'

2.  Examine the output files (no need to open, and remember the bed file cannot be read) and the log file

- How many individuals were in the original file?
- How many individuals were removed?
- How many males and females were left after screening?

### Apply SNP missingness and MAF thresholds

1.  Create new binary file sets from the 'D1D_imiss3pc' binary file set (NOT the original D1D files) by setting MAF threshold to 0.05 and
    SNP missingness threshold to 0.02 (See 'Inclusion thresholds' to obtain the correct threshold flags). Use the output file name'D1D_imiss3pc_lmiss2pc_maf5pc

2.  Examine the output files and the log file

- How many SNPs were in the original files?
- How many SNPs were removed for low minor allele frequency?
- How many SNPs were removed for missingness?

### Apply Hardy-Weinberg thresholds

1.  Generate a new binary file set called 'D1D_QC' from the D1D_imiss3pc_lmiss2pc_maf5pc file, applying a HWE threshold of 0.0001.

2.  This is our final, QC'ed file set.

3.  Examine log and output files.

-How many SNPs were removed for HWE *p-values* below the threshold?

📝 **It is useful to know how to do this, but be careful about setting this threshold - strong association signals can cause departure from HWE and you may remove great results! Use a lenient threshold and apply to controls only to avoid this problem. HWE can also be checked post-hoc for each SNP.**

## Perform GWAS

### Case/Control GWAS - no covariates

Run the following code, which performs a genetic association study using logistic regression on some case/control data:

    ../../code/plink --bfile D1D_QC --logistic --adjust --pheno D1D.pheno1 --out D1D_CC




- What are the raw and Bonferroni-adjusted p-values for the top hit?
- What does this mean - is there a significant association?
- Are there any other significant associations?

### Case/Control GWAS - with covariates

Here we repeat the previous analysis but this time including some covariates. The file D1D.pcs1234 contains the first 4 principal components from a PCA on the genetic data.

Run the analysis specifying the covariates file:

    ../../code/plink --bfile D1D_QC --logistic --adjust --pheno D1D.pheno1 --covar D1D.pcs1234 --out D1D_CC_PCadj



- What are the raw and Bonferroni-adjusted p-values for the top hit?
- What does this mean - is there a significant association?
- Suggest a reason for the different results when adjusting for the 4 PCs?
