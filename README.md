# iR-by-examples
Learning R by examples

## Basics

##### get and set working directory
```r
getwd()
setwd("/Users/Natsume/Documents/1learnD3/d3-template/")
```

##### check files inside a folder
```r
dir()
```

##### read csv files
```r
hkws <- read.csv("hkws.csv")

?read.csv # check use of different parameters

hkws <- read.csv("hkws.csv", header=F)

hkws %>% head()
```


##### Load libraries
```r
library(dplyr)
```


##### add or change column names
```r
colnames(hkws) <- c("Date", "Open", "High", "Low", "Close", "Volume", "Cash")
```


##### write a csv file
```r
write.csv(hkws, file = "/Users/Natsume/Documents/1learnD3/d3-template/hkwsName.csv")
```
