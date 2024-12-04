---
title: "Worksheet 6: Data Wrangling"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---

In this worksheet, we will:

-   Introduce `dplyr` functions to manipulate our dataset
-   Use the pipe `|>` to combine different steps
-   Think about the structure of our data

## 0. Dataset and Library

We will consider a built-in dataset that is available in the `tidyverse` package so first load the package:


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

The `txhousing` dataset contains information about the housing market in Texas between 2000 and 2015. Run `?txhousing` in your console for more details and take a look at the dataset:


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

Get information about the dimensions, types of variables, and some examples of values with `glimpse()`:


```r
# Dimensions and structure of the dataset
glimpse(txhousing) 
```

```
## Rows: 8,602
## Columns: 9
## $ city      <chr> "Abilene", "Abilene", "Abilene", "Abilene", "Abilene", "Abil…
## $ year      <int> 2000, 2000, 2000, 2000, 2000, 2000, 2000, 2000, 2000, 2000, …
## $ month     <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 1, 2, 3, 4, 5, 6, 7, …
## $ sales     <dbl> 72, 98, 130, 98, 141, 156, 152, 131, 104, 101, 100, 92, 75, …
## $ volume    <dbl> 5380000, 6505000, 9285000, 9730000, 10590000, 13910000, 1263…
## $ median    <dbl> 71400, 58700, 58100, 68600, 67300, 66900, 73500, 75000, 6450…
## $ listings  <dbl> 701, 746, 784, 785, 794, 780, 742, 765, 771, 764, 721, 658, …
## $ inventory <dbl> 6.3, 6.6, 6.8, 6.9, 6.8, 6.6, 6.2, 6.4, 6.5, 6.6, 6.2, 5.7, …
## $ date      <dbl> 2000.000, 2000.083, 2000.167, 2000.250, 2000.333, 2000.417, …
```

There are 8602 rows and 9 in the dataset.

What does one row represent?

**One city/year/month.**

Looking at the documentation with `?txhousing`, we are told that the information about the housing market in Texas was provided by the [TAMU real estate center](https://www.recenter.tamu.edu/). The variables are defined as follows:

| Variables       | Description                                                                                    |
|-----------------|------------------------------------------------------------------------------------------------|
| `city`          | Name of multiple listing service (MLS) area                                                    |
| `year`, `month` | Year, Month for the housing market data                                                        |
| `sales`         | Number of sales                                                                                |
| `volume`        | Total value of sales                                                                           |
| `median`        | Median sale price                                                                              |
| `listings`      | Total active listings                                                                          |
| `inventory`     | Amount of time (in months) it would take to sell all current listings at current pace of sales |
| `date`          | Date for the housing market data (`year` + `month` / 12)                                       |

Let's manipulate this dataset, with the 6 core `dplyr` functions.

## 1. The pipe

The pipe `|>` is a very important operator to build on code:


```r
# These two pieces of code are equivalent
summary(txhousing)
```

```
##      city                year          month            sales       
##  Length:8602        Min.   :2000   Min.   : 1.000   Min.   :   6.0  
##  Class :character   1st Qu.:2003   1st Qu.: 3.000   1st Qu.:  86.0  
##  Mode  :character   Median :2007   Median : 6.000   Median : 169.0  
##                     Mean   :2007   Mean   : 6.406   Mean   : 549.6  
##                     3rd Qu.:2011   3rd Qu.: 9.000   3rd Qu.: 467.0  
##                     Max.   :2015   Max.   :12.000   Max.   :8945.0  
##                                                     NA's   :568     
##      volume              median          listings       inventory     
##  Min.   :8.350e+05   Min.   : 50000   Min.   :    0   Min.   : 0.000  
##  1st Qu.:1.084e+07   1st Qu.:100000   1st Qu.:  682   1st Qu.: 4.900  
##  Median :2.299e+07   Median :123800   Median : 1283   Median : 6.200  
##  Mean   :1.069e+08   Mean   :128131   Mean   : 3217   Mean   : 7.175  
##  3rd Qu.:7.512e+07   3rd Qu.:150000   3rd Qu.: 2954   3rd Qu.: 8.150  
##  Max.   :2.568e+09   Max.   :304200   Max.   :43107   Max.   :55.900  
##  NA's   :568         NA's   :616      NA's   :1424    NA's   :1467    
##       date     
##  Min.   :2000  
##  1st Qu.:2004  
##  Median :2008  
##  Mean   :2008  
##  3rd Qu.:2012  
##  Max.   :2016  
## 
```

```r
txhousing |> summary()
```

```
##      city                year          month            sales       
##  Length:8602        Min.   :2000   Min.   : 1.000   Min.   :   6.0  
##  Class :character   1st Qu.:2003   1st Qu.: 3.000   1st Qu.:  86.0  
##  Mode  :character   Median :2007   Median : 6.000   Median : 169.0  
##                     Mean   :2007   Mean   : 6.406   Mean   : 549.6  
##                     3rd Qu.:2011   3rd Qu.: 9.000   3rd Qu.: 467.0  
##                     Max.   :2015   Max.   :12.000   Max.   :8945.0  
##                                                     NA's   :568     
##      volume              median          listings       inventory     
##  Min.   :8.350e+05   Min.   : 50000   Min.   :    0   Min.   : 0.000  
##  1st Qu.:1.084e+07   1st Qu.:100000   1st Qu.:  682   1st Qu.: 4.900  
##  Median :2.299e+07   Median :123800   Median : 1283   Median : 6.200  
##  Mean   :1.069e+08   Mean   :128131   Mean   : 3217   Mean   : 7.175  
##  3rd Qu.:7.512e+07   3rd Qu.:150000   3rd Qu.: 2954   3rd Qu.: 8.150  
##  Max.   :2.568e+09   Max.   :304200   Max.   :43107   Max.   :55.900  
##  NA's   :568         NA's   :616      NA's   :1424    NA's   :1467    
##       date     
##  Min.   :2000  
##  1st Qu.:2004  
##  Median :2008  
##  Mean   :2008  
##  3rd Qu.:2012  
##  Max.   :2016  
## 
```

The pipe makes code more readable by avoiding the need to nest functions inside each other and allows for a cleaner and more intuitive way to chain operations.

## 2. Operations on rows/observations

Let's consider some `dplyr` functions that apply to the rows/observations of our dataset.

### a. Filter

Use `filter()` to choose rows/observations verifying some conditions:


```r
# Filter with one criteria
txhousing |>
  filter(city == "Austin")
```

```
## # A tibble: 187 × 9
##    city    year month sales    volume median listings inventory  date
##    <chr>  <int> <int> <dbl>     <dbl>  <dbl>    <dbl>     <dbl> <dbl>
##  1 Austin  2000     1  1025 173053635 133700     3084       2   2000 
##  2 Austin  2000     2  1277 226038438 134000     2989       2   2000.
##  3 Austin  2000     3  1603 298557656 136700     3042       2   2000.
##  4 Austin  2000     4  1556 289197960 136900     3192       2.1 2000.
##  5 Austin  2000     5  1980 393073774 144700     3617       2.3 2000.
##  6 Austin  2000     6  1885 368290072 148800     3799       2.4 2000.
##  7 Austin  2000     7  1818 351539312 149300     3944       2.6 2000.
##  8 Austin  2000     8  1880 360255090 146300     3948       2.6 2001.
##  9 Austin  2000     9  1498 292799874 148700     4058       2.6 2001.
## 10 Austin  2000    10  1524 300952544 150100     4100       2.6 2001.
## # ℹ 177 more rows
```


```r
# Filter with multiple criteria
txhousing |>
  filter(city == "Austin", sales <= 1000)
```

```
## # A tibble: 2 × 9
##   city    year month sales    volume median listings inventory  date
##   <chr>  <int> <int> <dbl>     <dbl>  <dbl>    <dbl>     <dbl> <dbl>
## 1 Austin  2009     1   914 205512450 176000    10373       5.7  2009
## 2 Austin  2010     1   985 230799130 175300     9931       5.7  2010
```

```r
# Same as 
txhousing |>
  filter(city == "Austin" & sales <= 1000)
```

```
## # A tibble: 2 × 9
##   city    year month sales    volume median listings inventory  date
##   <chr>  <int> <int> <dbl>     <dbl>  <dbl>    <dbl>     <dbl> <dbl>
## 1 Austin  2009     1   914 205512450 176000    10373       5.7  2009
## 2 Austin  2010     1   985 230799130 175300     9931       5.7  2010
```

```r
# Same as 
txhousing |>
  filter(city == "Austin") |>
  filter(sales <= 1000)
```

```
## # A tibble: 2 × 9
##   city    year month sales    volume median listings inventory  date
##   <chr>  <int> <int> <dbl>     <dbl>  <dbl>    <dbl>     <dbl> <dbl>
## 1 Austin  2009     1   914 205512450 176000    10373       5.7  2009
## 2 Austin  2010     1   985 230799130 175300     9931       5.7  2010
```

We can check the number of rows that satisfy the conditions by pipping into `nrow()`:


```r
# Filter to check missing values for one variable
txhousing |>
  # Filter with multiple criteria
  filter(city == "Austin", sales <= 1000) |>
  # Count the rows
  nrow()
```

```
## [1] 2
```

Only 2 rows satisfied these conditions!

### b. Arrange

Use `arrange()` to sort rows/observations for some variables. Default is ascending (from least to greatest or alphabetically for categories) but to sort in the other direction use `desc()`.


```r
# Sort by number of sales, least-to-greatest 
txhousing |>
  arrange(sales)
```

```
## # A tibble: 8,602 × 9
##    city                year month sales  volume median listings inventory  date
##    <chr>              <int> <int> <dbl>   <dbl>  <dbl>    <dbl>     <dbl> <dbl>
##  1 San Marcos          2011    10     6 1156999 180000      163       8.3 2012.
##  2 Harlingen           2000     7     9 1110000  87500      719      30.8 2000.
##  3 South Padre Island  2011     1     9 2088500 225000     1258      55.7 2011 
##  4 San Marcos          2011     1    10 1482310 140000      165       7.5 2011 
##  5 San Marcos          2011    12    10 1561250 140000      148       8   2012.
##  6 San Marcos          2014    11    10 1506878 146700       96       4   2015.
##  7 South Padre Island  2010     1    10 2543721 200000     1290      NA   2010 
##  8 South Padre Island  2010    11    10 2098500 160000     1280      55.9 2011.
##  9 San Marcos          2001     2    11 1445000 126000      260       8.7 2001.
## 10 San Marcos          2008    10    11 1675000 143300      160       7.4 2009.
## # ℹ 8,592 more rows
```


```r
# Sort by number of sales, greatest-to-least (descending order)
txhousing |> 
  arrange(desc(sales))
```

```
## # A tibble: 8,602 × 9
##    city     year month sales     volume median listings inventory  date
##    <chr>   <int> <int> <dbl>      <dbl>  <dbl>    <dbl>     <dbl> <dbl>
##  1 Houston  2015     7  8945 2568156780 217600    23875       3.4 2016.
##  2 Houston  2006     6  8628 1795898108 155200    36281       5.6 2006.
##  3 Houston  2013     7  8468 2168720825 187800    21497       3.3 2014.
##  4 Houston  2015     6  8449 2490238594 222400    22311       3.2 2015.
##  5 Houston  2013     5  8439 2121508529 186100    20526       3.3 2013.
##  6 Houston  2014     6  8391 2342443127 211200    19725       2.9 2014.
##  7 Houston  2014     7  8391 2278932511 199700    20214       3   2014.
##  8 Houston  2014     8  8167 2195184825 202400    20007       2.9 2015.
##  9 Houston  2013     8  8155 2083377894 186700    21366       3.3 2014.
## 10 Houston  2006     5  8040 1602621368 151200    35398       5.5 2006.
## # ℹ 8,592 more rows
```

### c. Minimum/Maximum values

Let's try `top_n()` vs `slice_max()`/`slice_min()`, and `top_frac()`.


```r
# Select top rows (max values) for a variable
txhousing |> 
  # Use `top_n(number of rows, variables)`
  top_n(n = 4, sales)
```

```
## # A tibble: 4 × 9
##   city     year month sales     volume median listings inventory  date
##   <chr>   <int> <int> <dbl>      <dbl>  <dbl>    <dbl>     <dbl> <dbl>
## 1 Houston  2006     6  8628 1795898108 155200    36281       5.6 2006.
## 2 Houston  2013     7  8468 2168720825 187800    21497       3.3 2014.
## 3 Houston  2015     6  8449 2490238594 222400    22311       3.2 2015.
## 4 Houston  2015     7  8945 2568156780 217600    23875       3.4 2016.
```

How does it differ from `slice_max`?


```r
# Select top percent of rows (max values) for a variable
txhousing |>
  # Use `slice_max(number of rows, variables)`
  slice_max(n = 4, sales)
```

```
## # A tibble: 4 × 9
##   city     year month sales     volume median listings inventory  date
##   <chr>   <int> <int> <dbl>      <dbl>  <dbl>    <dbl>     <dbl> <dbl>
## 1 Houston  2015     7  8945 2568156780 217600    23875       3.4 2016.
## 2 Houston  2006     6  8628 1795898108 155200    36281       5.6 2006.
## 3 Houston  2013     7  8468 2168720825 187800    21497       3.3 2014.
## 4 Houston  2015     6  8449 2490238594 222400    22311       3.2 2015.
```


```r
# Select bottom rows (min values) for a variable
txhousing |> 
  # Use `top_n(-number of rows, variables)`
  top_n(n = -4, sales)
```

```
## # A tibble: 8 × 9
##   city                year month sales  volume median listings inventory  date
##   <chr>              <int> <int> <dbl>   <dbl>  <dbl>    <dbl>     <dbl> <dbl>
## 1 Harlingen           2000     7     9 1110000  87500      719      30.8 2000.
## 2 San Marcos          2011     1    10 1482310 140000      165       7.5 2011 
## 3 San Marcos          2011    10     6 1156999 180000      163       8.3 2012.
## 4 San Marcos          2011    12    10 1561250 140000      148       8   2012.
## 5 San Marcos          2014    11    10 1506878 146700       96       4   2015.
## 6 South Padre Island  2010     1    10 2543721 200000     1290      NA   2010 
## 7 South Padre Island  2010    11    10 2098500 160000     1280      55.9 2011.
## 8 South Padre Island  2011     1     9 2088500 225000     1258      55.7 2011
```

```r
# Why did we get more than 4 rows?
```


```r
# Select top percent of rows (max values) for a variable
txhousing |>
  # Use `top_frac(proportion of rows, variables)`
  top_frac(n = 0.001, sales)
```

```
## # A tibble: 8 × 9
##   city     year month sales     volume median listings inventory  date
##   <chr>   <int> <int> <dbl>      <dbl>  <dbl>    <dbl>     <dbl> <dbl>
## 1 Houston  2006     6  8628 1795898108 155200    36281       5.6 2006.
## 2 Houston  2013     5  8439 2121508529 186100    20526       3.3 2013.
## 3 Houston  2013     7  8468 2168720825 187800    21497       3.3 2014.
## 4 Houston  2014     6  8391 2342443127 211200    19725       2.9 2014.
## 5 Houston  2014     7  8391 2278932511 199700    20214       3   2014.
## 6 Houston  2014     8  8167 2195184825 202400    20007       2.9 2015.
## 7 Houston  2015     6  8449 2490238594 222400    22311       3.2 2015.
## 8 Houston  2015     7  8945 2568156780 217600    23875       3.4 2016.
```

```r
# Note: for the minimum values add `-` in front of the proportion
```

------------------------------------------------------------------------

#### **Try it! When were the lowest 5 numbers of sales for Austin? Display them in chronological order.**


```r
# Write and submit code here!
```

**Write sentences here.**

------------------------------------------------------------------------

## 3. Operations on columns

Let's consider some `dplyr` functions that apply to the columns/variables of our dataset.

### a. Select

Use `select()` to keep or rename a subset of columns/variables.


```r
# Select to keep only some variables
txhousing |>
  # Only see 4 variables
  select(city, year, month, sales)
```

```
## # A tibble: 8,602 × 4
##    city     year month sales
##    <chr>   <int> <int> <dbl>
##  1 Abilene  2000     1    72
##  2 Abilene  2000     2    98
##  3 Abilene  2000     3   130
##  4 Abilene  2000     4    98
##  5 Abilene  2000     5   141
##  6 Abilene  2000     6   156
##  7 Abilene  2000     7   152
##  8 Abilene  2000     8   131
##  9 Abilene  2000     9   104
## 10 Abilene  2000    10   101
## # ℹ 8,592 more rows
```


```r
# Select to keep columns using indexes of the columns
txhousing |>
  select(1:4,6)
```

```
## # A tibble: 8,602 × 5
##    city     year month sales median
##    <chr>   <int> <int> <dbl>  <dbl>
##  1 Abilene  2000     1    72  71400
##  2 Abilene  2000     2    98  58700
##  3 Abilene  2000     3   130  58100
##  4 Abilene  2000     4    98  68600
##  5 Abilene  2000     5   141  67300
##  6 Abilene  2000     6   156  66900
##  7 Abilene  2000     7   152  73500
##  8 Abilene  2000     8   131  75000
##  9 Abilene  2000     9   104  64500
## 10 Abilene  2000    10   101  59300
## # ℹ 8,592 more rows
```


```r
# Drop variables using "-"
txhousing |>
  # See all but these 4 variables
  select(-city, -year, -month, -date)
```

```
## # A tibble: 8,602 × 5
##    sales   volume median listings inventory
##    <dbl>    <dbl>  <dbl>    <dbl>     <dbl>
##  1    72  5380000  71400      701       6.3
##  2    98  6505000  58700      746       6.6
##  3   130  9285000  58100      784       6.8
##  4    98  9730000  68600      785       6.9
##  5   141 10590000  67300      794       6.8
##  6   156 13910000  66900      780       6.6
##  7   152 12635000  73500      742       6.2
##  8   131 10710000  75000      765       6.4
##  9   104  7615000  64500      771       6.5
## 10   101  7040000  59300      764       6.6
## # ℹ 8,592 more rows
```


```r
# Select and rename...
txhousing |>
  # Use `select()` to rename some variables new_name = old_name
  select(Location = city, 
         Calendar_Year = year,
         Month = month,
         Number_of_sales = sales)
```

```
## # A tibble: 8,602 × 4
##    Location Calendar_Year Month Number_of_sales
##    <chr>            <int> <int>           <dbl>
##  1 Abilene           2000     1              72
##  2 Abilene           2000     2              98
##  3 Abilene           2000     3             130
##  4 Abilene           2000     4              98
##  5 Abilene           2000     5             141
##  6 Abilene           2000     6             156
##  7 Abilene           2000     7             152
##  8 Abilene           2000     8             131
##  9 Abilene           2000     9             104
## 10 Abilene           2000    10             101
## # ℹ 8,592 more rows
```


```r
# or just use rename() with the same structure
txhousing |> 
  rename(Location = city, 
         Calendar_Year = year,
         Month = month,
         Number_of_sales = sales)
```

```
## # A tibble: 8,602 × 9
##    Location Calendar_Year Month Number_of_sales volume median listings inventory
##    <chr>            <int> <int>           <dbl>  <dbl>  <dbl>    <dbl>     <dbl>
##  1 Abilene           2000     1              72 5.38e6  71400      701       6.3
##  2 Abilene           2000     2              98 6.51e6  58700      746       6.6
##  3 Abilene           2000     3             130 9.28e6  58100      784       6.8
##  4 Abilene           2000     4              98 9.73e6  68600      785       6.9
##  5 Abilene           2000     5             141 1.06e7  67300      794       6.8
##  6 Abilene           2000     6             156 1.39e7  66900      780       6.6
##  7 Abilene           2000     7             152 1.26e7  73500      742       6.2
##  8 Abilene           2000     8             131 1.07e7  75000      765       6.4
##  9 Abilene           2000     9             104 7.62e6  64500      771       6.5
## 10 Abilene           2000    10             101 7.04e6  59300      764       6.6
## # ℹ 8,592 more rows
## # ℹ 1 more variable: date <dbl>
```

### b. Mutate

Use `mutate()` to create new columns/variables:


```r
# Find the mean sale price per row
txhousing |> 
  mutate(mean_price = volume/sales)
```

```
## # A tibble: 8,602 × 10
##    city     year month sales   volume median listings inventory  date mean_price
##    <chr>   <int> <int> <dbl>    <dbl>  <dbl>    <dbl>     <dbl> <dbl>      <dbl>
##  1 Abilene  2000     1    72  5380000  71400      701       6.3 2000      74722.
##  2 Abilene  2000     2    98  6505000  58700      746       6.6 2000.     66378.
##  3 Abilene  2000     3   130  9285000  58100      784       6.8 2000.     71423.
##  4 Abilene  2000     4    98  9730000  68600      785       6.9 2000.     99286.
##  5 Abilene  2000     5   141 10590000  67300      794       6.8 2000.     75106.
##  6 Abilene  2000     6   156 13910000  66900      780       6.6 2000.     89167.
##  7 Abilene  2000     7   152 12635000  73500      742       6.2 2000.     83125 
##  8 Abilene  2000     8   131 10710000  75000      765       6.4 2001.     81756.
##  9 Abilene  2000     9   104  7615000  64500      771       6.5 2001.     73221.
## 10 Abilene  2000    10   101  7040000  59300      764       6.6 2001.     69703.
## # ℹ 8,592 more rows
```

------------------------------------------------------------------------

#### **Try it! What's the difference between the average price as calculated above and the median sale price? Are these two measures the same? Why/Why not?**


```r
# Write and submit code here!
txhousing |> 
  mutate(mean_price = volume/sales,
         diff_price = mean_price - median) |>
  filter(diff_price == 0)
```

```
## # A tibble: 3 × 11
##   city        year month sales volume median listings inventory  date mean_price
##   <chr>      <int> <int> <dbl>  <dbl>  <dbl>    <dbl>     <dbl> <dbl>      <dbl>
## 1 Lufkin      2009     9    46 5.52e6 120000       NA      NA   2010.     120000
## 2 Nacogdoch…  2008     1    17 2.55e6 150000      295       8.9 2008      150000
## 3 San Marcos  2007     1    16 2.56e6 160000       NA      NA   2007      160000
## # ℹ 1 more variable: diff_price <dbl>
```

**Write sentences here.**

------------------------------------------------------------------------

## 4. Create summaries

Let's consider some `dplyr` functions that can create some summaries for our dataset.

### a. Summarize

Use `summarize()` (or `summarise()` in British!) to calculate summary statistics on columns/variables. Some useful summary functions: `mean()`, `sd()`, `median()`, `IQR()`, `min()`, `max()`, `n()`, `n_distinct()`, `cor()`, ...


```r
# Find the mean number of sales
txhousing |>
  summarize(mean_sales = mean(sales, na.rm = T)) # ignore NA values
```

```
## # A tibble: 1 × 1
##   mean_sales
##        <dbl>
## 1       550.
```


```r
# Add more summaries:
txhousing |>
  summarize(
    # the mean
    mean_sales = mean(sales, na.rm = T), 
    # the median
    median_sales = median(sales, na.rm = T),
    # the number of rows
    n_rows = n(),
    # the number of distinct cities in the dataset
    n_cities = n_distinct(city),
    # the correlation between sales and median price
    correlation = cor(sales, median, use = "complete.obs"))
```

```
## # A tibble: 1 × 5
##   mean_sales median_sales n_rows n_cities correlation
##        <dbl>        <dbl>  <int>    <int>       <dbl>
## 1       550.          169   8602       46       0.345
```

------------------------------------------------------------------------

#### **Try it! Find the total number of `sales` for Austin in 2009.**


```r
# Write and submit code here!
txhousing |>
  filter(city == "Austin" & year == 2010) |>
  summarize(total_sales = sum(sales))
```

```
## # A tibble: 1 × 1
##   total_sales
##         <dbl>
## 1       19872
```

**Write sentences here.**

------------------------------------------------------------------------

What if we wanted to generate a similar report for each year across all cities in `txhousing`? Let's use a function that allows us to create summaries per subgroup.

### b. Group by

This is one very important function! It enables us to create subgroups and apply a function to all these subgroups For example, find summaries per city and per year:


```r
# Find summaries by subgroups 
txhousing |>
  # Each year is a subgroup
  group_by(year) |> 
  # Create summaries for each subgroup
  summarize(total_sales = sum(sales, na.rm = TRUE), # total number of sales
            nb_rows = n()) # count how many rows in each subset 
```

```
## # A tibble: 16 × 3
##     year total_sales nb_rows
##    <int>       <dbl>   <int>
##  1  2000      222483     552
##  2  2001      231453     552
##  3  2002      234600     552
##  4  2003      253909     552
##  5  2004      283999     552
##  6  2005      316046     552
##  7  2006      347733     552
##  8  2007      327701     552
##  9  2008      276664     552
## 10  2009      255254     552
## 11  2010      243014     552
## 12  2011      246356     552
## 13  2012      287052     552
## 14  2013      335094     552
## 15  2014      345720     552
## 16  2015      208124     322
```

Note that there are less rows in 2015. How could it influence the total number of sales during that year?

**Total in 2015 would be less.**

Let’s try to be a little more specific and find the total number of sales per year and per month:


```r
txhousing |>
  # Each year/month is a subgroup
  group_by(year,month) |> 
  # Create summaries for each subgroup
  summarize(total_sales = sum(sales, na.rm = TRUE), # total number of sales
            nb_rows = n()) # count how many rows in each subset 
```

```
## `summarise()` has grouped output by 'year'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 187 × 4
## # Groups:   year [16]
##     year month total_sales nb_rows
##    <int> <int>       <dbl>   <int>
##  1  2000     1       11411      46
##  2  2000     2       15674      46
##  3  2000     3       20202      46
##  4  2000     4       18658      46
##  5  2000     5       22388      46
##  6  2000     6       23488      46
##  7  2000     7       21104      46
##  8  2000     8       22289      46
##  9  2000     9       17987      46
## 10  2000    10       17396      46
## # ℹ 177 more rows
```

------------------------------------------------------------------------

#### **Try it! Find the total number of sales per month in `txhousing`, but ignoring values from 2015 since there are some months missing. Then create a `ggplot` to show how the number of sales may vary per month.**


```r
# Write and submit code here!
```

**Write sentences here.**

------------------------------------------------------------------------

Check out the [dplyr Cheat sheet](https://dplyr.tidyverse.org/#cheat-sheet) for more practice!
