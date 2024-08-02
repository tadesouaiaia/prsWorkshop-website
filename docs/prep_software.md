[//]: ![Screenshot](img/sib1.jpg)

# Preparation 

## Checklist: 

### System Requirements
This workshop requires that the following software is installed and tested before the workshop: 


| Name/Link | Description  | Additional Requirements 
| -----------------|:----------:|:----------:|
| 1. [R v4.1+](https://www.r-project.org/) | Most popular analysis program  in statistical genetics | Yes (Specific Libraries).  
| 2. [Python 3](https://www.python.org/downloads/).  | Multi purpose Programming Language | Yes (The matplotlib library).  
| 3. [Plink 1.9+](https://www.cog-genomics.org/software) | Popular open-source whole genome association analysis toolset | No 



| Name/Link | Description  | Additional Requirements 
| -----------------|:----------:|:----------:|
| 1. [PRSice](https://choishingwan.github.io/PRSice/)  | Single ancestry polygenic risk score software | No 
| 2. [PRS-CS](https://github.com/getian107/PRScs)  | Single ancestry polygenic risk score software | No
| 3. [bridgePRS](www.bridgePRS.net)  | Multi-ancestry polygenic risk score software | No 
| 4. [PRS-CSx](https://github.com/getian107/PRScs)  | Multi-ancestry polygenic risk score software | No




### 1. R (v4.1+) 
R is the most popular analysis program in statistical genetics, and is required by PRSice, BridgePRS, and PRESet. An up-to-date version can be downloaded from the [R website](https://www.r-project.org/).  

Additionally, the following packages need to be installed from an R-terminal:  **BEDMatrix, boot, data.table, doMC, glmnet, MASS, optparse, parallel, and R.utils**

!!! tips "R Packages"
    These packages can be installed from inside an R terminal using the command:
        ```
        $ R
        install.packages(c("BEDMatrix","boot","data.table","doMC","glmnet","MASS","optparse","parallel","R.utils"))
        ```

### 2. Python 3 
Python is a very popular multi use programming language.  Many systems come installed with Python by default, 
but it can be downloaded from python [website](https://www.python.org/downloads/).  The matplotlib package is required to make plots and can be downloaded [here](https://matplotlib.org/stable/users/installing/index.html).
The following Python packages are required by BridgePRS and PRS-CS/PRS-CSx.....and can be installed by....


### 3. PLINK 1.9+
PLINK is a free, open-source whole genome association
analysis toolset, designed to perform a range of basic, large-scale analyses
in a computationally efficient manner. PLINK executable can be downloaded
from [website](https://www.cog-genomics.org/software).


!!! warning "Extra MacOs Security:"
    MacOs often block executables if they are not approved from the app store.
    You may have to change your settings to allow Plink to be called
    For instructions on how to do so, please click [here](req_mac.md).

### 4. PRScise 
Polygenic Risk Score software for calculating, applying, evaluat-
ing and plotting the results of polygenic risk score analyses (www.PRSice.info).
You can directly download the executable from here
https://choishingwan.github.io/PRSice/ and have a go at running PRSice
by following the instructions (see Quick Start guide) if you haven’t done
before.


### 5. bridgePRS
You need this.  



<!--
### 1. R (v4.1+) 
R is the most popular analysis program in statistical genetics, and is required by PRSice, BridgePRS, and PRESet. An up-to-date version can be downloaded from the 
[R website](https://www.r-project.org/).  

| Operating System | Link | Notes | 
| -----------------|:----------:|:----:| 
| Linux  64-bit | [v1.0.2](https://github.com/clivehoggart/BridgePRS/archive/refs/heads/main.zip) | Updated 7-12-2024 |  
| Mac  64-bit   | [v1.0.2](https://github.com/clivehoggart/BridgePRS/archive/refs/heads/main.zip) | Updated 6-27-2024 | 
| Windows       | NA     | Not Available | 

# Reference Panels 
| Source | Link | Notes | 
| -----------------|:----------:|:----:| 
| HapMap | [v1.0.2](https://github.com/clivehoggart/BridgePRS/archive/refs/heads/main.zip) | Updated 7-12-2024 |  
| 1000G_   | [v1.0.2](https://github.com/clivehoggart/BridgePRS/archive/refs/heads/main.zip) | Updated 6-27-2024 | 
| Windows       | NA     | Not Available | 
| 1000G Ref Panel | [1000G_ref.tar.gz](https://drive.google.com/file/d/1djAEwRiQsh4veinSLHO3laGjNF95vvN9/view?usp=drive_link) | Optional (Unzip into data directory to use) |    
| 1000G Ref Panel | [1000G_ref.tar.gz](https://drive.google.com/file/d/1djAEwRiQsh4veinSLHO3laGjNF95vvN9/view?usp=drive_link) | Optional (Unzip into data directory to use) |    
-->








<!--

|BridgePRS Packages |Reference Panels|
|--|--|
|<table> <tr><th> OS </th><th> Link </th><th> Last Update  </th></tr>  <tr><td> Linux 64-Bit </td><td> [v1.0.3](https://github.com/clivehoggart/BridgePRS/archive/refs/heads/main.zip) </td><td> 7-12-2024 </td></tr>  </th></tr>  <tr><td> Mac 64-Bit </td><td> [v1.0.3](https://github.com/clivehoggart/BridgePRS/archive/refs/heads/main.zip) </td><td> 6-16-2024 </td></tr> </th></tr>  <tr><td> Windows </td><td> NA </td><td> Not Available </td></tr> </table> | <table> <tr><th> Download Link  </th><th> Size </th><th> More Information </th></tr><tr><td> [HapMap Variants](https://drive.google.com/file/d/1EGFap5wjKxIT42SWHKr9MUOzVnK9CVew/view?usp=drive_link) </td><td> <1GB </td><td> [International HapMap Project](https://www.genome.gov/10001688/international-hapmap-project) </td></tr>  </th></tr>  <tr><td> [1000 Genomes Variants: MAF>5%](https://drive.google.com/file/d/1rmxKcTGF8XTYU0E7jAIkKsCeedGMNwDE/view?usp=drive_link) </td><td> 8GB </td><td> [International Genome Sample Resource](https://www.internationalgenome.org/) </td></tr> </th></tr>  </td><td> [1000 Genomes variants: MAF>1%](https://drive.google.com/file/d/1RuC8J_qJLDSLnQ4uOGuxx9uGSWg5fxKn/view?usp=drive_link) </td><td> 14GB </td><td> [International Genome Sample Resource](https://www.internationalgenome.org/) </td></tr> </table>

-->

<!--
|BridgePRS Package |Reference Panels|
|--|--|
|<table> <tr><th> OS </th><th> Link </th><th> Last Update  </th></tr>  <tr><td> Linux 64-Bit 
</td><td> [v1.0.3](https://github.com/clivehoggart/BridgePRS/archive/refs/heads/main.zip) 
</td><td> 7-12-2024 </td></tr>  </th></tr>  <tr><td> Mac 64-Bit 
</td><td> [v1.0.3](https://github.com/clivehoggart/BridgePRS/archive/refs/heads/main.zip) 
</td><td> 6-16-2024 
</td></tr> </th></tr>  <tr><td> Windows </td><td> NA </td><td> Not Available </td></tr> </table>       
| <table> <tr><th>Source</th><th>Link  </th><th> Notes </th></tr><tr><td> HapMap </td><td> [v1.0.3](https://github.com/clivehoggart/BridgePRS/archive/refs/heads/main.zip) 
</td><td> Cool </td></tr>  </th></tr>  <tr><td> 1k Genomes </td><td> [v1.0.3](https://github.com/clivehoggart/BridgePRS/archive/refs/heads/main.zip) 
</td><td> Cool </td></tr> </th></tr>  <tr><td> Windows </td><td> [v1.0.03](www.google.com) 
</td><td> Cool </td></tr> </table>     
-->



