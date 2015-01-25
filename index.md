---
title       : Residual Life Expectancy
subtitle    : How much longer am I expected to live?
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
--- bg:#EDE0CF

## Residual Life Expectancy
 
The world's population is continuing to age. According to the 
United Nations' Department of Economic and Social Affairs, 
the global share of people aged 60 years or more increased from 
9.2% in 1990 to 11.7% in 2013 and is expected to grow to 21.1% 
by 2050. [1]

Statistics compiled by the World Heath Organization 
show that the global average life expectancy at birth 
has risen from 64 years in 1990 to 70 years in 2012. [2]
As individuals, therefore, it is becoming increasingly important 
to understand how much longer we can be expected to live and the 
various lifestyle factors that can influence our lifespan. 

For this project, we have developed a convenient Shiny application that 
can predict people's residual life expectancy (RLE) based on where 
they live, their sex, and their age. Optionally, the application 
can also provide a more customized response based on key personal 
lifestyle risk factors as identified by a recent cohort study. [3]


^[1] ^http://www.un.org/en/development/desa/population/publications/pdf/ageing/WorldPopulationAgeing2013.pdf<br>
^[2] ^http://apps.who.int/iris/bitstream/10665/112738/1/9789240692671_eng.pdf?ua=1<br>
^[3] ^http://www.biomedcentral.com/1741-7015/12/59

--- bg:#EDE0CF

To develop the application, the relevant 2012 
data was downloaded from the World Health Organization's (WHO's) 
data repository. [4] From it, a tidy data frame was created
and saved. [5]


```r
library(dplyr)
regions = c("AFR", "EMR", "EUR", "AMR", "SEAR", "WPR")
prefix <- "http://apps.who.int/gho/athena/data/data-text.csv?target=GHO/"
ev <- "LIFE_0000000035"
suffix <- "&profile=text&filter=COUNTRY:-;YEAR:2012;SEX:FMLE;SEX:MLE;REGION:"
ev_df <- do.call(rbind, lapply(regions, function(region) {
        url <- paste(prefix, ev, suffix, region, sep="") 
        data <- distinct(select(read.csv(url), WHO.region, Sex, Age.Group, Numeric))
        return(data)}))
ages <- c("<1", "1-4", "5-9", "10-14", "15-19", "20-24", "25-29", "30-34", 
    "35-39", "40-44", "45-49", "50-54", "55-59", "60-64", "65-69", "70-74", 
    "75-79", "80-84", "85-89", "90-94", "95-99", "100+")
ev_df$Age.Group <- factor(ev_df$Age.Group, levels=ages)
ev_df <- arrange(ev_df, WHO.region, Sex, Age.Group)
saveRDS(ev_df, file = "./ddp_project/data/ev.rds")
```

^[4] ^http://apps.who.int/gho/data/?theme=main<br>
^[5] ^http://github.com/dhashman/developing_data_products/blob/master/ev.R

--- bg:#EDE0CF

As can be seen by the graph below, people's base RLE 
varies depending on what region of the world they live in, 
their sex, and their current age. Click on the interactive 
graph to see specific RLE values by region, sex, and age group.  
<br>

<iframe src=' assets/fig/plot1-1.html ' scrolling='no' frameBorder='0' seamless class='rChart dimple ' id=iframe- chart3acc220022c3 ></iframe> <style>iframe.rChart{ width: 100%; height: 400px;}</style>

--- bg:#EDE0CF

<style type="text/css">
img {     
  max-height: 560px;     
  max-width: 964px; 
}
</style>

Key personal lifestyle risk factors that can influence your 
RLE include whether you smoke or have smoked, your body 
mass index (BMI), and, on average, how much alcohol you drink 
and how much processed/red meat you eat on a daily basis. 
The web application looks like:  
<br>

<img src="RLE_application.JPG">

Go to https://dhashman.shinyapps.io/ddp_project/ and 
try it! It may help you to live longer.

