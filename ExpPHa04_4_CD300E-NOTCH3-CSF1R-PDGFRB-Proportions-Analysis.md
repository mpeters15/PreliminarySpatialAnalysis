``` r
rm(list=ls())
```

### Load packages

``` r
library(readr)
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

### Set working directory

``` r
#setwd("/Volumes/GoogleDrive/My Drive/20_jan_23_spatial_analysis/Halo archive 2020-01-23 19-17 - v3.0.311")
```

### Import data

``` r
df <- read_csv("ExpPHa04_4_CD300E-NOTCH3-CSF1R-PDGFRB.tif_1501_job2985.object_results.csv", 
    col_types = cols(`Object Id` = col_integer(), 
        `Opal 620 Classification` = col_integer(), 
        `Opal 690 Classification` = col_integer()))
```

### Select data of interest and rename columns

``` r
df_620_690 <- data.frame(df$`Object Id`, df$`Opal 620 Classification`, df$`Opal 690 Classification`)
colnames(df_620_690) <- c("Object_ID", "620_Classification", "690_Classification")
```

### Subset mixed data

``` r
mixed_3_3 <- subset(df_620_690, (df_620_690$`620_Classification` == 3) & (df_620_690$`690_Classification` == 3))
mixed_3_4 <- subset(df_620_690, (df_620_690$`620_Classification` == 3) & (df_620_690$`690_Classification` == 4))
mixed_4_3 <- subset(df_620_690, (df_620_690$`620_Classification` == 4) & (df_620_690$`690_Classification` == 3))
mixed_4_4 <- subset(df_620_690, (df_620_690$`620_Classification` == 4) & (df_620_690$`690_Classification` == 4))

mixedFrame <- bind_rows(mixed_3_3, mixed_3_4, mixed_4_3, mixed_4_4)
mixedFrame['Type']='mixed'
mixedTotal <- nrow(mixedFrame)
```

### Subset myeloid data

``` r
myel_3_0 <- subset(df_620_690, (df_620_690$`620_Classification` == 3) & (df_620_690$`690_Classification` == 0))
myel_3_1 <- subset(df_620_690, (df_620_690$`620_Classification` == 3) & (df_620_690$`690_Classification` == 1))
myel_4_0 <- subset(df_620_690, (df_620_690$`620_Classification` == 4) & (df_620_690$`690_Classification` == 0))
myel_4_1 <- subset(df_620_690, (df_620_690$`620_Classification` == 4) & (df_620_690$`690_Classification` == 1))
myel_4_2 <- subset(df_620_690, (df_620_690$`620_Classification` == 4) & (df_620_690$`690_Classification` == 2))

myelFrame <- bind_rows(myel_3_0, myel_3_1, myel_4_0, myel_4_1, myel_4_2)
myelFrame['Type']='myeloid'
myelTotal <- nrow(myelFrame)
```

### Subset fibroblast data

``` r
fibro_0_3 <- subset(df_620_690, (df_620_690$`620_Classification` == 0) & (df_620_690$`690_Classification` == 3))
fibro_0_4 <- subset(df_620_690, (df_620_690$`620_Classification` == 0) & (df_620_690$`690_Classification` == 4))
fibro_1_3 <- subset(df_620_690, (df_620_690$`620_Classification` == 1) & (df_620_690$`690_Classification` == 3))
fibro_1_4 <- subset(df_620_690, (df_620_690$`620_Classification` == 1) & (df_620_690$`690_Classification` == 4))
fibro_2_4 <- subset(df_620_690, (df_620_690$`620_Classification` == 2) & (df_620_690$`690_Classification` == 4))

fibroFrame <- bind_rows(fibro_0_3, fibro_0_4, fibro_1_3, fibro_1_4, fibro_2_4)
fibroFrame['Type']='fibroblast'
fibroTotal <- nrow(fibroFrame)
```

### Subset unclassified data

``` r
unclassified_0_0 <- subset(df_620_690, (df_620_690$`620_Classification` == 0) & (df_620_690$`690_Classification` == 0))
unclassified_0_1 <- subset(df_620_690, (df_620_690$`620_Classification` == 0) & (df_620_690$`690_Classification` == 1))
unclassified_0_2 <- subset(df_620_690, (df_620_690$`620_Classification` == 0) & (df_620_690$`690_Classification` == 2))
unclassified_1_0 <- subset(df_620_690, (df_620_690$`620_Classification` == 1) & (df_620_690$`690_Classification` == 0))
unclassified_1_1 <- subset(df_620_690, (df_620_690$`620_Classification` == 1) & (df_620_690$`690_Classification` == 1))
unclassified_1_2 <- subset(df_620_690, (df_620_690$`620_Classification` == 1) & (df_620_690$`690_Classification` == 2))
unclassified_2_0 <- subset(df_620_690, (df_620_690$`620_Classification` == 2) & (df_620_690$`690_Classification` == 0))
unclassified_2_1 <- subset(df_620_690, (df_620_690$`620_Classification` == 2) & (df_620_690$`690_Classification` == 1))
unclassified_2_2 <- subset(df_620_690, (df_620_690$`620_Classification` == 2) & (df_620_690$`690_Classification` == 2))
unclassified_2_3 <- subset(df_620_690, (df_620_690$`620_Classification` == 2) & (df_620_690$`690_Classification` == 3))
unclassified_3_2 <- subset(df_620_690, (df_620_690$`620_Classification` == 3) & (df_620_690$`690_Classification` == 2))

unclassifiedFrame <- bind_rows(unclassified_0_0, unclassified_0_1, unclassified_0_2, unclassified_1_0, unclassified_1_1, unclassified_1_2, unclassified_2_1)
unclassifiedFrame['Type']='unclassified'
unclassifiedTotal <- nrow(unclassifiedFrame)
```

### Summary results

Total number of myeloid cells: 746

Total number of fibroblast cells: 3080

Total number of mixed cells: 382

Total number of unclassified cells: 12013

Total number of subsetted lists: 16

### Plot data

``` r
cellCountsOveralldf <- bind_rows(myelFrame, fibroFrame, mixedFrame)

cellCountsOverall <- data.frame(
    type=c("Myeloid", "Fibroblast", "Mixed", "Unclassified"),
    count=c(myelTotal, fibroTotal, mixedTotal, unclassifiedTotal)
    )
rownames(cellCountsOverall) <- c("Myeloid", "Fibroblast", "Mixed", "Unclassified")

drops <- c("Unclassified")
cellCountsNoUnclassified <- cellCountsOverall[!(row.names(cellCountsOverall) %in% drops), ]
```

``` r
lbls <- cellCountsOverall$type
pct <- round(cellCountsOverall$count/sum(cellCountsOverall$count)*100)
lbls <- paste(lbls, pct, sep = ", ") # add percents to labels
lbls <- paste(lbls,"%",sep="") # add % to labels
pie(cellCountsOverall$count, labels = lbls, main = "Overall Cell Counts")
```

![](ExpPHa04_4_CD300E-NOTCH3-CSF1R-PDGFRB-Proportions-Analysis_files/figure-markdown_github/unnamed-chunk-11-1.png)

``` r
lbls <- cellCountsNoUnclassified$type
pct <- round(cellCountsNoUnclassified$count/sum(cellCountsNoUnclassified$count)*100)
lbls <- paste(lbls, pct, sep = ", ") # add percents to labels
lbls <- paste(lbls,"%",sep="") # add % to labels
pie(cellCountsNoUnclassified$count, labels = lbls, main = "Cell Counts (Ignoring Unclassified)")
```

![](ExpPHa04_4_CD300E-NOTCH3-CSF1R-PDGFRB-Proportions-Analysis_files/figure-markdown_github/unnamed-chunk-12-1.png)
