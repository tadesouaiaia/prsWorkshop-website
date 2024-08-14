## BridgePRS

### Learning Objectives
In the previous lecture we covered in detail the modelling used by
BridgePRS. Here we will use the BridgePRS software to apply the method.
The aim of this practical is to provide you with a basic understanding
and some experience of running BridgePRS software. After completing
this practical, you should:

* Be able to perform cross-population analyses.
* Set up the configuration files used as input by the software.
* Be familiar with running cross-ancestry PRS analyses using PRS-CSx.

### BridgePRS input data
In the BridgePRS directory there is a data folder which we will use in
this practical. View the data directory
```
$ ls -l data
total 5368
drwxr-xr-x  73 hoggac01  staff     2336 26 Jul 16:00 1000G_sample
-rw-r--r--   1 hoggac01  staff      469 12 Aug 14:35 afr.config
-rw-r--r--   1 hoggac01  staff      469 12 Aug 14:35 eas.config
-rw-r--r--   1 hoggac01  staff      410 12 Aug 14:35 eur.config
drwxr-xr-x   5 hoggac01  staff      160 14 Jul 17:22 pop_AFR
drwxr-xr-x   5 hoggac01  staff      160 14 Jul 17:22 pop_EAS
drwxr-xr-x   5 hoggac01  staff      160  7 Aug 20:30 pop_EUR
-rw-r--r--   1 hoggac01  staff   200376  7 Aug 21:30 qc_snplist.txt
```

The pop_\* folders contain simulated genotype, phenotype and GWAS
summary statistics representative of
Europeans, East Asians and Africans required by BridgePRS. Each pop_\*
folder is split into summary statistics, and
individual level genotype and phenotype folders, e.g.
```
$ ls -l data/pop_AFR/
total 0
drwxr-xr-x  68 hoggac01  staff  2176 14 Jul 17:22 genotypes
drwxr-xr-x   4 hoggac01  staff   128 14 Jul 17:22 phenotypes
drwxr-xr-x  68 hoggac01  staff  2176 12 Aug 11:02 sumstats
```
Look at each directory using ls. There are two sets of summary
statistics in each population folder from the analysis of a simulated
continuos phenotype. For computation speed the summary statistics are
only have a small subset of SNPs, 19k-20k genomewide
```
zcat data/pop_EAS/sumstats/EAS.chr* | wc -l
zcat data/pop_EUR/sumstats/EUR.chr* | wc -l
zcat data/pop_AFR/sumstats/AFR.chr* | wc -l
```
or on a Mac
```
gzcat data/pop_EAS/sumstats/EAS.chr* | wc -l
gzcat data/pop_EUR/sumstats/EUR.chr* | wc -l
gzcat data/pop_AFR/sumstats/AFR.chr* | wc -l
```
Results in these files are only shown for SNP with MAF>0.
The SNPs are a subset of the HapMap panel, there seems to
be a bias to polymorphoic EUR SNPs. Take a look a look at the files, e.g.
```
zcat data/pop_AFR/sumstats/AFR.chr19.glm.linear.gz | head 
zcat data/pop_AFR/sumstats/AFR_half.chr19.glm.linear.gz | head 
```
again use gzcat on a Mac.

In the OBS_CT column you'll see can that the "_half" summary
statistics files have half the sample size, 10,000 compared to 20,000
for the EAS and AFR populations and 40,000 compared to 80,000 for the
EUR population. The same SNPs are contained in each set of summary statistics.

The phenotypes folder has two files: "test" and "validation" with IDs and the outcome
phenotype. "Test" data is used to optimise the PRS and "validation"
data is not used to estimate the PRS, it is just to assess model performance.

The genotypes folders are in plink1.9 format and split by
chromosome. These folders contain the genetic data for individuals in
the phenotypes folder.

Test data will only be used for individuals with both genotype and phenotype
information. Similarly model performance metrics will only use  samples with both
genotype and phenotype information, however, predictions are generated
for all validation samples with genotype data.

### Passing arguments to run BridgePRS
Example run of BridgePRS:
```
./bridgePRS easyrun go -o out/ --config_files data/eas.config data/eur.config --fst 0.11 --phenotype y --cores 4 --restart
```
Here we see that arguments can be passed to BridgePRS on both the commandline and in
config files. config files will be used in this practical and are a
neat way to store these arguments. config files contain population
specific arguments, therefore for a standard two population analysis
two config are required. By default the first config is for the target
population and the second is base population. A full list of arguments
that can be used on the commandline can be found here....

#### The \*.config files
The Python wrapper for BridgePRS uses .config files to tell the
software where to find the required input files and the column headers of the
summary statistics files, take a look, e.g.
```
cat data/eas.config 
LD_PATH=1000G_sample
LDPOP=EAS
POP=EAS
SUMSTATS_PREFIX=pop_EAS/sumstats/EAS.chr
#SUMSTATS_PREFIX=pop_EAS/sumstats/EAS_half.chr
SUMSTATS_SUFFIX=.glm.linear.gz
GENOTYPE_PREFIX=pop_EAS/genotypes/chr
PHENOTYPE_FILE=pop_EAS/phenotypes/EAS_test.dat  
VALIDATION_FILE=pop_EAS/phenotypes/EAS_valid.dat 
COVARIATES=PC1,PC2
SNP_FILE=qc_snplist.txt
SSF-P=P
SSF-SNPID=ID
SSF-SE=SE
SSF-SS=OBS_CT
SSF-BETA=BETA
SSF-REF=REF
SSF-ALT=A1
SSF-MAF=A1_FREQ
```
This config file conatins all possible arguments that can be used in
config files. config files use the same argument names as the
commandline arguments but in uppercase, and use "=" instead of a space
between the argument name and the argument being passed.

#### Estimating Linkage Dissequilibrium (LD)
BridgePRS requires individual level genetic data in plink1.9 binary
format to estimate linkage dissequilibrium (LD) in the populations
which produced the GWAS summary statistics. The genotype test and
validation data could be used, i.e. data in these folders
pop_/genotypes/. If these data are small, less than 500 samples, or
are not representative of the GWAS population we provide 1000 Genomes
(1000G) data to estimate LD. Suitable 1000G data for this analysis
is in the 1000G_sample folder
for the small subset of SNPs used in these examples. The .config points to
the folder with reference LD data by the LD_PATH argument in the
config file. LD reference data is
available for the five 1000G *super population* (abbreviations
required to use in brackets): East Asian (EAS), South Asian (SAS),
European (Eur), African (AFR) and American (AMR).

**For real data analyses 1000G reference data for larger subsets of SNPs can be
downloaded here....**

The POP argument simply labels the population used in this .config
file for output. The FST argument is used to specify a prior
distribution used in the BridgePRS analysis and should be the Fst
between the base and target populations used in the analysis. Our
first analysis uses European base data and East Asian target data, the
Fst between these populations is 0.11.

Can you work out what the other arguments are doing?

### BridgePRS output
Open the output summary plot
```
evince  out1/prs-combined_AFR-EUR/bridge.afr-eur.prs-combined.result.pdf
```
on a Mac simply use open instead of evince.

The main result is the barplot at the top which shows the varaince
explained (R2) by the three PRS BridgePRS estimates and the variance
explained by a weighted average of the three model. The weighted model
is BridgePRS best PRS.

This weighted combined PRS should be used. The three
separate PRS estimated by BridgePRS are:
M1. PRS using a prior effect-size distribution from the European Model
   -- stage2
M2. PRS using only the target (Non-European) dataset -- stage1
M3. PRS using both stage1 and stage 2 results
Each of these 3 models are given weights corresponding to how well
they fit the test data. These weights are then used to combine the PRS
to give the single weighted combined PRS.

The separate models M1, M2 and M3 should not be used unless users have a
strong prior belief that the models is better, i.e.
Model M1 reflects the belief that the target population GWAS is only informative in conjugtion with the base population GWAS.

Model M2 reflects the belief that the target population GWAS is informative and the base population GWAS gives no addition information.

Model M3 reflects the belief both the base and target population GWAS contribute independent information.

### Running the BridgePRS pipeline:

#### Task
- Review the contents of the output directory  out_bridge-AFR-single/prs-single_AFRICA and subfolders

#### Questions
4. What evidence can you see that the analysis was successfully executed?

<br><br> 

### BridgePRS Scenario 2:  Prediction into African target data using European and African summary statistics
BridgePRS is most commonly used to combine the power of a smaller ancestry-matched GWAS with a much larger but genetically-distant GWAS population, for the purpose of maximising PRS prediction quality for under-served target populations.

#### Create configuration file for base and target populations
```
bridgePRS check pops -o out_config-EUR-AFR-easyrun --pop AFR EUR --sumstats_prefix data/pop_africa/sumstats/afr.chr data/pop_europe/sumstats/eur.chr --sumstats_suffix .glm.linear.gz .glm.linear.gz --genotype_prefix data/pop_africa/genotypes/afr_genotypes --phenotype_file data/pop_africa/phenotypes/afr_pheno.dat
```
#### Question
5. Carefully check the information given in the on-screen output: (i) How many different .config files have been produced
   and (ii) where are they located?

#### Tasks
- Incorporate the config path information into the code template provided below (for both European and African .config files).
- Based on your-recent understanding of genetic distances between continental populations, choose a sensible    
  value of the --fst parameter to reflect the genetic distance between Africans and Europeans. This extra information will inform the prior   distribution from which posterior effect weights for the target population will be calculated. This needs to be done before attempting to
  run the code given below.

### Multi-ancestry BRIDGEPRS analysis:
Add the missing peices of information to the code below, as you enter it into your terminal.
```
bridgePRS easyrun go -o out_easyrun-EUR-AFR --config_files target.AFR.config base.EUR.config --fst --phenotype y
```

#### Tasks
- After running the above code, navigate to the output directory: ./out_config-EUR-AFR-easyrun to inspect the results.
- Open either of the 2 plots that you see in the directory.

#### Questions
6. In the summary plot, which set of values expresses the correlation between the weights calculated by BridgePRS and
   the beta weights from the initial GWASs?
8. In which output directory will you find precise values of variance explained by the prs-combined-AFR model?
9. What is the variance explained by the prs-combined-AFR model?

### Short Quiz
I have GWAS data and genotype/phenotype data for a cohort consisting of >2000 samples from a small East European population. 
The population LD structure is unique and so I would like this information to be incorporated into my PRS prediction model. I additionally have GWAS and genotype/phenotype data from the UKB biobank that I want to include. How should I formulate the relevant Config files for the BridgePRS analysis?

#### File types
```
ukr/sumstats/ukr.sumstats.out
ukr/genotypes/chr1.bed,bim,fam...chr22.bed,bin,fam,
ukr/phenotypes/ukr_test.dat, ukr/phenotypes/ukr_validation.dat
ukb/sumstats/ukb.sumstats.gz
ukb/genotypes/chr1.bed,bim,fam...chr22.bed,bin,fam
ukb/phenotypes/ukb_test.dat, ukb/phenotypes/ukb_validation.dat
```
#### Answer
```
Target config:
POP=
LDPOP=
SUMSTATS_PREFIX=
GENOTYPE_PREFIX=
PHENOTYPE_FILE=
VALIDATION_FILE=

Model config file:
POP=
LDPOP=
SUMSTATS_PREFIX=
GENOTYPE_PREFIX=
PHENOTYPE_FILE=
VALIDATION_FILE=
```
