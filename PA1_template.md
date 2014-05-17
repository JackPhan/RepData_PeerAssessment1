# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
data <- read.csv("activity.csv")
mydt <- tapply(data$steps, data$date, mean)
hist(mydt, main = "Total number of steps taken each day", xlab = "Date", ylab = "Steps")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1.png) 


## What is mean total number of steps taken per day?

```r
mean(mydt, na.rm = TRUE)
```

```
## [1] 37.38
```

```r
median(mydt, na.rm = TRUE)
```

```
## [1] 37.38
```


## What is the average daily activity pattern?

```r
a = na.omit(data)
dt = split(a$steps, a$interval)
b = lapply(dt, mean)
c = as.numeric(b)
time = data$interval[1:288]
plot(time, c, type = "l", main = "The average daily activity pattern", xlab = "5 min time interval", 
    ylab = "Average steps")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

5-minute interval contains the maximum number of steps

```r
b[match(max(c), b)]
```

```
## $`835`
## [1] 206.2
```


## Imputing missing values

```r
data[is.na(data)] = c
```


## Are there differences in activity patterns between weekdays and weekends?

```r
data$date = as.Date(data$date)
tf = weekdays(data$date) == "Sunday" | weekdays(data$date) == "Saturday"
data$date = as.character(data$date)
data$date[tf] <- "weekend"
data$date[!tf] <- "weekday"

library(ggplot2)
g = qplot(interval, steps, data = data, facets = date ~ ., binwidth = 2)
g + geom_line() + ylim(-2, 4)
```

```
## Warning: Removed 4154 rows containing missing values (geom_point).
## Warning: Removed 1685 rows containing missing values (geom_point).
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6.png) 

