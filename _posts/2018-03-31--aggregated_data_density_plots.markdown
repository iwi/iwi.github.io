---
title: "Aggregate data for density plots"
output:
  html_document: default
  html_notebook: default
---

When doing descriptive stats it's quite useful to draw some histograms. A nice approach to that are 
the density plots that you can draw with `ggplot2::geom_density`. It is also useful 
to create a grid of plots by different subsets of data with `ggplot2::facet_grid`.

Let's create some data to show it

```r
library(dplyr)
library(ggplot2)
library(knitr)
```


```r
simulator <- function(n) {
  t <- tibble(
    x = c(rnorm(n = n/2, 0, 1), rnorm(n = n, 2, 2), rnorm(n = n, 2, 3)),
    y = as.factor(c(rep("A", n/2), rep("B", n), rep("C", n))),
    z = as.factor(rbinom(n * 2.5, size = 1, prob = 0.5)),
    w = as.factor(sample(1:5, replace = TRUE, prob = rep(0.2, 5), size = n * 2.5))
  )
  return(t)
}

t <- simulator(1e5)
glimpse(t)
```

```
## Observations: 250,000
## Variables: 4
## $ x <dbl> 0.34173925, 2.09904014, -0.03973865, 0.45381049, 2.14291862,...
## $ y <fct> A, A, A, A, A, A, A, A, A, A, A, A, A, A, A, A, A, A, A, A, ...
## $ z <fct> 1, 1, 0, 1, 1, 1, 1, 0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 1, 0, ...
## $ w <fct> 4, 4, 2, 5, 2, 1, 4, 5, 4, 2, 1, 5, 3, 1, 4, 2, 5, 5, 2, 4, ...
```

```r
summary(t)
```

```
##        x             y          z          w        
##  Min.   :-10.57888   A: 50000   0:124881   1:50406  
##  1st Qu.: -0.08537   B:100000   1:125119   2:50048  
##  Median :  1.37243   C:100000              3:49551  
##  Mean   :  1.60387                         4:50004  
##  3rd Qu.:  3.19154                         5:49991  
##  Max.   : 14.92003
```

and then let's plot it

```r
p <- 
  t %>% 
    ggplot(aes(x = x, colour = y, fill = y)) +
      geom_density(alpha = 0.2, adjust = 1) +
      xlim(c(-10, 10)) +
      theme_bw() +
      facet_grid(w ~ z,
                 margins = TRUE,
                 as.table = TRUE)
p
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png)
another plot that is nice to see...

```r
t %>% 
  ggplot(aes(x, colour = y, fill = y)) +
    geom_freqpoly() +
    theme_bw() +
    facet_grid(w ~ z,
               margins = TRUE)
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png)

However, if the data is a bit large, e.g. 10 million records or more, R chokes.


```r
t <- simulator(1e7)

t %>% 
  ggplot(aes(x = x, colour = y, fill = y)) +
    geom_density(alpha = 0.2, adjust = 1) +
    xlim(c(-10, 10)) +
    theme_bw() +
    facet_grid(w ~ z,
               margins = TRUE,
               as.table = TRUE)
```

```
Error: cannot allocate vector of size 762.9 Mb
```

A solution to that is to aggregate the data before drawing the plots. Of course, there will be
information loss, but that should be ok if the aggregation is done properly.

This is the first attempt to do an aggregation. Initially focusing on drawing a simple density plot for all the data.

I took the mean within each decile.

For the example I'll use just 10,000 to see that different options capture the differences.

```r
nt <- 
  t %>% 
    mutate(parts = case_when(
      x < quantile(x, probs = 0.1) ~ 1,
      x < quantile(x, probs = 0.2) ~ 2,
      x < quantile(x, probs = 0.3) ~ 3,
      x < quantile(x, probs = 0.4) ~ 4,
      x < quantile(x, probs = 0.5) ~ 5,
      x < quantile(x, probs = 0.6) ~ 6,
      x < quantile(x, probs = 0.7) ~ 7,
      x < quantile(x, probs = 0.8) ~ 8,
      x < quantile(x, probs = 0.9) ~ 9,
      x <= quantile(x, probs = 1) ~ 10))

agg <- 
  nt %>% 
    group_by(parts, y) %>% 
    summarise(s = mean(x))
```


```r
agg %>% 
  ggplot(aes(x = s)) +
    geom_density(alpha = 0.2,
                 adjust = 1,
                 fill = "red") +
    xlim(c(-2.5, 7.5)) +
    theme_bw()
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png)

Which is a bit different to the original

```r
t %>% 
  ggplot(aes(x = x)) +
    geom_density(alpha = 0.2,
                 adjust = 2,
                 fill = "blue") +
    xlim(c(-2.5, 7.5)) +
    theme_bw()
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8-1.png)

The quantilisation is ugly and difficult to adjust if I want more precision. This option is a bit better becausee it
by moving the `by` I can adjust it.

Here instead of taking the mean I'm taking the percentile value.

```r
qx <- tibble(q = quantile(t$x, probs = seq(0, 1, by = 0.0001)))
qx %>% 
  ggplot(aes(x = q)) +
    geom_density(alpha = 0.2,
                 adjust = 2,
                 fill = "green") +
    xlim(c(-2.5, 7.5)) +
    theme_bw()
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9-1.png)


This option is fast enough, so we can now expand it to using one grouping variable.

```r
library(tidyr)
library(broom)
library(purrr)

qx <- 
  t %>%
    nest(-y) %>%
    mutate(Quantiles = map(data, ~ quantile(.$x, probs = seq(0, 1, by = 0.0001)))) %>% 
    unnest(map(Quantiles, tidy))

qx %>% 
  ggplot(aes(x = x, colour = y, fill = y)) +
    geom_density(alpha = 0.2,
                 adjust = 2) +
    xlim(c(-2.5, 7.5)) +
    theme_bw()
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10-1.png)

Which looks extremely similar to the original one:

```r
t %>% 
  ggplot(aes(x = x, colour = y, fill = y)) +
    geom_density(alpha = 0.2,
                 adjust = 2) +
    xlim(c(-2.5, 7.5)) +
    theme_bw()
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11-1.png)


And adding in the `facet_grid` it still runs fast and the plots look very very similar to what they would look like using the original data.

```r
qx <- 
  t %>%
    nest(-y,
         -w,
         -z) %>%
    mutate(Quantiles = map(data, ~ quantile(.$x, probs = seq(0, 1, by = 0.0001)))) %>% 
    unnest(map(Quantiles, tidy))

qx %>% 
  ggplot(aes(x = x, colour = y, fill = y)) +
    geom_density(alpha = 0.2, adjust = 1) +
    xlim(c(-10, 10)) +
    theme_bw() +
    facet_grid(w ~ z,
               margins = TRUE)
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12-1.png)

And the original was

```r
p
```

![plot of chunk unnamed-chunk-13](figure/unnamed-chunk-13-1.png)


As always stackoverflow crowd provided 90% of [the solution](https://stackoverflow.com/questions/30488389/using-dplyr-window-functions-to-calculate-percentiles).
