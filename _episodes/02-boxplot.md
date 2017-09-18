---
title: "Kick the Bar Chart Habit: Boxplots"
teaching: 60
exercises: 30
questions:
- "What are the limitations of bar charts?"
- "What information do boxplots display?"
- "How can I subset data ?"
objectives:
- "Load data from a URL."
- "Describe the anatomy of a box plot."
- "Use box plots to compare mean and median values between groups."
- "Order groups by mean value."
- "Subset data."
- "Describe some pitfalls of using bar charts to display or compare means."
keypoints:
- ""
source: Rmd
---





## Preliminaries
Bar charts are useful for displaying count data but are often used to portray statistical information that they don't represent well. In this lesson we'll learn to ['Kick the bar chart habit'](http://www.nature.com/nmeth/journal/v11/n2/full/nmeth.2837.html) by creating box plots as an alternative to bar charts. This lesson uses data from a multi-system survey of mouse physiology in 8 inbred founder strains and 54 F1 hybrids of the Collaborative Cross. The study is described in [Lenarcic et al, 2012](http://www.genetics.org/content/190/2/413.full). For more information about this data set, see the [CGDpheno3 data](http://phenome.jax.org/projects/CGDpheno3) at Mouse Phenome Database. 

#### Load package and library
Load the ggplot library. You'll need to install the packages
first if you haven't done so already. Install them from the `Packages` tab in RStudio, or use the `install.packages()` command in the Console. Use double quotes around the package name.


~~~
install.packages("ggplot2")
~~~
{: .r}

You only need to install a package once to download it into your machine's library. Once you have installed the package on your machine, you need to load the library into R in order to use the functions contained in the package.  


~~~
library(ggplot2)
~~~
{: .r}

#### Load and explore data
Load the data from this shortened URL. Mind the double quotes.  


~~~
cc_data <- read.csv(file = "http://bit.ly/CGDPheno3")
~~~
{: .r}

Explore the data variables. The first 4 columns contain strain, sex, and ID numbers. The remaining contain phenotype measurements with abbreviated names.


~~~
names(cc_data)
~~~
{: .r}



~~~
 [1] "strain"          "sex"             "id"             
 [4] "mouse_num"       "CHOL"            "HDL"            
 [7] "GLU"             "TG"              "WBC"            
[10] "pctLYMP"         "pctNEUT"         "pctMONO"        
[13] "pctBASO"         "pctEOS"          "pctLUC"         
[16] "RBC"             "pctRetic"        "RDW"            
[19] "MCH"             "MCHC"            "CHCM"           
[22] "HDW"             "MCV"             "cHGB"           
[25] "mHGB"            "HCT"             "PLT"            
[28] "MPV"             "pulse"           "pulse_std"      
[31] "systolic_BP"     "systolic_BP_std" "CV"             
[34] "HR"              "HRV"             "R_amplitude"    
[37] "RS_amplitude"    "N"               "PQ"             
[40] "PR"              "QRS"             "QT"             
[43] "QT_dispersion"   "QTc"             "QTc_dispersion" 
[46] "RR"              "ST"              "BMC"            
[49] "BMD"             "bone_area"       "total_area"     
[52] "LTM"             "pct_fat"         "TTM"            
[55] "bw"             
~~~
{: .output}

How many mice? 

~~~
dim(cc_data)
~~~
{: .r}



~~~
[1] 642  55
~~~
{: .output}













































