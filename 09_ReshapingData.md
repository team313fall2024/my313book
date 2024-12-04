---
title: "Reshaping Data"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---



In this worksheet, we will:

-   Understand the principles of tidy data
-   Discuss some functions that can help us tidy our data
-   Identify when we need to tidy data

## 0. Datasets and Libraries

Today we will manipulate datasets that are part of the `tidyr` package, included in `tidyverse`:


```r
# Load a package
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

The following tables all display the number of tuberculosis (TB) cases documented by the World Health Organization in Afghanistan, Brazil, and China between 1999 and 2000. They contain values associated with four variables (country, year, cases, and population), but each table organizes the values in a different way:


```r
# Open the different tabular representations of the tuberculosis data
table1
```

```
## # A tibble: 6 × 4
##   country      year  cases population
##   <chr>       <dbl>  <dbl>      <dbl>
## 1 Afghanistan  1999    745   19987071
## 2 Afghanistan  2000   2666   20595360
## 3 Brazil       1999  37737  172006362
## 4 Brazil       2000  80488  174504898
## 5 China        1999 212258 1272915272
## 6 China        2000 213766 1280428583
```

```r
table2
```

```
## # A tibble: 12 × 4
##    country      year type            count
##    <chr>       <dbl> <chr>           <dbl>
##  1 Afghanistan  1999 cases             745
##  2 Afghanistan  1999 population   19987071
##  3 Afghanistan  2000 cases            2666
##  4 Afghanistan  2000 population   20595360
##  5 Brazil       1999 cases           37737
##  6 Brazil       1999 population  172006362
##  7 Brazil       2000 cases           80488
##  8 Brazil       2000 population  174504898
##  9 China        1999 cases          212258
## 10 China        1999 population 1272915272
## 11 China        2000 cases          213766
## 12 China        2000 population 1280428583
```

```r
table3
```

```
## # A tibble: 6 × 3
##   country      year rate             
##   <chr>       <dbl> <chr>            
## 1 Afghanistan  1999 745/19987071     
## 2 Afghanistan  2000 2666/20595360    
## 3 Brazil       1999 37737/172006362  
## 4 Brazil       2000 80488/174504898  
## 5 China        1999 212258/1272915272
## 6 China        2000 213766/1280428583
```

```r
table4a
```

```
## # A tibble: 3 × 3
##   country     `1999` `2000`
##   <chr>        <dbl>  <dbl>
## 1 Afghanistan    745   2666
## 2 Brazil       37737  80488
## 3 China       212258 213766
```

```r
table4b
```

```
## # A tibble: 3 × 3
##   country         `1999`     `2000`
##   <chr>            <dbl>      <dbl>
## 1 Afghanistan   19987071   20595360
## 2 Brazil       172006362  174504898
## 3 China       1272915272 1280428583
```

```r
table5
```

```
## # A tibble: 6 × 4
##   country     century year  rate             
##   <chr>       <chr>   <chr> <chr>            
## 1 Afghanistan 19      99    745/19987071     
## 2 Afghanistan 20      00    2666/20595360    
## 3 Brazil      19      99    37737/172006362  
## 4 Brazil      20      00    80488/174504898  
## 5 China       19      99    212258/1272915272
## 6 China       20      00    213766/1280428583
```

Which of these tables are tidy?

**Just the first one.**

## 1. Pivoting

### a. Wide to long

Let's focus on `table4a`:


```r
# Look at table4a - Tuberculosis cases
table4a
```

```
## # A tibble: 3 × 3
##   country     `1999` `2000`
##   <chr>        <dbl>  <dbl>
## 1 Afghanistan    745   2666
## 2 Brazil       37737  80488
## 3 China       212258 213766
```

The function `pivot_longer()` makes a dataset "longer" by increasing the number of rows and decreasing the number of columns.


```r
# Use pivot_longer() to have an observation for each country/year
newtable4a <-  pivot_longer(table4a,
                            # Columns in table4a to put as rows
                            cols = c(`1999`, `2000`), # use `` for unconventional variable names
                            # Save the columns 1999 and 2000 as values of a variable `year`
                            names_to = "year", 
                            # Save the cell values as the values of a variable `cases`
                            values_to = "cases") 
newtable4a
```

```
## # A tibble: 6 × 3
##   country     year   cases
##   <chr>       <chr>  <dbl>
## 1 Afghanistan 1999     745
## 2 Afghanistan 2000    2666
## 3 Brazil      1999   37737
## 4 Brazil      2000   80488
## 5 China       1999  212258
## 6 China       2000  213766
```

Here is how we pivoted `table4` into a longer, tidy form:

![](https://d33wubrfki0l68.cloudfront.net/3aea19108d39606bbe49981acda07696c0c7fcd8/2de65/images/tidy-9.png)

#### **Try it! Do the same for `table4b`. Think about what the numbers represent in that table to name the variable appropriately and name the resulting dataset as `newtable4b`. Then join `newtable4a` and `newtable4b`. Is the joined dataset tidy? If so, calculate the rate of TB cases (`cases` divided by `population`).**


```r
# Write and submit code here!
newtable4b <-  pivot_longer(table4b,
                            # Columns in table4a to put as rows
                            cols = c(`1999`, `2000`), # use `` for unconventional variable names
                            # Save the columns 1999 and 2000 as values of a variable `year`
                            names_to = "year", 
                            # Save the cell values as the values of a variable `cases`
                            values_to = "population") 

full_join(newtable4a,newtable4b, by = c("country", "year")) |>
  mutate(rate = cases / population * 100000)
```

```
## # A tibble: 6 × 5
##   country     year   cases population  rate
##   <chr>       <chr>  <dbl>      <dbl> <dbl>
## 1 Afghanistan 1999     745   19987071  3.73
## 2 Afghanistan 2000    2666   20595360 12.9 
## 3 Brazil      1999   37737  172006362 21.9 
## 4 Brazil      2000   80488  174504898 46.1 
## 5 China       1999  212258 1272915272 16.7 
## 6 China       2000  213766 1280428583 16.7
```

**Write sentences here.**

What if we had joined `table4a` and `table4b` before tidying?


```r
# Take a look at both tables again
table4a
```

```
## # A tibble: 3 × 3
##   country     `1999` `2000`
##   <chr>        <dbl>  <dbl>
## 1 Afghanistan    745   2666
## 2 Brazil       37737  80488
## 3 China       212258 213766
```

```r
table4b
```

```
## # A tibble: 3 × 3
##   country         `1999`     `2000`
##   <chr>            <dbl>      <dbl>
## 1 Afghanistan   19987071   20595360
## 2 Brazil       172006362  174504898
## 3 China       1272915272 1280428583
```


```r
# Join untidy tables
inner_join(table4a, table4b, by = "country")
```

```
## # A tibble: 3 × 5
##   country     `1999.x` `2000.x`   `1999.y`   `2000.y`
##   <chr>          <dbl>    <dbl>      <dbl>      <dbl>
## 1 Afghanistan      745     2666   19987071   20595360
## 2 Brazil         37737    80488  172006362  174504898
## 3 China         212258   213766 1272915272 1280428583
```

```r
# Improve how we join these untidy tables
joined_untidy <- inner_join(table4a, table4b, by = "country",
                            # Adding explicit suffixes
                            suffix = c("_cases", "_population"))
joined_untidy
```

```
## # A tibble: 3 × 5
##   country     `1999_cases` `2000_cases` `1999_population` `2000_population`
##   <chr>              <dbl>        <dbl>             <dbl>             <dbl>
## 1 Afghanistan          745         2666          19987071          20595360
## 2 Brazil             37737        80488         172006362         174504898
## 3 China             212258       213766        1272915272        1280428583
```

Now, let's try to tidy the joined dataset:


```r
# Using pivot_longer() on all columns
pivot_longer(joined_untidy, 
             # Refer to all variables with `:`
             cols = c('1999_cases':'2000_population'))
```

```
## # A tibble: 12 × 3
##    country     name                 value
##    <chr>       <chr>                <dbl>
##  1 Afghanistan 1999_cases             745
##  2 Afghanistan 2000_cases            2666
##  3 Afghanistan 1999_population   19987071
##  4 Afghanistan 2000_population   20595360
##  5 Brazil      1999_cases           37737
##  6 Brazil      2000_cases           80488
##  7 Brazil      1999_population  172006362
##  8 Brazil      2000_population  174504898
##  9 China       1999_cases          212258
## 10 China       2000_cases          213766
## 11 China       1999_population 1272915272
## 12 China       2000_population 1280428583
```

We need to split values for the variable `name` like 1999.cases into two columns (one for `year`, one for `cases`/`population`). The function `separate()` can find the separator automatically (or we can specify the separator with `sep =`):


```r
# Using pivot_longer() on all columns
pivot_longer(joined_untidy, cols = c('1999_cases':'2000_population')) |>
  # Distinguish between year and cases/population variables
  separate(name, sep = "_", into = c("year", "type"))
```

```
## # A tibble: 12 × 4
##    country     year  type            value
##    <chr>       <chr> <chr>           <dbl>
##  1 Afghanistan 1999  cases             745
##  2 Afghanistan 2000  cases            2666
##  3 Afghanistan 1999  population   19987071
##  4 Afghanistan 2000  population   20595360
##  5 Brazil      1999  cases           37737
##  6 Brazil      2000  cases           80488
##  7 Brazil      1999  population  172006362
##  8 Brazil      2000  population  174504898
##  9 China       1999  cases          212258
## 10 China       2000  cases          213766
## 11 China       1999  population 1272915272
## 12 China       2000  population 1280428583
```

The column `value` does not refer to a unique variable and each row does not represent one observation of country/year (for example, Afghanistan in 1999 is represented by 2 rows) We need to make the dataset *wider*.

### b. Long to wide

The last dataset we created above is actually called `table2` in `tidyr`.


```r
# Take a look at table2
table2
```

```
## # A tibble: 12 × 4
##    country      year type            count
##    <chr>       <dbl> <chr>           <dbl>
##  1 Afghanistan  1999 cases             745
##  2 Afghanistan  1999 population   19987071
##  3 Afghanistan  2000 cases            2666
##  4 Afghanistan  2000 population   20595360
##  5 Brazil       1999 cases           37737
##  6 Brazil       1999 population  172006362
##  7 Brazil       2000 cases           80488
##  8 Brazil       2000 population  174504898
##  9 China        1999 cases          212258
## 10 China        1999 population 1272915272
## 11 China        2000 cases          213766
## 12 China        2000 population 1280428583
```

The function `pivot_wider()` makes datasets wider by increasing the number of columns and decreasing the number of rows.


```r
# Use pivot_wider() to have a variable for the number of cases and one for population
pivot_wider(table2, 
            # the values of the variable `type` will become variables 
            names_from = type, 
            # the cell values values of `count` will match the corresponding variable
            values_from = count) 
```

```
## # A tibble: 6 × 4
##   country      year  cases population
##   <chr>       <dbl>  <dbl>      <dbl>
## 1 Afghanistan  1999    745   19987071
## 2 Afghanistan  2000   2666   20595360
## 3 Brazil       1999  37737  172006362
## 4 Brazil       2000  80488  174504898
## 5 China        1999 212258 1272915272
## 6 China        2000 213766 1280428583
```

Here is how we pivoted `table2` into a wider, tidy form:

![](https://d33wubrfki0l68.cloudfront.net/8350f0dda414629b9d6c354f87acf5c5f722be43/bcb84/images/tidy-8.png)

#### **Try it! Do something similar for `table1` so we only have one row per country. Is this resulting data tidy? Why or why not?**


```r
# Write and submit code here!
pivot_wider(table1, 
            # the values of the variable `type` will become variables 
            names_from = year, 
            # the cell values values of `count` will match the corresponding variable
            values_from = c("cases","population")) 
```

```
## # A tibble: 3 × 5
##   country     cases_1999 cases_2000 population_1999 population_2000
##   <chr>            <dbl>      <dbl>           <dbl>           <dbl>
## 1 Afghanistan        745       2666        19987071        20595360
## 2 Brazil           37737      80488       172006362       174504898
## 3 China           212258     213766      1272915272      1280428583
```

**Write sentences here.**

## 2. Separating and uniting

Some other functions that can help make our data tidy are `separate()` (see above for an example) and `unite()`.

### a. Separate

As mentioned in 2.a, we can split a variable into two or more variables with `separate()`. R can find the separator automatically or you could specify the separator with the argument `sep = " "`.


```r
# Take a look at table3
table3
```

```
## # A tibble: 6 × 3
##   country      year rate             
##   <chr>       <dbl> <chr>            
## 1 Afghanistan  1999 745/19987071     
## 2 Afghanistan  2000 2666/20595360    
## 3 Brazil       1999 37737/172006362  
## 4 Brazil       2000 80488/174504898  
## 5 China        1999 212258/1272915272
## 6 China        2000 213766/1280428583
```

#### **Try it! Separate `rate` into two variables: `cases` and `population`. What do you notice about the type of the resulting variables? Why do you think that happened? Note: Add the argument `convert = TRUE` in `separate()` to convert the variables in the appropriate format.**


```r
# Write and submit code here!
table3 |>
  separate(rate, into = c("cases", "population"), sep = "/", convert = T) |>
  mutate(rate = cases/population)
```

```
## # A tibble: 6 × 5
##   country      year  cases population      rate
##   <chr>       <dbl>  <int>      <int>     <dbl>
## 1 Afghanistan  1999    745   19987071 0.0000373
## 2 Afghanistan  2000   2666   20595360 0.000129 
## 3 Brazil       1999  37737  172006362 0.000219 
## 4 Brazil       2000  80488  174504898 0.000461 
## 5 China        1999 212258 1272915272 0.000167 
## 6 China        2000 213766 1280428583 0.000167
```

**Write sentences here.**

### b. Unite

On the opposite, we can combine two variables into one with `unite()`.


```r
# Take a look at table5
table5
```

```
## # A tibble: 6 × 4
##   country     century year  rate             
##   <chr>       <chr>   <chr> <chr>            
## 1 Afghanistan 19      99    745/19987071     
## 2 Afghanistan 20      00    2666/20595360    
## 3 Brazil      19      99    37737/172006362  
## 4 Brazil      20      00    80488/174504898  
## 5 China       19      99    212258/1272915272
## 6 China       20      00    213766/1280428583
```

Let's gather `century` and `year`:


```r
# Use unite() to rejoin the variables century and year created above
unite(table5, new, century, year)
```

```
## # A tibble: 6 × 3
##   country     new   rate             
##   <chr>       <chr> <chr>            
## 1 Afghanistan 19_99 745/19987071     
## 2 Afghanistan 20_00 2666/20595360    
## 3 Brazil      19_99 37737/172006362  
## 4 Brazil      20_00 80488/174504898  
## 5 China       19_99 212258/1272915272
## 6 China       20_00 213766/1280428583
```

```r
# R places "_" automatically or you can specify a separator with `sep = ""`
unite(table5, new, century, year, sep = "") |>
  mutate(new = as.numeric(new))
```

```
## # A tibble: 6 × 3
##   country       new rate             
##   <chr>       <dbl> <chr>            
## 1 Afghanistan  1999 745/19987071     
## 2 Afghanistan  2000 2666/20595360    
## 3 Brazil       1999 37737/172006362  
## 4 Brazil       2000 80488/174504898  
## 5 China        1999 212258/1272915272
## 6 China        2000 213766/1280428583
```

------------------------------------------------------------------------

## Group Practice

Practice the `tidyr` functions we have learned on the `billboard` dataset. This built-in dataset contains songs rankings for Billboard top 100 in the year of 2000 for each week after it entered the Billboard (`wk1`-`wk76`).


```r
# Take a look at the dataset
head(billboard)
```

```
## # A tibble: 6 × 79
##   artist      track date.entered   wk1   wk2   wk3   wk4   wk5   wk6   wk7   wk8
##   <chr>       <chr> <date>       <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
## 1 2 Pac       Baby… 2000-02-26      87    82    72    77    87    94    99    NA
## 2 2Ge+her     The … 2000-09-02      91    87    92    NA    NA    NA    NA    NA
## 3 3 Doors Do… Kryp… 2000-04-08      81    70    68    67    66    57    54    53
## 4 3 Doors Do… Loser 2000-10-21      76    76    72    69    67    65    55    59
## 5 504 Boyz    Wobb… 2000-04-15      57    34    25    17    17    31    36    49
## 6 98^0        Give… 2000-08-19      51    39    34    26    26    19     2     2
## # ℹ 68 more variables: wk9 <dbl>, wk10 <dbl>, wk11 <dbl>, wk12 <dbl>,
## #   wk13 <dbl>, wk14 <dbl>, wk15 <dbl>, wk16 <dbl>, wk17 <dbl>, wk18 <dbl>,
## #   wk19 <dbl>, wk20 <dbl>, wk21 <dbl>, wk22 <dbl>, wk23 <dbl>, wk24 <dbl>,
## #   wk25 <dbl>, wk26 <dbl>, wk27 <dbl>, wk28 <dbl>, wk29 <dbl>, wk30 <dbl>,
## #   wk31 <dbl>, wk32 <dbl>, wk33 <dbl>, wk34 <dbl>, wk35 <dbl>, wk36 <dbl>,
## #   wk37 <dbl>, wk38 <dbl>, wk39 <dbl>, wk40 <dbl>, wk41 <dbl>, wk42 <dbl>,
## #   wk43 <dbl>, wk44 <dbl>, wk45 <dbl>, wk46 <dbl>, wk47 <dbl>, wk48 <dbl>, …
```

1.  Why is that data not technically tidy? Which `pivot_...()` function should we use to make `billboard` tidy? Try it!

**Write sentences here.**


```r
# Write and submit code here!
billboard
```

```
## # A tibble: 317 × 79
##    artist     track date.entered   wk1   wk2   wk3   wk4   wk5   wk6   wk7   wk8
##    <chr>      <chr> <date>       <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
##  1 2 Pac      Baby… 2000-02-26      87    82    72    77    87    94    99    NA
##  2 2Ge+her    The … 2000-09-02      91    87    92    NA    NA    NA    NA    NA
##  3 3 Doors D… Kryp… 2000-04-08      81    70    68    67    66    57    54    53
##  4 3 Doors D… Loser 2000-10-21      76    76    72    69    67    65    55    59
##  5 504 Boyz   Wobb… 2000-04-15      57    34    25    17    17    31    36    49
##  6 98^0       Give… 2000-08-19      51    39    34    26    26    19     2     2
##  7 A*Teens    Danc… 2000-07-08      97    97    96    95   100    NA    NA    NA
##  8 Aaliyah    I Do… 2000-01-29      84    62    51    41    38    35    35    38
##  9 Aaliyah    Try … 2000-03-18      59    53    38    28    21    18    16    14
## 10 Adams, Yo… Open… 2000-08-26      76    76    74    69    68    67    61    58
## # ℹ 307 more rows
## # ℹ 68 more variables: wk9 <dbl>, wk10 <dbl>, wk11 <dbl>, wk12 <dbl>,
## #   wk13 <dbl>, wk14 <dbl>, wk15 <dbl>, wk16 <dbl>, wk17 <dbl>, wk18 <dbl>,
## #   wk19 <dbl>, wk20 <dbl>, wk21 <dbl>, wk22 <dbl>, wk23 <dbl>, wk24 <dbl>,
## #   wk25 <dbl>, wk26 <dbl>, wk27 <dbl>, wk28 <dbl>, wk29 <dbl>, wk30 <dbl>,
## #   wk31 <dbl>, wk32 <dbl>, wk33 <dbl>, wk34 <dbl>, wk35 <dbl>, wk36 <dbl>,
## #   wk37 <dbl>, wk38 <dbl>, wk39 <dbl>, wk40 <dbl>, wk41 <dbl>, wk42 <dbl>, …
```

```r
pivot_longer(billboard,
             cols = c(wk1:wk76), 
             names_to = "week",
             values_to = "ranking")
```

```
## # A tibble: 24,092 × 5
##    artist track                   date.entered week  ranking
##    <chr>  <chr>                   <date>       <chr>   <dbl>
##  1 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk1        87
##  2 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk2        82
##  3 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk3        72
##  4 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk4        77
##  5 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk5        87
##  6 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk6        94
##  7 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk7        99
##  8 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk8        NA
##  9 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk9        NA
## 10 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk10       NA
## # ℹ 24,082 more rows
```

2.  In which format should `billboard` be if we wanted to check at what rank songs enter the Billboard top 100? Try it! What is the average ranking on the first week? What is the best ranking on the first week?


```r
# Find the max of wk1
billboard |>
  summarize(best_rank_wk1 = min(wk1),
            avg_rank_wk1 = mean(wk1))
```

```
## # A tibble: 1 × 2
##   best_rank_wk1 avg_rank_wk1
##           <dbl>        <dbl>
## 1            15         80.0
```


3.  In which format should `billboard` be if we wanted to check when a song is more likely to be at the top of the Billboard? Try it! On which week a song is more likely to reach the top?


```r
# Use the tidy version!
pivot_longer(billboard,
             cols = c(wk1:wk76), 
             names_to = "week",
             values_to = "ranking") |>
  # Check when the ranking is at the top!
  filter(ranking == 1) |>
  # which week have the higher rank
  group_by(week) |>
  summarize(n = n()) |>
  filter(n > 1) |>
  arrange(desc(n))
```

```
## # A tibble: 12 × 2
##    week      n
##    <chr> <int>
##  1 wk13      8
##  2 wk14      7
##  3 wk11      5
##  4 wk15      5
##  5 wk9       5
##  6 wk12      4
##  7 wk16      3
##  8 wk17      3
##  9 wk8       3
## 10 wk10      2
## 11 wk18      2
## 12 wk19      2
```

4.  In which format should `billboard` be if we wanted to count how many weeks a song stayed on the Billboard top 100? Try it! Which 5 songs stayed the longest?


```r
# Use the tidy version!
pivot_longer(billboard,
             cols = c(wk1:wk76), 
             names_to = "week",
             values_to = "ranking") |>
  # Get rid of the missing values
  filter(!is.na(ranking)) |>
  # Calculate number of weeks per track
  group_by(track) |>
  summarize(n = n()) |>
  slice_max(n, n = 5)
```

```
## # A tibble: 5 × 2
##   track                   n
##   <chr>               <int>
## 1 Higher                 57
## 2 Amazed                 55
## 3 Breathe                53
## 4 Kryptonite             53
## 5 With Arms Wide Open    47
```

5.  In which format should `billboard` be if we wanted to count how many songs per artist entered the Billboard top 100 in 2000? Try it! Which artists had more than three songs on the billboard that year?


```r
# Find the number of songs per artist
billboard |>
  group_by(artist) |>
  summarize(n = n()) |>
  filter(n > 3) 
```

```
## # A tibble: 3 × 2
##   artist                n
##   <chr>             <int>
## 1 Dixie Chicks, The     4
## 2 Houston, Whitney      4
## 3 Jay-Z                 5
```

