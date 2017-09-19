---
title: "Scatterplots"
teaching: 60
exercises: 30
questions:
- "How can I visualize the relationship between two variables?"
- "How can I compare scatterplots between sexes?"
- "How can I add a regression line to a scatterplot?"
objectives:
- "Load data from a URL."
- "Add a regression line to a scatterplot."
- "View sexes in separate scatterplots"
- "Order groups by mean value."
- "Subset data."
- "Describe some pitfalls of using bar charts to display or compare means."
- "Specify dimensions and save a plot as a PDF or PNG file."

keypoints:
- ""
source: Rmd
---



## Preliminaries
Scatterplots are simple ways to visualize data. We can explore the relationship between data variables by scatterplotting one against the other and by adding regression lines to the plots. We'll use baseline survey data for the 8 inbred Collaborative Cross founder strains and 54 F1 hybrids for this purpose. Measurements include blood, cardiovascular, bone, body size, weight, and composition. For more information about this data set, see the [CGDpheno3 data](http://phenome.jax.org/db/q?rtn=projects/details&id=439) at Mouse Phenome Database.

Load the ggplot library and the data.


~~~
library(ggplot2)
cc_data <- read.csv(file="http://bit.ly/CGDpheno3")
~~~
{: .r}



~~~
Error in file(file, "rt"): cannot open the connection to 'http://bit.ly/CGDpheno3'
~~~
{: .error}




















































