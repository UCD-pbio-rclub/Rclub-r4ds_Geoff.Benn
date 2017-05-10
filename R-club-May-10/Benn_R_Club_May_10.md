# Benn_R_Club_May_10




```r
library(tidyverse)
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

```r
library(nycflights13)
data <- flights
```


```r
sessionInfo()
```

```
## R version 3.3.2 (2016-10-31)
## Platform: x86_64-apple-darwin13.4.0 (64-bit)
## Running under: OS X El Capitan 10.11.6
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] nycflights13_0.2.2 dplyr_0.5.0        purrr_0.2.2       
## [4] readr_1.1.0        tidyr_0.6.1        tibble_1.3.0      
## [7] ggplot2_2.2.1      tidyverse_1.1.1   
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_0.12.10     cellranger_1.1.0 plyr_1.8.4       forcats_0.2.0   
##  [5] tools_3.3.2      digest_0.6.12    jsonlite_1.4     lubridate_1.6.0 
##  [9] evaluate_0.10    nlme_3.1-131     gtable_0.2.0     lattice_0.20-35 
## [13] psych_1.7.5      DBI_0.6-1        yaml_2.1.14      parallel_3.3.2  
## [17] haven_1.0.0      xml2_1.1.1       stringr_1.2.0    httr_1.2.1      
## [21] knitr_1.15.1     hms_0.3          rprojroot_1.2    grid_3.3.2      
## [25] R6_2.2.0         readxl_1.0.0     foreign_0.8-68   rmarkdown_1.5   
## [29] modelr_0.1.0     reshape2_1.4.2   magrittr_1.5     backports_1.0.5 
## [33] scales_0.4.1     htmltools_0.3.6  rvest_0.3.2      assertthat_0.2.0
## [37] mnormt_1.5-5     colorspace_1.3-2 stringi_1.1.5    lazyeval_0.2.0  
## [41] munsell_0.4.3    broom_0.4.2
```

##5.2.4 Exercises

**1. Find all flights that**

1. Had an arrival delay of two or more hours  


```r
q1 <- filter(data, arr_delay >= 120)
```

2. Flew to Houston (IAH or HOU)  


```r
q2 <- filter(data, dest == "IAH" | dest == "HOU")
```

3. Were operated by United, American, or Delta  


```r
q3 <- filter(data, carrier == "UA"| carrier == "AA" | carrier == "DL")
q3.2 <- filter(data, carrier %in% c('UA','AA','DL'))
```

4. Departed in summer (July, August, and September)


```r
q4 <- filter(data, month %in% c(7,8,9))
```

5. Arrived more than two hours late, but didn’t leave late


```r
q5 <- filter(data, arr_delay > 120 & dep_delay == 0)
```

6. Were delayed by at least an hour, but made up over 30 minutes in flight


```r
q6 <- filter(data, dep_delay >= 60 & (dep_delay - arr_delay) > 30)
```

7. Departed between midnight and 6am (inclusive)

```r
q7 <- filter(data, dep_time <= 600)
```

**2.Another useful dplyr filtering helper is between(). What does it do? Can you use it to simplify the code needed to answer the previous challenges?**

Between looks for numeric values in between two numbers.

```r
q4.2 <- filter(data, between(dep_time, 0, 600))
```

**3. How many flights have a missing dep_time? What other variables are missing? What might these rows represent?**


```r
q3.3 <- filter(data, is.na(dep_time))
```

These data points are also missing arrival times, air times and delays. These are likely canceled flights.

**4. Why is NA ^ 0 not missing? Why is NA | TRUE not missing? Why is FALSE & NA not missing? Can you figure out the general rule? (NA * 0 is a tricky counterexample!)**


```r
NA ^ 0
```

```
## [1] 1
```

```r
NA | TRUE
```

```
## [1] TRUE
```

```r
NA & FALSE
```

```
## [1] FALSE
```

```r
NA*0
```

```
## [1] NA
```
Anything raised to the 0 is 1, NA or TRUE would evaluate to true as the first position doesn't matter, the third one I'm less sure of. I can't figure out the general rule.

##5.3.1 Exercises

**1. How could you use arrange() to sort all missing values to the start? (Hint: use is.na()).**


```r
q3.1.1 <- arrange(data, desc(is.na(dep_time)))
```

**2. Sort flights to find the most delayed flights. Find the flights that left earliest.**


```r
q3.1.2 <- arrange(data, desc(dep_delay))
q3.1.2b <- arrange(data, dep_delay)
```

**3. Sort flights to find the fastest flights.**


```r
q3.1.3 <- arrange(data, desc(distance/air_time))
```

**4. Which flights travelled the longest? Which travelled the shortest?**


```r
q3.1.4a <- arrange(data, desc(distance))
```

The JFK to Honolulu flights were the longest.


```r
q3.1.4b <- arrange(data, distance)
```

The shortest regular flights were between Newark and Philly.

##5.4.1 Exercises

**1. Brainstorm as many ways as possible to select dep_time, dep_delay, arr_time, and arr_delay from flights.**


```r
select(data, dep_time, dep_delay, arr_time, arr_delay)
```

```
## # A tibble: 336,776 × 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
## 1       517         2      830        11
## 2       533         4      850        20
## 3       542         2      923        33
## 4       544        -1     1004       -18
## 5       554        -6      812       -25
## 6       554        -4      740        12
## 7       555        -5      913        19
## 8       557        -3      709       -14
## 9       557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```

```r
select(data, dep_time:arr_delay, -(contains("sched")))
```

```
## # A tibble: 336,776 × 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
## 1       517         2      830        11
## 2       533         4      850        20
## 3       542         2      923        33
## 4       544        -1     1004       -18
## 5       554        -6      812       -25
## 6       554        -4      740        12
## 7       555        -5      913        19
## 8       557        -3      709       -14
## 9       557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```

```r
select(data, contains("arr_"), contains("dep_"), -(contains("sched")))
```

```
## # A tibble: 336,776 × 4
##    arr_time arr_delay dep_time dep_delay
##       <int>     <dbl>    <int>     <dbl>
## 1       830        11      517         2
## 2       850        20      533         4
## 3       923        33      542         2
## 4      1004       -18      544        -1
## 5       812       -25      554        -6
## 6       740        12      554        -4
## 7       913        19      555        -5
## 8       709       -14      557        -3
## 9       838        -8      557        -3
## 10      753         8      558        -2
## # ... with 336,766 more rows
```

```r
select(data, ends_with("time"), ends_with("delay"),-(contains("sched")), -(contains("air")))
```

```
## # A tibble: 336,776 × 4
##    dep_time arr_time dep_delay arr_delay
##       <int>    <int>     <dbl>     <dbl>
## 1       517      830         2        11
## 2       533      850         4        20
## 3       542      923         2        33
## 4       544     1004        -1       -18
## 5       554      812        -6       -25
## 6       554      740        -4        12
## 7       555      913        -5        19
## 8       557      709        -3       -14
## 9       557      838        -3        -8
## 10      558      753        -2         8
## # ... with 336,766 more rows
```

**2. What happens if you include the name of a variable multiple times in a select() call?**


```r
select(data, dep_time, dep_time, dep_time)
```

```
## # A tibble: 336,776 × 1
##    dep_time
##       <int>
## 1       517
## 2       533
## 3       542
## 4       544
## 5       554
## 6       554
## 7       555
## 8       557
## 9       557
## 10      558
## # ... with 336,766 more rows
```

It seems to ignore it.

**3. What does the one_of() function do? Why might it be helpful in conjunction with this vector?**


```r
vars <- c("year", "month", "day", "dep_delay", "arr_delay")
select(data, one_of(vars))
```

```
## # A tibble: 336,776 × 5
##     year month   day dep_delay arr_delay
##    <int> <int> <int>     <dbl>     <dbl>
## 1   2013     1     1         2        11
## 2   2013     1     1         4        20
## 3   2013     1     1         2        33
## 4   2013     1     1        -1       -18
## 5   2013     1     1        -6       -25
## 6   2013     1     1        -4        12
## 7   2013     1     1        -5        19
## 8   2013     1     1        -3       -14
## 9   2013     1     1        -3        -8
## 10  2013     1     1        -2         8
## # ... with 336,766 more rows
```

It looks for column names that contain any of the strings in the vector.

**4. Does the result of running the following code surprise you? How do the select helpers deal with case by default? How can you change that default?**


```r
select(data, contains("TIME"))
```

```
## # A tibble: 336,776 × 6
##    dep_time sched_dep_time arr_time sched_arr_time air_time
##       <int>          <int>    <int>          <int>    <dbl>
## 1       517            515      830            819      227
## 2       533            529      850            830      227
## 3       542            540      923            850      160
## 4       544            545     1004           1022      183
## 5       554            600      812            837      116
## 6       554            558      740            728      150
## 7       555            600      913            854      158
## 8       557            600      709            723       53
## 9       557            600      838            846      140
## 10      558            600      753            745      138
## # ... with 336,766 more rows, and 1 more variables: time_hour <dttm>
```

This ignores case and returns everything with time.


```r
select(data, contains("TIME", ignore.case = FALSE))
```

```
## # A tibble: 336,776 × 0
```

##5.5.2 Exercises

**1. Currently dep_time and sched_dep_time are convenient to look at, but hard to compute with because they’re not really continuous numbers. Convert them to a more convenient representation of number of minutes since midnight.**


```r
q5.2.1 <- mutate(data, dep_abs = ((dep_time %/% 100)*60) + dep_time %% 100, sched_dep_abs = ((sched_dep_time %/% 100)*60) + sched_dep_time %% 100)
```

**2. Compare air_time with arr_time - dep_time. What do you expect to see? What do you see? What do you need to do to fix it?**

```r
q5.2.2 <- mutate(data, air_time, air.diff = arr_time - dep_time)
```

I'm not sure how air_time is being calculated - it doesn't seem to line up with the listed times.

**3. Compare dep_time, sched_dep_time, and dep_delay. How would you expect those three numbers to be related?**
dep_delay is the difference between dep_time and sched_dep_time.

**4. Find the 10 most delayed flights using a ranking function. How do you want to handle ties? Carefully read the documentation for min_rank().**


```r
q5.2.4 <- mutate(data, delay.rank = min_rank(desc(dep_delay, arr_delay)))
q5.2.4 <- arrange(q5.2.4, delay.rank)
filter(q5.2.4, delay.rank <= 10)
```

```
## # A tibble: 10 × 20
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     9      641            900      1301     1242
## 2   2013     6    15     1432           1935      1137     1607
## 3   2013     1    10     1121           1635      1126     1239
## 4   2013     9    20     1139           1845      1014     1457
## 5   2013     7    22      845           1600      1005     1044
## 6   2013     4    10     1100           1900       960     1342
## 7   2013     3    17     2321            810       911      135
## 8   2013     6    27      959           1900       899     1236
## 9   2013     7    22     2257            759       898      121
## 10  2013    12     5      756           1700       896     1058
## # ... with 13 more variables: sched_arr_time <int>, arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>,
## #   time_hour <dttm>, delay.rank <int>
```

So it looks like you can set the tie method using ties.method, however I can't find a list of all the options anywhere.

**5. What does 1:3 + 1:10 return? Why?**


```r
1:3 + 1:10
```

```
## Warning in 1:3 + 1:10: longer object length is not a multiple of shorter
## object length
```

```
##  [1]  2  4  6  5  7  9  8 10 12 11
```

It returns a vector, where 1, 2, and 3 were added to the numbers 1-10, with recycling.

**6. What trig functions does R have?**

Sin, cos, tan, arcsin, arccos, arctan.
