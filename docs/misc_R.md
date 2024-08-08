

# Introduction to R

**R** is a useful programming language that allows us to perform a
variety of statis- tical tests and data manipulation. It can also be
used to generate fantastic data visualisations. Here we will go
through some of the basics of **R** so that you can better understand
the practicals throughout the workshop.


## Basics

 If you are not using R Studio then you can type **R** in your terminal
 to run **R** in the terminal.

 ## Adding script to working dir

      cd ~/data/Day1a_Data/Day1a_Data
      wget https://raw.githubusercontent.com/WCSCourses/prs_2023/main/scripts/nagelkerke.R

 

## Working Directory

 When we start **R**, we will be working in a specific folder called
 the **working directory**. We can check the current/working
 directory we are in by typing:

       getwd()

 And we can change our working directory to the **Practical** folder by

       setwd("~/data/Day1a_Data/Day1a_Data")

## Libraries

 Most functionality of **R** is organised in \"packages\" or
 \"libraries\". To access these functions, we will have to install and
 \"load\" these packages. Most commonly used packages are installed
 together with the standard installation process. You can install a new
 library using the install.packages function.

 For example, to install *ggplot2*, run the command:

        install.packages("ggplot2")


 After installation, you can load the library by typing

        library(ggplot2)

 Alternatively, we can import functions (e.g. that we have written)
 from an R script file on our computer. For example, you can load the
 Nagelkerke *R2* function by typing

         source("nagelkerke.R")

 And you are now able to use the Nagelkerke *R2* function (we will use
 this function at the end of this worksheet).

## Variables in R

 You can assign a value or values to any variable you want using \<-.
 e.g

Assign a number to a

      a <- 1

Assign a vector containing a,b,c to v1

      v1 <- c("a", "b","c")

## Functions

 You can perform lots of operations in **R** using diﬀerent built-in R
 functions. Some examples are below:

Assign number of samples

        nsample <- 10000

Generate nsample random normal variable with mean = 0 and sd = 1

        normal <- rnorm(nsample, mean=0,sd=1)

        normal.2 <- rnorm(nsample, mean=0,sd=1)

We can examine the first few entries of the result using head

      head(normal)

And we can obtain the mean and sd using

      mean(normal)

      sd(normal)

We can also calculate the correlation between two variables
 using cor

      cor(normal, normal.2)

## Plotting

 While **R** contains many powerful plotting functions in its base
 packages,customisationn can be diﬃcult (e.g. changing the colour
 scales, arranging the axes). **ggplot2** is a powerful visualization package that provides extensive
 flexibility and customi- sation of plots. As an example, we can do the following

Load the package

      library(ggplot2)

Specify sample size

      nsample <-1000

 Generate random grouping using sample with replacement

     groups <- sample(c("a","b"), nsample, replace=T)

 Now generate the data

     dat <- data.frame(x=rnorm(nsample), y=rnorm(nsample), groups)

 Generate a scatter plot with diﬀerent coloring based on group

     ggplot(dat, aes(x=x,y=y,color=groups))+geom_point()


## Regression Models

 In statistical modelling, regression analyses are a set of statistical
 techniques for estimating the relationships among variables or
 features. We can perform regression analysis in **R**.

 Use the following code to perform linear regression on simulated
 variables "x" and "y":

 Simulate data

     nsample <- 10000

     x <- rnorm(nsample)

     y <- rnorm(nsample)

 Run linear regression

      lm(y~x)

 We can store the result into a variable

       reg <- lm(y~x)

 And get a detailed output using summary

       summary(lm(y~x))

 We can also extract the coeﬃcient of regression using

       reg$coeﬃcient

 And we can obtain the residuals by

      residual <- resid(reg)

 Examine the first few entries of residuals

       head(residual)

 We can also include covariates into the model

      covar <- rnorm(nsample)

      lm(y~x+covar)

 And can even perform interaction analysis

      lm(y~x+covar+x*covar)

 Alternatively, we can use the glm function to perform the regression:

      glm(y~x)

 For binary traits (case controls studies), logistic regression can be
 performed using

 Simulate samples

      nsample <- 10000

      x <- rnorm(nsample)

Simulate binary traits (must be coded with 0 and 1)

      y <- sample(c(0,1), size=nsample, replace=T)

 Perform logistic regression

      glm(y~x, family=binomial)

 Obtain the detailed output

      summary(glm(y~x, family=binomial))

 We will need the NagelkerkeR2 function

 to calculate the pseudo R2 for logistic model

       source("nagelkerke.R")

       reg <- glm(y~x, family=binomial)

 Calculate the Nagelkerke R2 using the NagelkerkeR2 function

         NagelkerkeR2(reg)
