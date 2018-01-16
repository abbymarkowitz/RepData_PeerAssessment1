---
output: 
  html_document: 
    keep_md: yes
---
Course 5 Week 2 Assignment

1. Set working directory, read and import the data, get per day steps

```r
setwd("C:/Users/abmarkowitz/Documents/coursera")
data <- read.csv("course5week2data.csv")
head(data)
```

```
##   steps      date interval
## 1    NA 10/1/2012        0
## 2    NA 10/1/2012        5
## 3    NA 10/1/2012       10
## 4    NA 10/1/2012       15
## 5    NA 10/1/2012       20
## 6    NA 10/1/2012       25
```

```r
dataByDay <- aggregate(steps ~ date, sum, data=data)
```

2. What is the mean and median total number of steps taken per day?

```r
mean(dataByDay$steps, na.rm = TRUE)
```

```
## [1] 10766.19
```

```r
hist(dataByDay$steps)
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
median(dataByDay$steps, na.rm = TRUE)
```

```
## [1] 10765
```

3. What is the average daily activity pattern? Which interval contains the max number of steps?

```r
dataByTime <- aggregate(steps ~ interval, mean, data=data)
plot(dataByTime$interval, dataByTime$steps, type ="l")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```r
which.max(dataByTime$steps)
```

```
## [1] 104
```

```r
dataByTime[which.max(dataByTime$steps),]
```

```
##     interval    steps
## 104      835 206.1698
```

4. Find the number of NAs, replace them with the average number of steps, and recalculate mean and median.

```r
sum(is.na(data$steps))
```

```
## [1] 2304
```

```r
data$steps[is.na(data$steps)] <- mean(data$steps, na.rm = TRUE)
NoNAs <- data
sum(is.na(NoNAs))
```

```
## [1] 0
```

```r
NoNAsByDay <- aggregate(steps ~ date, sum, data=NoNAs)
mean(NoNAsByDay$steps, na.rm = TRUE)
```

```
## [1] 10766.19
```

```r
hist(NoNAsByDay$steps)
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
median(NoNAsByDay$steps, na.rm = TRUE)
```

```
## [1] 10766.19
```

Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?
-The mean does not change, as we replaced missing value with the average value. However, the median increased slightly. 

5. Are there differences in activity patterns between weekdays and weekends?
-Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.


```r
data$date <- as.Date(data$date, "%m/%d/%Y")
data$day <- weekdays(data$date)
weekdays1 <- c('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday')
weekend1 <- c("Saturday", "Sunday")
data$day[data$day %in% weekdays1] <- "weekday"
data$day[data$day %in% weekend1] <- "weekend"

weekdays <- subset(data, day == 'weekday', select = steps:day)
weekends <- subset(data, day == "weekend", select = steps:day)

par(mfrow=c(2,1))
plot(weekdays$interval, weekdays$steps, type = "l")
plot(weekends$interval, weekends$steps, type = "l")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


