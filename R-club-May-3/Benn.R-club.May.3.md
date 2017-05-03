# R-club-May-3.Benn




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

##3.6.1 Exercises  

**1. What geom would you use to draw a line chart? A boxplot? A histogram? An area chart?**
geom_line(), geom_boxplot(), geom_histogram(), geom_area().

**2. Run this code in your head and predict what the output will look like. Then, run the code in R and check your predictions.**  

This will produce a scatterplot of displacement by highway mileage, with points colored based on drivetrain. There will also be a trendline with no standard error bars.


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_point() + 
  geom_smooth(se = FALSE)
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

So I was not quite correct. The trendlines also follow the color aes, so there are 3 lines.

**3. What does show.legend = FALSE do? What happens if you remove it? Why do you think I used it earlier in the chapter?**  

It hides the legend. This keeps the graph looking cleaner if there are many categories.


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_point(show.legend = FALSE) + 
  geom_smooth(se = TRUE)
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

Not clear on this one.

**4. What does the se argument to geom_smooth() do?**
It generates standard errors around the trendlines.

**5. Will these two graphs look different? Why/why not?**
The graphs should look the same - they do the mapping, just in different places.

**6. Generate R code to recreate graphs.**

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth(se = FALSE)
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth(mapping = aes(x = displ, y = hwy, group = drv), se = FALSE)
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_smooth(mapping = aes(x = displ, y = hwy, group = drv), se = FALSE)
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_smooth(mapping = aes(x = displ, y = hwy), se = FALSE)
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_smooth(mapping = aes(x = displ, y = hwy, linetype = drv), se = FALSE)
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

```r
ggplot() + 
  geom_point(data = mpg, mapping = aes(x = displ, y = hwy, color = drv, stroke = 4))
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

##3.7.1 Exercises
**1. What is the default geom associated with stat_summary()? How could you rewrite the previous plot to use that geom function instead of the stat function?**
Stat_summary is associated with geom_histogram and geom_freqpoly.


```r
ggplot(data=mpg) +
  geom_histogram(mapping = aes(x=hwy),binwidth = 5)
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

**couldn't figure out how to duplicate the above with stat_summary()**

```r
ggplot(data=mpg) +
  stat_summary(fun.y = "count", mapping = aes(hwy))
```

```
## Warning: Computation failed in `stat_summary()`:
## no applicable method for 'group_by_' applied to an object of class "NULL"
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

**couldn't figure out how to duplicate the stat_summary thing with either of the associated geoms**

```r
ggplot(data = diamonds) + 
  geom_histogram(mapping = aes(x = cut, y = depth))
```

```
## Error: stat_bin() must not be used with a y aesthetic.
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

**2. What does geom_col() do? How is it different to geom_bar()?**
geom_col() doesn't seem to exist in my version of R.

**3. Most geoms and stats come in pairs that are almost always used in concert. Read through the documentation and make a list of all the pairs. What do they have in common?**

geom_boxplot - stat_boxplot
geom_bar - stat_count
geom_point - stat = "identity"
geom_errorbar - stat = "identity"

I'm not sure what the last part of the question is getting at.

**4. What variables does stat_smooth() compute? What parameters control its behaviour?**

```r
ggplot(mpg, aes(displ, hwy)) +
  geom_point() +
  geom_smooth()
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
smooth() plots a trendline and produces error bars. It takes input in terms of the method - either a formula or a method.

**5. In our proportion bar chart, we need to set group = 1. Why? In other words what is the problem with these two graphs?**  


```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = ..prop..))
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = ..prop.., group = 1))
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

It looks like without group, it's calculating proportions within each diamond grade.

##3.8.1 Exercises

**1. What is the problem with this plot? How could you improve it?**

```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point()
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

I'm guessing it's omitting points.

```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point() +
  geom_jitter()
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

**2. What parameters to geom_jitter() control the amount of jittering?**
width and height control the amount of jittering.

**3. Compare and contrast geom_jitter() with geom_count().**

```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point() +
  geom_count()
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

jitter shifts points slightly to prevent overplotting, while count increases point size when there are multiple points at a particular coordinate.

**4. What’s the default position adjustment for geom_boxplot()? Create a visualisation of the mpg dataset that demonstrates it.**
The default is dodge.

```r
ggplot(data = mpg, mapping = aes(x = drv, y = hwy, fill = manufacturer)) + 
  geom_boxplot(position = position_dodge(1))
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

##3.9.1 Exercises

**1. Turn a stacked bar chart into a pie chart using coord_polar().**

```r
x <- ggplot(data = mpg) + geom_bar(mapping = aes(x = manufacturer, fill = drv))
x + coord_flip()
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

```r
x + coord_polar()
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-20-2.png)<!-- -->

**2. labs()?**
Allows you to specify labels.

**3. coord_quickmap vs coord_map**
Quickmap makes a simplified projection that runs faster, but is less accurate.

**4. What does the plot below tell you about the relationship between city and highway mpg? Why is coord_fixed() important? What does geom_abline() do?**
This tells us that city and hwy mpg are correlated.

```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_point() + 
  geom_abline() +
  coord_fixed()
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_point() + 
  geom_abline()
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-22-1.png)<!-- -->

I can't tell the difference when coord_fixed() is removed - the scales stay the same, though the proportions are different. Not sure why this is significant.
I'm also not clear on what abline is doing - it's clearly drawing a line, but I'm not sure what the defaults are for setting the slope and intercept.

##4.4 Practice
**1. Why does this code not work?**
my_variable is mispelled in line 2- there's a 1 instead of an i.

**2. Tweak each of the following R commands so that they run correctly:**


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))
```

![](Benn.R-club.May.3_files/figure-html/unnamed-chunk-23-1.png)<!-- -->


```r
filter(mpg, cyl == 8)
```

```
## # A tibble: 70 × 11
##    manufacturer              model displ  year   cyl      trans   drv
##           <chr>              <chr> <dbl> <int> <int>      <chr> <chr>
## 1          audi         a6 quattro   4.2  2008     8   auto(s6)     4
## 2     chevrolet c1500 suburban 2wd   5.3  2008     8   auto(l4)     r
## 3     chevrolet c1500 suburban 2wd   5.3  2008     8   auto(l4)     r
## 4     chevrolet c1500 suburban 2wd   5.3  2008     8   auto(l4)     r
## 5     chevrolet c1500 suburban 2wd   5.7  1999     8   auto(l4)     r
## 6     chevrolet c1500 suburban 2wd   6.0  2008     8   auto(l4)     r
## 7     chevrolet           corvette   5.7  1999     8 manual(m6)     r
## 8     chevrolet           corvette   5.7  1999     8   auto(l4)     r
## 9     chevrolet           corvette   6.2  2008     8 manual(m6)     r
## 10    chevrolet           corvette   6.2  2008     8   auto(s6)     r
## # ... with 60 more rows, and 4 more variables: cty <int>, hwy <int>,
## #   fl <chr>, class <chr>
```

**4. alt+shift+k**
This brings up keyboard shortcuts. This can also be accessed via the help menu.
