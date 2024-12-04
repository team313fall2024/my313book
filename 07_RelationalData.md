---
title: "Relational Data"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---



In this worksheet, we will:

-   Identify key variables and how they are used to join datasets
-   Perform different types of joins to combine datasets in various ways
-   Address common issues in joining datasets

## 0. Datasets and Library

We will discuss joining functions to combine datasets from the `tidyverse` package:


```r
# Load a package
library(tidyverse)
```

Consider the following built-in datasets containing information about some band members of the Beatles and Rolling Stones:


```r
# Preview datasets
band_members
```

```
## # A tibble: 3 × 2
##   name  band   
##   <chr> <chr>  
## 1 Mick  Stones 
## 2 John  Beatles
## 3 Paul  Beatles
```

```r
band_instruments
```

```
## # A tibble: 3 × 2
##   name  plays 
##   <chr> <chr> 
## 1 John  guitar
## 2 Paul  bass  
## 3 Keith guitar
```

To join datasets, we first need to identify a **key variable** (a variable, or sometimes a set of variables, that defines a unique row in a dataset). What is the key variable to join the two datasets above?

**Write sentences here.**

## 1. Different types of joining functions

### a. Inner join

Join datasets using `inner_join()` to get the information they have in common:


```r
# Join 2 datasets with `inner_join()`
inner_join(band_members, band_instruments, by = "name")
```

```
## # A tibble: 2 × 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 John  Beatles guitar
## 2 Paul  Beatles bass
```

Why we only get 2 rows?

**Write sentences here.**

### b. Left join

Join datasets using `left_join()` to keep information from the "left" dataset and add information from the "right" dataset:


```r
# Join 2 datasets with `left_join()` 
left_join(band_members, band_instruments, by = "name")
```

```
## # A tibble: 3 × 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 Mick  Stones  <NA>  
## 2 John  Beatles guitar
## 3 Paul  Beatles bass
```

There was one row in the left dataset that did not appear in the right dataset. How did R handle that?

**Write sentences here.**

#### **Try it! Swap the left and right datasets from above. How do the resulting joined dataset compare?**


```r
# Write and submit code here!
```

**Write sentences here.**

### c. Right join

This function does the opposite of `left_join()` so it is not widely used.


```r
# Join 2 datasets with `right_join()`
right_join(band_members, band_instruments, by = "name")
```

```
## # A tibble: 3 × 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 John  Beatles guitar
## 2 Paul  Beatles bass  
## 3 Keith <NA>    guitar
```

Which `left_join()` function above gave a similar result?

**Write sentences here.**

### d. Full join

Join datasets using `full_join()` to keep information from both datasets:


```r
# Join 2 datasets with `full_join()`
full_join(band_members, band_instruments, by = "name")
```

```
## # A tibble: 4 × 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 Mick  Stones  <NA>  
## 2 John  Beatles guitar
## 3 Paul  Beatles bass  
## 4 Keith <NA>    guitar
```

Note how R added missing values for the names that were in only one of these two datasets.

### e. Anti join

We can use `anti_join()` to get information from the left dataset for which there is no information in the right dataset:


```r
# Find missing observations with `anti_join()`
anti_join(band_members, band_instruments, by = "name")
```

```
## # A tibble: 1 × 2
##   name  band  
##   <chr> <chr> 
## 1 Mick  Stones
```

Mick did not have an instrument in `band_instruments`.

## 2. Common issues with joining

There are some options and common issues to consider when joining different datasets.

### a. No matching key

Some datasets may refer to the same variable with different names. Consider the following datasets:


```r
band_members
```

```
## # A tibble: 3 × 2
##   name  band   
##   <chr> <chr>  
## 1 Mick  Stones 
## 2 John  Beatles
## 3 Paul  Beatles
```

```r
band_instruments2
```

```
## # A tibble: 3 × 2
##   artist plays 
##   <chr>  <chr> 
## 1 John   guitar
## 2 Paul   bass  
## 3 Keith  guitar
```

What happens if we are joining 2 datasets that have different names for the key variable?


```r
# Join the two datasets with different key variables
left_join(band_members, band_instruments2, by = "name")
```

We need to specify the name of the key in each dataset:


```r
# Join the two datasets with different key variables
left_join(band_members, band_instruments2,
          # and specify which variables match across datasets with `c()`
          by = c("name" = "artist"))
```

```
## # A tibble: 3 × 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 Mick  Stones  <NA>  
## 2 John  Beatles guitar
## 3 Paul  Beatles bass
```

*Note: The order in which we specify the match for the key variable matters: specify the match from the left dataset to the right dataset. Also, note that only the first name of the key variable is kept in the joined dataset.*

### b. Auto-suffixing

Here is another dataset reporting fun facts about instruments:


```r
# Consider this new dataset
band_instruments3 <- data.frame(
  name = c("John","Paul","Keith"),
  plays = c("a Steinway upright piano his biggest solo hit Imagine ", 
            "54 different instruments", 
            "10 out of his 3,000 guitars regularly"))
band_instruments3
```

```
##    name                                                  plays
## 1  John a Steinway upright piano his biggest solo hit Imagine 
## 2  Paul                               54 different instruments
## 3 Keith                  10 out of his 3,000 guitars regularly
```

What happens if we are joining 2 datasets with the same variable name that is *not a key* variable?


```r
# Join the two variables of instruments played
left_join(band_instruments, band_instruments3, by = "name")
```

```
## # A tibble: 3 × 3
##   name  plays.x plays.y                                                 
##   <chr> <chr>   <chr>                                                   
## 1 John  guitar  "a Steinway upright piano his biggest solo hit Imagine "
## 2 Paul  bass    "54 different instruments"                              
## 3 Keith guitar  "10 out of his 3,000 guitars regularly"
```

Any columns that have the same name in both datasets but are not used to join on will be given suffixes `.x` and `.y` to specify which original dataset they came from (left and right, respectively). You can modify the default suffix:


```r
# Join the two variables of instruments played
left_join(band_instruments, band_instruments3, by = "name",
          # To give names to the suffix, use `suffix =`
          suffix = c(".instrument",".fun_fact"))
```

```
## # A tibble: 3 × 3
##   name  plays.instrument plays.fun_fact                                         
##   <chr> <chr>            <chr>                                                  
## 1 John  guitar           "a Steinway upright piano his biggest solo hit Imagine…
## 2 Paul  bass             "54 different instruments"                             
## 3 Keith guitar           "10 out of his 3,000 guitars regularly"
```

*Note: If the same variable appears in both dataset with the same meaning, it might be a key variable! See section d. below.*

### c. Duplicates

Here is another dataset reporting a member of a new band:


```r
# Consider this new dataset
band_members2 <- data.frame(
  name = c("Mick","John","Paul","John"),
  band = c("Stones", "Beatles", "Beatles", "Bluesbreakers"))
band_members2
```

```
##   name          band
## 1 Mick        Stones
## 2 John       Beatles
## 3 Paul       Beatles
## 4 John Bluesbreakers
```

#### **Try it! Join all the information from `band_members2` to the instruments they play. Is the information contained in the resulting dataset correct?**


```r
# Write and submit code here!
```

**Write sentences here.**

*Note that it is sometimes useful to add repeating information for some rows that share the same key. We just need to be careful that it makes sense!*

### d. Several key variables

Sometimes one variable is not enough to identify a unique match. Consider this new dataset:


```r
# Add a variable to instruments
band_instruments4 <- band_instruments |> 
  mutate(band = c("Beatles", "Beatles", "Stones"))
band_instruments4
```

```
## # A tibble: 3 × 3
##   name  plays  band   
##   <chr> <chr>  <chr>  
## 1 John  guitar Beatles
## 2 Paul  bass   Beatles
## 3 Keith guitar Stones
```

What key variable(s) should be taken into account to identify a unique row when matching this data with `band_members2`?


```r
# Join 2 datasets with 2 key variables
left_join(band_members2, band_instruments4, 
          # List the key variables with `c()`
          by = c("name", "band"))
```

```
##   name          band  plays
## 1 Mick        Stones   <NA>
## 2 John       Beatles guitar
## 3 Paul       Beatles   bass
## 4 John Bluesbreakers   <NA>
```

------------------------------------------------------------------------

## Group Practice

You will join information from the `flights` and `weather` datasets from the *nycflights* package:


```r
library(nycflights13)
flights <- flights
weather <- weather
```

We can see how these tables relate to each other: ![](https://d33wubrfki0l68.cloudfront.net/245292d1ea724f6c3fd8a92063dcd7bfb9758d02/5751b/diagrams/relational-nycflights.png)

Within your group, you will explore if the weather impacts whether or not a flight was delayed. To do so:

1.  In `flights`, create a variable called `delay_60minutes` that takes values `TRUE` if the departure delay was greater than or equal to 60 minutes and `FALSE` if the delay was less than 60 minutes (but still greater than 0).
2.  Join the information from the `weather`, if available.
3.  Choose one of the variables from the weather dataset and check if there is difference between the flights that were delayed more than 60 minutes or not with a plot and summary statistics.

Share your plot, statistics, and conclusion [here](https://docs.google.com/presentation/d/1-0vXn7aHlU9SZARjkIfGrCRqxk48VTVMU_8nqsbYtjI/edit?usp=sharing).

4.  Were there any flights that did not have any weather information?

------------------------------------------------------------------------
