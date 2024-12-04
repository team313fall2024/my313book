---
title: "More Data Wrangling"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---

In this worksheet, we will:

-   Recode some variables for easier visualizations
-   Discuss how to handle missing values

## 0. Dataset and Library

We will again consider the built-in dataset `txhousing`, available in the `tidyverse` package:


```r
# Upload the package
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 4.3.3
```

```
## Warning: package 'ggplot2' was built under R version 4.3.3
```

```
## Warning: package 'tibble' was built under R version 4.3.3
```

```
## Warning: package 'tidyr' was built under R version 4.3.3
```

```
## Warning: package 'readr' was built under R version 4.3.3
```

```
## Warning: package 'purrr' was built under R version 4.3.3
```

```
## Warning: package 'stringr' was built under R version 4.3.3
```

```
## Warning: package 'forcats' was built under R version 4.3.3
```

```
## Warning: package 'lubridate' was built under R version 4.3.3
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
# Take a look at the first few rows of the dataset
head(txhousing) 
```

```
## # A tibble: 6 × 9
##   city     year month sales   volume median listings inventory  date
##   <chr>   <int> <int> <dbl>    <dbl>  <dbl>    <dbl>     <dbl> <dbl>
## 1 Abilene  2000     1    72  5380000  71400      701       6.3 2000 
## 2 Abilene  2000     2    98  6505000  58700      746       6.6 2000.
## 3 Abilene  2000     3   130  9285000  58100      784       6.8 2000.
## 4 Abilene  2000     4    98  9730000  68600      785       6.9 2000.
## 5 Abilene  2000     5   141 10590000  67300      794       6.8 2000.
## 6 Abilene  2000     6   156 13910000  66900      780       6.6 2000.
```

Let's further manipulate this dataset and create visualizations.

## 1. Recoding variables

There are many reasons why we may want to recode some variables. This list is not exhaustive!

### a. Log-transformation

When a variable is highly skewed, it might be difficult to "see" the variation:


```r
# Check this boxplot
txhousing |>
  ggplot() +
  geom_boxplot(aes(x = sales))
```

```
## Warning: Removed 568 rows containing non-finite outside the scale range
## (`stat_boxplot()`).
```

<img src="06_MoreDataWrangling_files/figure-html/unnamed-chunk-2-1.png" width="672" />

For distributions of numeric variables that are heavily right-skewed like this one we can apply a log-transformation:


```r
txhousing |>
  # Transform the sales variable
  mutate(log_sales = log(sales)) |>
  # Use this new variable in a ggplot
  ggplot() +
  geom_boxplot(aes(x = log_sales))
```

```
## Warning: Removed 568 rows containing non-finite outside the scale range
## (`stat_boxplot()`).
```

<img src="06_MoreDataWrangling_files/figure-html/unnamed-chunk-3-1.png" width="672" />

What is the unit of this new variable?

**Write sentences here.**

Note that this log-transformation only works for right-skewed data and can only be applied to values greater than 0.

### b. Numeric variable considered as a categorical variable

Some variables are coded numerically but would be better to represent as a category:


```r
# Check this plot
txhousing |>
  ggplot(aes(x = year, y = sales)) +
  geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'
```

```
## Warning: Removed 568 rows containing non-finite outside the scale range
## (`stat_smooth()`).
```

<img src="06_MoreDataWrangling_files/figure-html/unnamed-chunk-4-1.png" width="672" />

```r
txhousing |>
  # Color by year as a numeric variable
  ggplot(aes(x = year, y = sales, color = month)) +
  geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'
```

```
## Warning: Removed 568 rows containing non-finite outside the scale range
## (`stat_smooth()`).
```

```
## Warning: The following aesthetics were dropped during statistical transformation:
## colour.
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
```

<img src="06_MoreDataWrangling_files/figure-html/unnamed-chunk-4-2.png" width="672" />

```r
txhousing |>
  # Consider year as a factor (a type of categorical variable)
  mutate(month_fct = as.factor(month)) |>
  # Color by year as a categorical variable
  ggplot(aes(x = year, y = sales, color = month_fct)) +
  geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'
```

```
## Warning: Removed 568 rows containing non-finite outside the scale range
## (`stat_smooth()`).
```

<img src="06_MoreDataWrangling_files/figure-html/unnamed-chunk-4-3.png" width="672" />

The `as.numeric()` would do the opposite: make a variable numeric.

### c. Recoding values based on conditions

We can recode some values based on conditions with `case_when()`:


```r
txhousing |>
  # Recode months into 4 quarters
  mutate(month_cat = case_when(
    month <= 3 ~ "1st quarter",
    4 <= month & month <= 6 ~ "2nd quarter",
    7 <= month & month <= 9 ~ "3rd quarter",
    10 <= month & month <= 12 ~ "4th quarter")) |>
  # Use this new variable in a ggplot
  ggplot(aes(x = year, y = sales, color = month_cat)) +
  geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'
```

```
## Warning: Removed 568 rows containing non-finite outside the scale range
## (`stat_smooth()`).
```

<img src="06_MoreDataWrangling_files/figure-html/unnamed-chunk-5-1.png" width="672" />

Or we can recode each value with `recode()`:


```r
txhousing |>
  # Recode months with Spanish names
  mutate(month_es = recode(month,
                           `1` = "enero", `2` = "febrero", `3` = "marzo",
                           `4` = "abril", `5` = "mayo", `6` = "junio",
                           `7` = "julio", `8` = "agosto", `9` = "septiembre",
                           `10` = "octubre", `11` = "noviembre", `12` = "diciembre")) |>
  # You can control in which order the levels of a factor variable would appear
  mutate(month_es = factor(month_es, 
                           levels = c("enero", "febrero", "marzo", 
                                      "abril", "mayo", "junio",
                                      "julio", "agosto", "septiembre", 
                                      "octubre", "noviembre", "diciembre"))) |>
  # Use this new variable in a ggplot
  ggplot(aes(x = month_es, y = sales)) +
  geom_bar(stat = "summary", fun = sum)
```

```
## Warning: Removed 568 rows containing non-finite outside the scale range
## (`stat_summary()`).
```

<img src="06_MoreDataWrangling_files/figure-html/unnamed-chunk-6-1.png" width="672" />

## 2. Handling missing values

We talked about removing missing values in calculations with `na.rm = TRUE` for example. But simply ignoring missing data without trying to understand why the data might be missing could introduce some bias in our visualizations and statistics.

### a. Missing values never go unnoticed

When calculating statistics:


```r
txhousing |>
  # Find mean and correlation
  summarize(mean_sales = mean(sales),
            correlation = cor(sales,listings))
```

```
## # A tibble: 1 × 2
##   mean_sales correlation
##        <dbl>       <dbl>
## 1         NA          NA
```

When making visualizations:


```r
# Check the Warning message
txhousing |>
  ggplot() +
  geom_boxplot(aes(x = sales))
```

```
## Warning: Removed 568 rows containing non-finite outside the scale range
## (`stat_boxplot()`).
```

<img src="06_MoreDataWrangling_files/figure-html/unnamed-chunk-8-1.png" width="672" />

Before omitting the missing values, we should check how many values are missing and if there is any pattern in the missing data:


```r
txhousing |> 
  # Count missing values for each variable
  summarize_all(~ sum(is.na(.)))
```

```
## # A tibble: 1 × 9
##    city  year month sales volume median listings inventory  date
##   <int> <int> <int> <int>  <int>  <int>    <int>     <int> <int>
## 1     0     0     0   568    568    616     1424      1467     0
```


```r
txhousing |>
  # Split by year/month
  group_by(year,month) |>
  # Count missing values for each variable
  summarize_all(~ sum(is.na(.)))
```

```
## # A tibble: 187 × 9
## # Groups:   year [16]
##     year month  city sales volume median listings inventory  date
##    <int> <int> <int> <int>  <int>  <int>    <int>     <int> <int>
##  1  2000     1     0     8      8      9        9         9     0
##  2  2000     2     0     7      7      8        8         8     0
##  3  2000     3     0     8      8      9       10        10     0
##  4  2000     4     0     7      7      8        9         9     0
##  5  2000     5     0     7      7      9        8         8     0
##  6  2000     6     0     7      7      9       10        10     0
##  7  2000     7     0     8      8      9        9         9     0
##  8  2000     8     0     7      7      8        8         8     0
##  9  2000     9     0     7      7      8        9         9     0
## 10  2000    10     0     7      7      8       10        10     0
## # ℹ 177 more rows
```

Note that it looks like more recent data is not missing as much as older data.


```r
txhousing |>
  # Split by city
  group_by(city) |>
  # Count missing values for each variable
  summarize_all(~ sum(is.na(.)))
```

```
## # A tibble: 46 × 9
##    city                  year month sales volume median listings inventory  date
##    <chr>                <int> <int> <int>  <int>  <int>    <int>     <int> <int>
##  1 Abilene                  0     0     0      0      0        1         1     0
##  2 Amarillo                 0     0     0      0      0        5         5     0
##  3 Arlington                0     0     0      0      0        1         1     0
##  4 Austin                   0     0     0      0      0        0         0     0
##  5 Bay Area                 0     0     0      0      0        1         1     0
##  6 Beaumont                 0     0     0      0      0        0         0     0
##  7 Brazoria County          0     0    14     14     14       56        58     0
##  8 Brownsville              0     0     2      2      2      104       104     0
##  9 Bryan-College Stati…     0     0     0      0      0        0         0     0
## 10 Collin County            0     0     0      0      0        1         1     0
## # ℹ 36 more rows
```

Some cities don't have any data missing, others have only a few, and others have a lot!

### b. Values that were omitted

If we have monthly data in `txhousing` for 46 cities, from 2000 to 2015, how many rows should we expect?



The missing rows are due to the data only being reported until July of 2015. It would not make sense to compare yearly data in 2015 to other years.
