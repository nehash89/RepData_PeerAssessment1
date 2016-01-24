# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

This is an R Markdown document for Reproducable Research - Course Project 1. We start by reading the file into R and formatting it.


```r
    activity <- read.csv("C:/Users/Nehash/OneDrive/Data Science Specialization/Reproducable Research/repdata-data-activity/activity.csv")
    activity$date <- as.Date(activity$date, format = "%Y-%m-%d")
```

Next step is to load Dplyr package in R


```r
  library("dplyr", lib.loc="~/R/win-library/3.1")
```

```
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

## What is mean total number of steps taken per day?


```r
  activity <- group_by(activity, date)
  daily_step_summary <- summarise(activity, sum = sum(steps, na.rm = TRUE))

  hist(daily_step_summary$sum, xlab = "Total steps taken per day", main = paste("Histogram of" ,  "total number of steps taken each day"))
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

```r
  mean(daily_step_summary$sum)
```

```
## [1] 9354.23
```

```r
  median(daily_step_summary$sum)
```

```
## [1] 10395
```

## What is the average daily activity pattern?


```r
  activity <- group_by(activity, interval)
  daily_interval_summary <- summarise(activity, avg = mean(steps, na.rm = TRUE))
  
  plot(daily_interval_summary$interval, daily_interval_summary$avg, type = "l", xlab = "Interval", ylab = "Average", main = "Time series plot of interval and the average steps taken")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 

## Imputing missing values

```r
  sum(is.na(activity$steps))
```

```
## [1] 2304
```

```r
  activity_copy <- activity
  indx <- which(is.na(activity_copy$steps), arr.ind=TRUE)
  activity_copy$steps[indx] <- mean(activity_copy$steps, na.rm = TRUE)
  
  activity_copy <- group_by(activity_copy, date)
  daily_step_summary_copy <- summarise(activity_copy, sum = sum(steps, na.rm = TRUE))
  hist(daily_step_summary_copy$sum, xlab = "Total steps taken per day", main = paste("Histogram of" ,  "total number of steps taken each day - NA REPLACED"))
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

```r
  mean(daily_step_summary_copy$sum)
```

```
## [1] 10766.19
```

```r
  median(daily_step_summary_copy$sum)
```

```
## [1] 10766.19
```
## Are there differences in activity patterns between weekdays and weekends?


