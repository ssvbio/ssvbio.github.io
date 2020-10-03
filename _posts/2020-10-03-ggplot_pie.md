---
layout: post
title: Pie chart with ggplot2 and iris data
categories: iris 'pie chart' CRAN-R ggplot2
---

Here is the code on pie chart with ggplot2 and iris data set. Only Sepal
length variable is used in this tutorial

Load libraries

``` r
library(ggplot2)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

Calculate the sum of the Sepal Lengths

``` r
Sum=sum(iris$Sepal.Length)
```

Now draw pie chart

``` r
iris %>%
    group_by(Species) %>%
    summarise(perc=(sum(Sepal.Length)/Sum)*100) %>%
    ggplot(aes(x="", y=perc, fill=Species)) +
    geom_bar(stat="identity", width=1) +
    theme(legend.position = "None",
          axis.title = element_blank(),
          axis.ticks = element_blank()) +
    geom_text(aes(label = paste0(Species,"\n",round(perc,2),"%")), position = position_stack(vjust = 0.5), size=6) +
    coord_polar("y", start=0)
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

