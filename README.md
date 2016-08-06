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


##### write a csv file, without adding row names
```r
write.csv(hkws, file = "/Users/Natsume/Documents/1learnD3/d3-template/hkwsName.csv", row.names = F)
```

#### add a new column to a dataframe
```r
origin <- hkws$Close[1]
hkws %>% mutate(return = round(Close/origin,2)) %>% select(return) %>% head()
hkws <- hkws %>% mutate(return = round(Close/origin,2))
```

#### transmute: create a column on its own out of df
```r
hkws_return <- hkws %>% transmute(return = round(Close/origin,2))
```

#### gsub: "2016/08/05" => "20160805"
```r
hkws_d3 <- hkws

hkws_d3$Date <- gsub("/", "", hkws$Date) %>% as.numeric()
```

#### read.table: read tsv, keep string not factorize
```r
dist_hash_key <- read.table("/Users/Natsume/Downloads/DiTechData1/season_1/training_data/cluster_map/cluster_map", sep = "\t", stringsAsFactors = F)

colnames(dist_hash_key) <- c("Hash", "ID")

dist_hash_key %>% glimpse()
```

#### compare two df equal or not
```r
mean(dist_hash_key == dist_hash_key_test)
```


#### save df into .RData file
```r
save(dist_hash_key, dist_hash_key_test, file = "/Users/Natsume/Downloads/DiTechCleanData/hash_key_train_test.RData")
```

#### read two lines of a file
```r
readLines("/Users/Natsume/Downloads/DiTechData1/season_1/training_data/order_data/order_data_2016-01-01", 2)
```

#### get all files' full-path-names of a directory
```r
setwd("/Users/Natsume/Downloads/DiTechData1/season_1/training_data/") # order_data is a sub-folder name
infiles <- dir("order_data", full.names = TRUE)
infiles %>% length()
infiles %>% glimpse()
infiles %>% print()
```

#### apply func to every element of array, return a list
```r
order_data <- lapply(infiles, function(x) read.table(x, sep = "\t", stringsAsFactors = F))
order_data[[2]] %>% glimpse()#class()
```

#### rbind: row-bind 2 dataframe
```r
z = c(1,2,3)
x = c(4,5,6)
c <- rbind(z, x)
```


#### Reduce + rbind: row-bind more dataframe in a list
```r
a <- list(z = c(1,2,3), x=c(4,5,6))
d <- rbind(a$z, a$x)
temp <- Reduce(rbind, a)
temp %>% str()
temp %>% dim()
```

#### as.xts(orderby = ): dataframe => xts
```r
if_day <- read.csv("if_day_df.csv", stringsAsFactors = F)

# order.by = must be Date class or POXSIX.ct or timeDate
if_day %>% select(IF_DAY.Open:IF_DAY.Volume) %>% xts(order.by = as.Date(if_day$IF_DAY.Date))  %>% glimpse()

if_min <- read.csv("IFhot-1min.csv")
if_min <- if_min %>% select(Open:TotalVolume) %>% as.xts(order.by = as.POSIXct(if_min$Date))
```

#### merge(xts, xts): many stock's returns into one table
```r
# ? merge.xts --> details worth reading
(x <- xts(4:10, Sys.Date()+4:10))
(y <- xts(1:6, Sys.Date()+1:6))
(z <- xts(8:15, Sys.Date()+8:15))
merge(x,y) # keep all data and making rest NA
merge(x,y,z) # keep all data and making rest NA
merge(x,y, join='inner') # keep only intersection
merge(x,y, join='left') # keep all left, leaving right NA
merge(x,y, join='right') # keep all right, leaving left NA
```

#### merge 3 or more xts
```r
full <- merge(shls_return, ssgf_return, jqr_return )
full_inner <- merge(merge(shls_return, ssgf_return, join = "inner"), jqr_return, join = "inner")
merge(merge(x,z, join = "left"),y,join='left')[,c(1,3,2)]
m <- x %>% merge(y, join = "left") %>% merge(z, join = "left")
names(m) <-  c("x", "y", "z")
```

#### access a period of data
```r
sample.xts['2007']  # all of 2007
sample.xts['2007-03/']  # March 2007 to the end of the data set
sample.xts['2007-03/2007']  # March 2007 to the end of 2007
sample.xts['/'] # the whole data set
sample.xts['/2007'] # the beginning of the data through 2007
sample.xts['2007-01-03'] # just the 3rd of January 2007
sample.xts['2006-01-03/2007']

if_min["2010-10-01 09:20:00/2012-05-03"] %>% tail() # head, tail
```

#### extract a price without a date attached from xts
```r
shls_origin <- shls.xts["2011-03-02"]$Close[[1]]
```

#### convert xts => data.frame
```r
shls.xts %>% as.data.frame() %>% mutate(Date = shls.xts %>% index()) %>% glimpse()
```

#### remove/ignore/acess rownames
```r
full_df <- full %>% as.data.frame(row.names = F)
full_df %>% rownames()
write.csv(full_df, file = "/Users/Natsume/Documents/1learnD3/R4D3/stocks_returns.csv", row.names = F)
```

##### use of dplyr
```r
library(nycflights13)

```
