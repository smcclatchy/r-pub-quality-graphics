---
title: "Time Series Plots"
teaching: 60
exercises: 30
questions:
- "How can I work with dates to create a time series plot?"
- "How can I fine-tune a plot by adjusting the scale, breaks, axis labels, and plot annotations?"
objectives:
- "Load data from a URL."
- "Convert date objects and formats."
- "Log-transform the scale of a plot."
- "Specify breaks in the x and y axes."
- "Use plot annotations and custom axis labels."
- "Save plot as a PDF."
keypoints:
- ""
source: Rmd
---



## Preliminaries
Time series plots are valuable but can be tricky to create because date and time formats in computing are not straightforward. Here we'll recreate the plot showing growth in the [Sequence Read Archive](http://www.ncbi.nlm.nih.gov/Traces/sra/) from 2008 to present.

#### Load packages and libraries
Load the ggplot and scales libraries. You'll need to install the packages
first if you haven't done so already. Install them from the Packages tab,
or use the install.packages() command. Use double quotes around the package
name.

~~~
install.packages("ggplot2")
install.packages("scales")
~~~
{: .r}
You only need to install a package once to download it into your machine's library. Once you have installed the package on your machine, you need to load the library in order to use the functions contained in the package.

~~~
library(ggplot2)
library(scales)
~~~
{: .r}
When you load a library you'll get a warning message indicating the R version in which the library was built. If it's different from the R version that you're running, you might occasionally run into problems depending on the library and the functions it contains. To find out what version of R you have, type


~~~
version
~~~
{: .r}



~~~
               _                           
platform       x86_64-apple-darwin15.6.0   
arch           x86_64                      
os             darwin15.6.0                
system         x86_64, darwin15.6.0        
status                                     
major          3                           
minor          4.1                         
year           2017                        
month          06                          
day            30                          
svn rev        72865                       
language       R                           
version.string R version 3.4.1 (2017-06-30)
nickname       Single Candle               
~~~
{: .output}

The version of R is given as version.string, followed by the nickname for the version.

#### Load data and explore
Read in the [Sequence Read Archive](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?) database growth file  and view structure.


~~~
sra <- read.csv("https://www.ncbi.nlm.nih.gov/Traces/sra/sra_stat.cgi")
str(sra)
~~~
{: .r}



~~~
'data.frame':	3174 obs. of  5 variables:
 $ date             : Factor w/ 3174 levels "01/01/2011","01/01/2012",..: 1338 790 798 833 887 906 914 923 942 961 ...
 $ bases            : num  2.03e+10 3.98e+10 4.14e+10 4.18e+10 4.19e+10 ...
 $ open_access_bases: num  2.03e+10 3.98e+10 4.14e+10 4.18e+10 4.19e+10 ...
 $ bytes            : num  5.05e+10 9.86e+10 1.03e+11 1.04e+11 1.04e+11 ...
 $ open_access_bytes: num  5.05e+10 9.86e+10 1.03e+11 1.04e+11 1.04e+11 ...
~~~
{: .output}

The data have 3174 rows and 5 columns. The first column is listed as a factor when in fact it is a date in the MM/DD/YYYY format. A factor is a categorical variable (i.e. red, green, blue, or low, middle, and high-income). Date variables are a data type that includes month, day and year, and that have their own specific functions to extract weekdays or count the number of days until an event, for example. To place dates on the x-axis in proper order, convert the first column to a date object in the YYYY-MM-DD format. First check to make sure that the first several dates will be converted correctly.


~~~
class(sra$date)
~~~
{: .r}



~~~
[1] "factor"
~~~
{: .output}



~~~
class(as.Date(sra$date, format = "%m/%d/%Y"))
~~~
{: .r}



~~~
[1] "Date"
~~~
{: .output}



~~~
head(sra$date)
~~~
{: .r}



~~~
[1] 06/05/2007 04/04/2008 04/05/2008 04/09/2008 04/15/2008 04/17/2008
3174 Levels: 01/01/2011 01/01/2012 01/01/2013 01/01/2014 ... 12/31/2016
~~~
{: .output}



~~~
head(as.Date(sra$date, format = "%m/%d/%Y"))
~~~
{: .r}



~~~
[1] "2007-06-05" "2008-04-04" "2008-04-05" "2008-04-09" "2008-04-15"
[6] "2008-04-17"
~~~
{: .output}

#### Convert date format
The first several dates convert correctly from MM/DD/YYYY to YYYY-MM-DD format, which is the default date format in R and the International Standards Organization (ISO) standard date format. Now convert all dates to the new format, and convert them from a factor variable to a date variable.


~~~
sra$date <- as.Date(sra$date, format = "%m/%d/%Y")
class(sra$date)
~~~
{: .r}



~~~
[1] "Date"
~~~
{: .output}



~~~
head(sra$date)
~~~
{: .r}



~~~
[1] "2007-06-05" "2008-04-04" "2008-04-05" "2008-04-09" "2008-04-15"
[6] "2008-04-17"
~~~
{: .output}



~~~
str(sra)
~~~
{: .r}



~~~
'data.frame':	3174 obs. of  5 variables:
 $ date             : Date, format: "2007-06-05" "2008-04-04" ...
 $ bases            : num  2.03e+10 3.98e+10 4.14e+10 4.18e+10 4.19e+10 ...
 $ open_access_bases: num  2.03e+10 3.98e+10 4.14e+10 4.18e+10 4.19e+10 ...
 $ bytes            : num  5.05e+10 9.86e+10 1.03e+11 1.04e+11 1.04e+11 ...
 $ open_access_bytes: num  5.05e+10 9.86e+10 1.03e+11 1.04e+11 1.04e+11 ...
~~~
{: .output}

## Plotting with [ggplot](http://ggplot2.org/)
Plot the number of bases and open access bases in SRA by layering lines over the time series on the x-axis. Use the ggplot() function, which is found in the ggplot2 library. Quick reminder of ggplot syntax: ggplot(data, mapping) + layer(). 
Start by plotting bases alone on the y-axis.

~~~
ggplot(data = sra, mapping = aes(x = date)) + 
  geom_line(aes(y = bases))
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />

Can you identify the ggplot syntax in the code above? Remember: ggplot(data, mapping) + layer(). The layer is a line.
Now add a second layer, a line representing growth of open access bases. Use the up arrow on your keyboard to retrieve the last bit of code you wrote, and add onto that.

~~~
ggplot(data = sra, mapping = aes(x = date)) + 
  geom_line(aes(y = bases)) + 
  geom_line(aes(y = open_access_bases))
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" style="display: block; margin: auto;" />

Change the line colors and sizes. Use the up arrow to retrieve the last code you ran, and add onto it rather than typing anew. More typing = more pain and suffering.


~~~
ggplot(data = sra, mapping = aes(x = date)) + 
  geom_line(aes(y = bases), colour = "blue", size = 1.5) + 
  geom_line(aes(y = open_access_bases), colour = "yellow", size = 1.5)
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-10-1.png" title="plot of chunk unnamed-chunk-10" alt="plot of chunk unnamed-chunk-10" style="display: block; margin: auto;" />

The plot seems to show zero bases until the year 2010. Check the smallest number of total bases and open access bases in the data. Are the minimum numbers of bases both zero?


~~~
min(sra$bases)
~~~
{: .r}



~~~
[1] 20304190150
~~~
{: .output}



~~~
min(sra$open_access_bases)
~~~
{: .r}



~~~
[1] 20304190150
~~~
{: .output}



~~~
max(sra$bases)
~~~
{: .r}



~~~
[1] 1.319109e+16
~~~
{: .output}



~~~
max(sra$open_access_bases)
~~~
{: .r}



~~~
[1] 5.428109e+15
~~~
{: .output}

#### Log-transform the y-axis
The smallest number of bases is 2.03e+10, and the largest number is 1.32e+16. The smallest number of open access bases is 2.03e+10, and the largest number is 5.43e+15. We need to transform the y axis to logarithmic for accurate display, so that the plot doesn't show values of zero that don't exist.

~~~
ggplot(data = sra, mapping = aes(x = date)) + 
  geom_line(aes(y = bases), colour = "blue", size = 1.5) + 
  geom_line(aes(y = open_access_bases), colour = "yellow", size = 1.5) +
  scale_y_log10()
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-12-1.png" title="plot of chunk unnamed-chunk-12" alt="plot of chunk unnamed-chunk-12" style="display: block; margin: auto;" />

#### Specify axis breaks and labels
Now manually define the y axis breaks so that each order of magnitude is represented.

~~~
ggplot(data = sra, mapping = aes(x = date)) + 
  geom_line(aes(y = bases), colour = "blue", size = 1.5) + 
  geom_line(aes(y = open_access_bases), colour = "yellow", size = 1.5) +
  scale_y_log10(breaks=c(1e+10, 1e+11, 1e+12, 1e+13, 1e+14, 1e+15))
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-13-1.png" title="plot of chunk unnamed-chunk-13" alt="plot of chunk unnamed-chunk-13" style="display: block; margin: auto;" />

Supply simpler superscripted exponents on the y axis labels. These are easier to read and interpret than are combinations of digits, plus signs, and the letter e.

~~~
ggplot(data = sra, mapping = aes(x = date)) + 
  geom_line(aes(y = bases), colour = "blue", size = 1.5) + 
  geom_line(aes(y = open_access_bases), colour = "yellow", size = 1.5) +
  scale_y_log10(breaks=c(1e+10, 1e+11, 1e+12, 1e+13, 1e+14, 1e+15),
                labels = expression("10"^"10", "10"^"11", "10"^"12",
                                    "10"^"13", "10"^"14", "10"^"15"))
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-14-1.png" title="plot of chunk unnamed-chunk-14" alt="plot of chunk unnamed-chunk-14" style="display: block; margin: auto;" />

Compare to the plot at the [Sequence Read Archive](http://www.ncbi.nlm.nih.gov/Traces/sra/).
Notice the difference in x and y axis labels and starting points. Our plot starts in the year 2000 with 2.03e+10 bases. The [Sequence Read Archive](http://www.ncbi.nlm.nih.gov/Traces/sra/) plot starts before the year 2009 with 10^12^ bases.

How many data points are there in each year?


~~~
table(format(sra$date, "%Y"))
~~~
{: .r}



~~~

2007 2008 2009 2010 2011 2012 2013 2014 2015 2016 2017 
   1  149  271  332  340  364  363  365  365  366  258 
~~~
{: .output}

Remove the single measurements in years 2000 and 2007. These two data points extend 
the line backward in time and give misleading information about the actual growth in the number of bases by making it appear as if data collection was sequential year-over-year. In fact, there were many years missing data altogether. Start at year 2008, the first year of sustained,  consecutive data collection.


~~~
which(format(sra$date, "%Y") %in% c("2000", "2007"))
~~~
{: .r}



~~~
[1] 1
~~~
{: .output}



~~~
sra <- sra[-(1:2),]
table(format(sra$date, "%Y"))
~~~
{: .r}



~~~

2008 2009 2010 2011 2012 2013 2014 2015 2016 2017 
 148  271  332  340  364  363  365  365  366  258 
~~~
{: .output}

Save the plot to a variable now that we've removed the two data points. To view the plot, type the name of the variable.

~~~
logplot <- ggplot(data = sra, mapping = aes(x = date)) +
  geom_line(aes(y = bases), colour = "blue", size = 1.5) + 
  geom_line(aes(y = open_access_bases), colour = "yellow", size = 1.5) +
  scale_y_log10(breaks=c(1e+10, 1e+11, 1e+12, 1e+13, 1e+14, 1e+15),
                labels = expression("10"^"10", "10"^"11", "10"^"12", "10"^"13", "10"^"14",
                                    "10"^"15"))
logplot
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-17-1.png" title="plot of chunk unnamed-chunk-17" alt="plot of chunk unnamed-chunk-17" style="display: block; margin: auto;" />

Specify the breaks in the x-axis so that each year is shown. Redefine the variable
logplot with these changes.

~~~
logplot <- logplot + scale_x_date(labels = date_format("%Y"), breaks = date_breaks("years"))
logplot
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" style="display: block; margin: auto;" />

#### Add text annotations, axis labels, and title
Add text annotations to the plot to label the lines for total and open access bases.

~~~
logplot <- logplot + 
  annotate("text", x=sra$date[1100], y=1.6e+15, label="total bases") + 
  annotate("text", x=sra$date[1700], y=1.25e+14, label="open access bases")
logplot
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-19-1.png" title="plot of chunk unnamed-chunk-19" alt="plot of chunk unnamed-chunk-19" style="display: block; margin: auto;" />

Add axis labels.

~~~
logplot <- logplot + 
  xlab("Year") + 
  ylab("Number of bases")
logplot
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-20-1.png" title="plot of chunk unnamed-chunk-20" alt="plot of chunk unnamed-chunk-20" style="display: block; margin: auto;" />

Add a title. Redefine logplot each time to save the changes.

~~~
logplot <- logplot + 
  ggtitle("Sequence Read Archive Database Growth")
logplot
~~~
{: .r}

<img src="../fig/rmd-03-unnamed-chunk-21-1.png" title="plot of chunk unnamed-chunk-21" alt="plot of chunk unnamed-chunk-21" style="display: block; margin: auto;" />

#### Save the plot as a PDF file
Save the plot as a PDF using the pdf() command. You can also save as a png(), jpeg(), tiff(), or bmp() using the corresponding command. Provide a file name inside the parentheses surrounded by double quotes. Be sure to turn the graphics device off afterward to return graphics output to your plot window in RStudio. Use dev.off() to turn off the graphics device.

~~~
pdf(file = "SRA-database-growth.pdf")
print(logplot)
dev.off()
~~~
{: .r}



~~~
quartz_off_screen 
                2 
~~~
{: .output}

Now locate the file on your machine and open it. Compare with the plot at the [Sequence Read Archive](http://www.ncbi.nlm.nih.gov/Traces/sra/).

> Code Challenge: Plot bytes and open access bytes. Change to log scale. Color the lines, add text annotations, label the axes and add a title. Save the plot to a variable. Print the variable to a file and open the file to view your plot. 
