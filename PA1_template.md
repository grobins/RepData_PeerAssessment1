# Reproducible Research: Peer Assessment 1



## Loading and preprocessing the data


```r
require(dplyr)
```

```
## Loading required package: dplyr
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
require(ggplot2)
```

```
## Loading required package: ggplot2
```

```r
require(ggthemes)
```

```
## Loading required package: ggthemes
```

```r
dat <- read.csv('activity.csv')
dat$date <- as.Date(dat$date)
```





## What is mean total number of steps taken per day?

There are a mean of 9354.2 steps taken each day

```r
dat %>% group_by(date) %>% 
  summarise(dailysteps = sum(steps, na.rm=T)) %>% 
    ggplot(aes(dailysteps)) + geom_histogram(binwidth = 500) + 
  theme_hc() + scale_colour_hc() +
  ggtitle('Distibution of Daily Steps') + xlab('Daily Step Total') + ylab('Frequency')
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
dat %>% group_by(date) %>% 
  summarise(dailysteps = sum(steps, na.rm=T)) %>% 
    summarise(mean = mean(dailysteps, na.rm=T),
              median = median(dailysteps, na.rm=T))
```

```
## Source: local data frame [1 x 2]
## 
##      mean median
## 1 9354.23  10395
```


## What is the average daily activity pattern?

Below is a plot of the average daily pattern across 61 days.  The 5-min interval labelled 


```r
dat %>% group_by(interval) %>% summarise(avgSteps = mean(steps, na.rm=T)) %>% filter(avgSteps != 0) %>% 
  ggplot(aes(interval, avgSteps)) + geom_path() + ggtitle('Trend of step count for 5min intervals through an average day') + 
  xlab('5-min Interval During Day') + ylab('Step Count')
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

The 5-min interval labelled 835 has the highest step count during the average day.


```r
dat %>% group_by(interval) %>% summarise(avgSteps = mean(steps, na.rm=T)) %>% arrange(desc(avgSteps)) %>% top_n(1)
```

```
## Selecting by avgSteps
```

```
## Source: local data frame [1 x 2]
## 
##   interval avgSteps
## 1      835 206.1698
```




## Imputing missing values




## Are there differences in activity patterns between weekdays and weekends?
