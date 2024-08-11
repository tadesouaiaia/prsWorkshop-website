
### Required System Software 
This workshop requires that the following software be installed and tested before the workshop: 


| Name/Link | Description  | Additional Requirements 
| -----------------|:----------:|:----------:|
| 1. [R](https://www.r-project.org/) | Most popular analysis program  in statistical genetics | Yes (Specific Libraries).  
| 2. [Python 3](https://www.python.org/downloads/).  | Multi purpose Programming Language | Yes (The matplotlib library).  


Both popular computer languages, **R** and **python3** will be used during the workshop.  Here we will 
provide a guide for installation of both languages and their associated libraries. 



### 1. R (and associated packages) 
R is the most popular analysis program in statistical genetics, and required for most genetic analysis.  We recommend that MacOS users download an up-to-date (4.4.1)  
version directly from the [R website](https://cloud.r-project.org/) and using the R installer.  If you are having trouble we recommend [this video tutorial](https://www.youtube.com/watch?v=I5WIMX4LK8M). 

While Linux users can also find a suitable version on the website, we recommend using the package installer that comes with Debian based distributions (Debian, Ubuntu, Mint, etc) 
and downloading **R** directly using the following terminal commands: 

    sudo apt install r-base               # to install R 
    sudo apt install build-essential      # to install the essential packages 



!!! warning "Important: Additional R Packages Are Required"
    This workshop requires the following non-standard R packages: **BEDMatrix, boot, data.table, doMC, glmnet, MASS, optparse, parallel, and R.utils**
    These packages can be installed by opening up an **R** session from within the terminal, by typing:  

        R 

    And typing the following command: 
        ```
        install.packages(c("BEDMatrix","boot","data.table","doMC","glmnet","MASS","optparse","parallel","R.utils"))
        ```

After you have finished installation and testing of R, please consider completing our [R-tutorial](tut_R.md) to better familiarize yourself with some basic analysis commands. 




### 2. Python3 (and matplotlib) 
Python3 is a popular multiuse programming language.  This workshop requires Python3 and the matplotlib library.  Any version of Python3 is acceptable 
but a newer version Python3.10.10+ is recommended. 

Many systems come installed with Python3 by default, but it can be downloaded from the python [website](https://www.python.org/downloads/).  The matplotlib package can be found [here](https://matplotlib.org/stable/users/installing/index.html).  

Users of macOs can find detailed instructions on how to install python3 by visiting the following [video link](https://www.youtube.com/watch?v=NmB1AwF3G3k) that we find very helpful.  After installing python3, macOs users can install matplotlib using pip:  

    python3 -m ensurepip 
    sudo python3 -m pip install -U matplotlib 

For Linux we again recommend that you use the package installer to install python3 and matplotlib  

    sudo apt install python3             # to install python3
    python3 --version                    # to verify that it worked
    sudo apt-get install python3-matplotlib # to install matplotlib 


After you have finished with this section, don't forget to complete our [Python tutorial](tut_python.md) to familiarize yourself with some basic commands.  

