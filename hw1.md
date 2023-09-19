p8105_hw1_rl3328
================
Ruixi Li
2023-09-19

Load the moderndive library

``` r
library(moderndive)
library(tidyverse)
```

Load the early_january_weather dataset

``` r
data("early_january_weather")
```

# Problem 1

This problem focuses the use of inline R code, plotting, and the
behavior of ggplot for variables of different types.

## Short descriptive

``` r
colnames(early_january_weather)
```

    ##  [1] "origin"     "year"       "month"      "day"        "hour"      
    ##  [6] "temp"       "dewp"       "humid"      "wind_dir"   "wind_speed"
    ## [11] "wind_gust"  "precip"     "pressure"   "visib"      "time_hour"

``` r
nrow(early_january_weather)
```

    ## [1] 358

``` r
ncol(early_january_weather)
```

    ## [1] 15

``` r
mean(early_january_weather$temp)
```

    ## [1] 39.58212

- The variables name of this dataset are:origin, year, month, day, hour,
  temp, dewp, humid, wind_dir, wind_speed, wind_gust, precip, pressure,
  visib, time_hour.
- This data contains 358 entries and 15 columns.
- The mean temperature is 39.5821229.

## Scatterplot

we show a scatterplot of `temp` vs `time_hour`.

``` r
scatter_plot <-  ggplot(early_january_weather, aes(x = time_hour, y = temp, color = humid)) + geom_point()
print(scatter_plot)
```

![](hw1_files/figure-gfm/scatterplot-1.png)<!-- -->

``` r
ggsave("scatter_plot.pdf", height = 4, width = 6)
```

The temperature in LGA, JFK and EWR seems to increase as time(hour) pass
by in January 2013. The temperature decreases when there’s rainfall.

# Problem 2

This problem is intended to emphasize variable types and introduce
coercion; some awareness of how R treats numeric, character, and factor
variables is necessary for working with these data types in practice.

``` r
factor_levels <- c("Red", "Blue", "Green")

df <- tibble(
  sample = rnorm(10),
  logic = sample > 0,
  char_vector = c("A","B","C","D","E","F","G","H","I","J"),
  factor_vector = factor(sample(factor_levels, 10, replace = TRUE), levels = factor_levels) 
)
print(df)
```

    ## # A tibble: 10 × 4
    ##        sample logic char_vector factor_vector
    ##         <dbl> <lgl> <chr>       <fct>        
    ##  1 -0.247     FALSE A           Green        
    ##  2 -3.08      FALSE B           Red          
    ##  3 -0.363     FALSE C           Red          
    ##  4  0.212     TRUE  D           Green        
    ##  5  0.301     TRUE  E           Blue         
    ##  6  0.512     TRUE  F           Green        
    ##  7 -0.0000456 FALSE G           Blue         
    ##  8  1.19      TRUE  H           Green        
    ##  9  0.986     TRUE  I           Blue         
    ## 10  0.624     TRUE  J           Blue

## Take the mean of each variable in df

``` r
mean(pull(df,sample))
mean(pull(df,logic))
mean(pull(df,char_vector))
mean(pull(df,factor_vector))
```

- The mean for variable sample is 0.0125458.
- The mean for variable sample is 0.6.
- mean() function doesn’t work on character variables or factor
  variables.

## Convert variables from one type to another

``` r
as.numeric(pull(df,logic))
as.numeric(pull(df,char_vector))
as.numeric(pull(df,factor_vector))
```

- The logical variable and factor variable can be explicitly coverted
  into numeric, but the character variable cannot.
- It helps partly explain what happens when I try to take the mean.
  - mean() accepts numeric, When I call a function with an argument of
    the wrong type, R will try to coerce values to a different type so
    that the function will work. So, the logical variable is converted
    to numeric(TRUE-1,FALSE-0). That’s why mean() works for logical
    variables.
  - But that doesn’t explain why I cannot use mean() to calculate the
    mean of factor variables.factor variable is categorical
    variable(either ordinal or nominal), but as.numeric() treats factor
    variable as ordinal and apply numbers to it, which may be
    inappropriate.
