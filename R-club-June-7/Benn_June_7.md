# Benn.June-7




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
## [1] dplyr_0.5.0     purrr_0.2.2     readr_1.1.0     tidyr_0.6.1    
## [5] tibble_1.3.0    ggplot2_2.2.1   tidyverse_1.1.1
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

##11.2.2 Exercises

**1. What function would you use to read a file where fields were separated with “|”?**  

I would use read_delim() as you can specify any delimiting symbol.

**2. Apart from file, skip, and comment, what other arguments do read_csv() and read_tsv() have in common?**

Quite a few. Some that might be of interest include "na", col_names, and trim_ws.

**3. What are the most important arguments to read_fwf()?**

File, col_positions, widths, start, end.

**4. Sometimes strings in a CSV file contain commas. To prevent them from causing problems they need to be surrounded by a quoting character, like " or '. By convention, read_csv() assumes that the quoting character will be ", and if you want to change it you’ll need to use read_delim() instead. What arguments do you need to specify to read the following text into a data frame?**

"x,y\n1,'a,b'"

The question isn't really clear here - I'm going to assume that the outer "" are part of the string and the whole thing is a single value that we want to read in as part of a bigger CSV file.


```r
q4 <- read_delim("x,y\n1,'a,b'",delim = ",", quote = "\'")
```

**5. Identify what is wrong with each of the following inline CSV files. What happens when you run the code?**


```r
read_csv("a,b\n1,2,3\n4,5,6")
```

```
## Warning: 2 parsing failures.
## row col  expected    actual         file
##   1  -- 2 columns 3 columns literal data
##   2  -- 2 columns 3 columns literal data
```

```
## # A tibble: 2 × 2
##       a     b
##   <int> <int>
## 1     1     2
## 2     4     5
```

Generates an error because there are initially 2 columns (a and b) and then there are 3, so the 3rd column in the next two rows gets dropped.  


```r
read_csv("a,b,c\n1,2\n1,2,3,4")
```

```
## Warning: 2 parsing failures.
## row col  expected    actual         file
##   1  -- 3 columns 2 columns literal data
##   2  -- 3 columns 4 columns literal data
```

```
## # A tibble: 2 × 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2    NA
## 2     1     2     3
```

Same issue, this times its 3 columns, then 2, then 4.


```r
read_csv("a,b\n\"1")
```

```
## Warning: 2 parsing failures.
## row col                     expected    actual         file
##   1  a  closing quote at end of file           literal data
##   1  -- 2 columns                    1 columns literal data
```

```
## # A tibble: 1 × 2
##       a     b
##   <int> <chr>
## 1     1  <NA>
```

Not totally sure what was intended here. Either the quote was not escaped properly or there's uneven column numbers.  


```r
read_csv("a,b\n1,2\na,b")
```

```
## # A tibble: 2 × 2
##       a     b
##   <chr> <chr>
## 1     1     2
## 2     a     b
```

This seemed to work fine.


```r
read_csv("a;b\n1;3")
```

```
## # A tibble: 1 × 1
##   `a;b`
##   <chr>
## 1   1;3
```

Also okay if the goal was 1 column and two rows. If the goal was to use ; as a seperator, then read_delim should have been used.

##11.3.5

**1. What are the most important arguments to locale()?**

It seems like the date_format and time_format would be important, as would the grouping and decimal marks.

**2. What happens if you try and set decimal_mark and grouping_mark to the same character? What happens to the default value of grouping_mark when you set decimal_mark to “,”? What happens to the default value of decimal_mark when you set the grouping_mark to “.”?**


```r
locale(decimal_mark = ".", grouping_mark = ".")
```

```
## Error: `decimal_mark` and `grouping_mark` must be different
```


```r
locale(decimal_mark = ",")
```

```
## <locale>
## Numbers:  123.456,78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

I'm not sure how to check what happens to the grouping default.

**3. I didn’t discuss the date_format and time_format options to locale(). What do they do? Construct an example that shows when they might be useful.**


```r
parse_time("09PM")
```

```
## Warning: 1 parsing failure.
## row col   expected actual
##   1  -- time like    09PM
```

```
## NA
```

```r
locale(time_format = "%I/%p")
```

```
## <locale>
## Numbers:  123,456.78
## Formats:  %AD / %I/%p
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

```r
parse_time("09PM")
```

```
## Warning: 1 parsing failure.
## row col   expected actual
##   1  -- time like    09PM
```

```
## NA
```

Not totally sure why this didn't work.

**5. What’s the difference between read_csv() and read_csv2()?**  
This for european style files with semi-colon seperators and commas for decimals.

**6. What are the most common encodings used in Europe? What are the most common encodings used in Asia? Do some googling to find out.**

It looks like may european countries use variations on ISO-8859, while in China Big5 and GB18030 are used. However it looks like UTF-8 is common across the board.

**7. Generate the correct format string to parse each of the following dates and times:**


```r
d1 <- "January 1, 2010"
parse_date(d1, "%B %d%. %Y")
```

```
## [1] "2010-01-01"
```


```r
d2 <- "2015-Mar-07"
parse_date(d2, "%Y-%b-%d")
```

```
## [1] "2015-03-07"
```


```r
d3 <- "06-Jun-2017"
parse_date(d3, "%d-%b-%Y")
```

```
## [1] "2017-06-06"
```


```r
d4 <- c("August 19 (2015)", "July 1 (2015)")
parse_date(d4, "%B %d (%Y)")
```

```
## [1] "2015-08-19" "2015-07-01"
```


```r
d5 <- "12/30/14" # Dec 30, 2014
parse_date(d5,"%m/%d/%y")
```

```
## [1] "2014-12-30"
```


```r
library(hms)
t1 <- "1705"
parse_time(t1,"%H%M")
```

```
## 17:05:00
```


```r
t2 <- "11:15:10.12 PM"
parse_time(t2, "%H:%M:%S %p")
```

```
## 23:15:10
```

**other notes**
readxls looks cool - could skip the making a csv or tab stage of the process.


