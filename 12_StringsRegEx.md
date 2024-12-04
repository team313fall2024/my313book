---
title: "Strings and Regular expressions"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---

In this worksheet, we will:

-   Manipulate and analyze string data using functions from the `stringr` package
-   Apply regular expressions (RegEx) to identify patterns within strings

## 0. Set up

Let's load `tidyverse` which contains the `stringr` package:


```r
# Load package 
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

We will refer to some string objects and also manipulate strings within a dataframe containing information about the MetroBike trips taken on Halloween night 2023 (Oct 31, 6pm until Nov 1, 6am):


```r
# Get the dataset from the data portal directly
metrobike <- read_csv("https://data.austintexas.gov/resource/tyfh-5r8s.csv?$query=SELECT%0A%20%20%60trip_id%60%2C%0A%20%20%60membership_type%60%2C%0A%20%20%60bicycle_id%60%2C%0A%20%20%60bike_type%60%2C%0A%20%20%60checkout_datetime%60%2C%0A%20%20%60checkout_date%60%2C%0A%20%20%60checkout_time%60%2C%0A%20%20%60checkout_kiosk_id%60%2C%0A%20%20%60checkout_kiosk%60%2C%0A%20%20%60return_kiosk_id%60%2C%0A%20%20%60return_kiosk%60%2C%0A%20%20%60trip_duration_minutes%60%2C%0A%20%20%60month%60%2C%0A%20%20%60year%60%0AWHERE%0A%20%20%60checkout_datetime%60%0A%20%20%20%20BETWEEN%20%222023-10-31T18%3A00%3A00%22%20%3A%3A%20floating_timestamp%0A%20%20%20%20AND%20%222023-11-01T06%3A00%3A00%22%20%3A%3A%20floating_timestamp")
```

```
## Rows: 357 Columns: 14
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (5): membership_type, bicycle_id, bike_type, checkout_kiosk, return_kiosk
## dbl  (6): trip_id, checkout_kiosk_id, return_kiosk_id, trip_duration_minutes...
## dttm (2): checkout_datetime, checkout_date
## time (1): checkout_time
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Take a look at the dataset. Which variables are what we call strings?


```r
head(metrobike)
```

```
## # A tibble: 6 × 14
##    trip_id membership_type    bicycle_id bike_type checkout_datetime  
##      <dbl> <chr>              <chr>      <chr>     <dttm>             
## 1 31737505 Student Membership 21639      electric  2023-10-31 18:00:46
## 2 31737521 Student Membership 19221      electric  2023-10-31 18:01:43
## 3 31737530 Student Membership 19258      electric  2023-10-31 18:02:47
## 4 31737533 Local31            21725      electric  2023-10-31 18:02:55
## 5 31737534 Student Membership 19583      electric  2023-10-31 18:02:57
## 6 31737551 Student Membership 21639      electric  2023-10-31 18:04:34
## # ℹ 9 more variables: checkout_date <dttm>, checkout_time <time>,
## #   checkout_kiosk_id <dbl>, checkout_kiosk <chr>, return_kiosk_id <dbl>,
## #   return_kiosk <chr>, trip_duration_minutes <dbl>, month <dbl>, year <dbl>
```

**Examples: membership, kiosk.**

Let's manipulate strings with functions from the `stringr` package: the name of these functions start with `str_`.

## 1. Strings

Strings are defined with either single quotes `'` or double quotes `"`:

### a. Calculating length

The `str_length()` function can help us find the length of a string:


```r
# String length
str_length("abc")
```

```
## [1] 3
```

```r
# How is that different?
str_length("a b c")
```

```
## [1] 5
```

We can apply this function to many strings contained in a vector!


```r
# String length of a vector
str_length(metrobike$membership_type)
```

```
##   [1] 18 18 18  7 18 18  7  8 18  8  7 18  7 15 18 15  7 18 18 15 18 18 18 18 18
##  [26] 18 18 18 18 18  7 18 18  7 18 18 18 18 18 18 18 18 18 18  8  8 18 18  7 18
##  [51] 18  8  8 15 18 18  7  8 18 18 18  7 18 15 18 18  8 18 18 18 18 18 18 18 18
##  [76] 18 18 18  8 18 18  8 18 18 18 18 18 18 18 18 18 18 18 18 18 18 18  8 18 18
## [101] 18 18 18 18 18  7 18  7 18 18 18 15 18  8 18 18 18  7  7 18 18  7 18 18 18
## [126] 15 18 18  8 18  8 18 18 18 18 18 18 18  8 18  8 18 18 18  8 18 18 18 18 18
## [151] 18 18 18 18 18  8 18 18 18 18 18 18 18 18  8 18 18 18 18  8 18 18 18  7 18
## [176] 18 18 18 18 18 18 18 18 18  7 18 18 18 18 18  7 18  7 18 18 18 18 18  7 18
## [201] 18 18 15 18  8 18 18 18 18 18 18 18 18 18 18 18 18 18 18 18 18 18  8 18 18
## [226] 18 18 18 18 18 18 18 18 18 18 18  8 18  8 18 18 18 18 18  8 18 18 18  7 18
## [251] 18 18 18  7 18 18 18 15 18 18 18 18 18 18 18  7  7 18 18  8  8 18 18 18 18
## [276] 18 18 18 18 18 18 18 18 18 18 18 18 18  8  8 18 18 18 18  7 18 18 18 18 18
## [301] 18 18  7 18 18 18 18 18 18 18 18 18  8 18  7 18 18  7 18 18 18  7  7 18 18
## [326] 18  7 18 18  8  8 18 18 18 18 18 18 18 18 18 18 18  8  7 18 18  7  8  7 18
## [351] 18 18 18 18  7 18  8
```

We can also apply this function to create a new variable in a dataframe:


```r
metrobike |>
  # for distinct values
  distinct(membership_type) |>
  # Find the length and create a new variable
  mutate(length_value = str_length(membership_type))
```

```
## # A tibble: 5 × 2
##   membership_type    length_value
##   <chr>                     <int>
## 1 Student Membership           18
## 2 Local31                       7
## 3 Local365                      8
## 4 Pay-as-you-ride              15
## 5 Explorer                      8
```

### b. Combining strings

We can use `str_c()` to combine two or more strings:


```r
# Combine strings
str_c("Happy", "Halloween", "!")
```

```
## [1] "HappyHalloween!"
```

```r
# By default, no space but we can add the argument sep = 
str_c("Happy", "Halloween", "!", sep = " ")
```

```
## [1] "Happy Halloween !"
```

------------------------------------------------------------------------

#### **Try it! Combine `checkout_kiosk` with `return_kiosk`: this information represents the route of a trip! What were the top 5 routes on that Halloween night?**


```r
# Write and submit code here!
metrobike |>
  mutate(route = str_c(checkout_kiosk, return_kiosk, sep = " ")) |>
  select(route) |>
  count(route) |>
  slice_max(n, n = 5)
```

```
## # A tibble: 5 × 2
##   route                                                          n
##   <chr>                                                      <int>
## 1 21st/Speedway @ PCL Dean Keeton/Whitis                        24
## 2 Dean Keeton/Whitis 21st/Speedway @ PCL                        11
## 3 21st/Speedway @ PCL 21st/Guadalupe                             8
## 4 28th/Rio Grande 21st/Speedway @ PCL                            8
## 5 Guadalupe/West Mall @ University Co-op 21st/Speedway @ PCL     8
```

**Write sentences here.**

------------------------------------------------------------------------

What if we want to combine all the values of one vector/variable together in one object?


```r
# Use the argument collapse =
str_c(c("a","b","c"), collapse = "")
```

```
## [1] "abc"
```

```r
# Or separate by a comma and a space
str_c(c("a","b","c"), collapse = ", ")
```

```
## [1] "a, b, c"
```

We can get all distinct memberships in one object!


```r
metrobike |>
  # for distinct values
  distinct(membership_type) |> 
  # Pull the city as a vector
  pull() |> 
  # Collapse all cities together, separated by a comma and a space
  str_c(collapse = ", ")
```

```
## [1] "Student Membership, Local31, Local365, Pay-as-you-ride, Explorer"
```

### c. Changing cases

We can change the strings from lower to uppercase and vice-versa (also use sentence case):


```r
# To lower case
str_to_lower("Happy Halloween!")
```

```
## [1] "happy halloween!"
```

```r
# To upper case
str_to_upper("Happy Halloween!")
```

```
## [1] "HAPPY HALLOWEEN!"
```

```r
# To sentence case
str_to_sentence("Happy Halloween!")
```

```
## [1] "Happy halloween!"
```

Especially useful if there are some inconsistencies in the categories of a variable!

### d. Subsetting strings

We can focus on a subset of a string with `str_sub()` (only works with indexing positions though):


```r
# Select a position in the string
str_sub("Happy Halloween!", start = 1, end = 5)
```

```
## [1] "Happy"
```

```r
# Or count backwards with -
str_sub("Happy Halloween!", start = -10, end = -2)
```

```
## [1] "Halloween"
```

We can also split a string by finding a separator:


```r
# Split given a pattern
str_split("Happy Halloween!", pattern = " ")
```

```
## [[1]]
## [1] "Happy"      "Halloween!"
```

Note that the resulting object is called a list and is difficult to manipulate within dataframes.

### e. Finding (exact) matches in strings

Let's start finding patterns in strings! We can find if a pattern occurs in our data with `str_detect()`:


```r
# Detect the matches
str_detect("Halloween", pattern = "Ha")
```

```
## [1] TRUE
```

------------------------------------------------------------------------

#### **Try it! Find how many trips in `metrobike` were done with a local membership.**


```r
# Write and submit code here!
metrobike |>
  filter(str_detect(membership_type, "Local")) |>
  count()
```

```
## # A tibble: 1 × 1
##       n
##   <int>
## 1    65
```

**Write sentences here.**

------------------------------------------------------------------------

What if we want to replace a pattern with `str_replace()`:


```r
# Replace the matches
str_replace("Happy Halloween", pattern = "Happy", replacement = "Spooky")
```

```
## [1] "Spooky Halloween"
```

## 2. Regular expressions (Regex)

Regular expressions are used to describe patterns in strings. They're a little weird at first but they can be very useful, especially when we are looking for patterns with some flexibility.

### a. Wildcards

Use `.` to match any character (except a new line):


```r
str_detect(c("Street", "street"), pattern = ".treet")
```

```
## [1] TRUE TRUE
```

### b. Anchors

Let's find a match at the beginning of a string with `^` or at the end of a string with `$` :


```r
metrobike |>
  # for distinct values
  distinct(membership_type) |>
  # Filter membership starting with Local
  filter(str_detect(membership_type, "^Local"))
```

```
## # A tibble: 2 × 1
##   membership_type
##   <chr>          
## 1 Local31        
## 2 Local365
```


```r
metrobike |>
  # for distinct values
  distinct(checkout_kiosk) |>
  # Filter membership starting with Local
  filter(str_detect(checkout_kiosk, "st$"))
```

```
## # A tibble: 3 × 1
##   checkout_kiosk
##   <chr>         
## 1 Boardwalk West
## 2 3rd/West      
## 3 6th/West
```

### c. Flexible patterns

To look for certain patterns, we will use `[]`. Here are a few useful patterns:

-   `[0-9]` matches any digit

-   `[ ]` matches any single space

-   `[abc]` matches a, b, or c

-   `[a-zA-Z]` matches any letter, lower case or upper case

-   `[a-zA-Z0-9]` matches any alphanumeric character

Let's find any checkout kiosk with a vowel:


```r
metrobike |>
  # for distinct values
  distinct(checkout_kiosk) |>
  # Filter cities starting with A or B
  filter(str_detect(checkout_kiosk, "[aeiou]$"))
```

```
## # A tibble: 14 × 1
##    checkout_kiosk                                        
##    <chr>                                                 
##  1 28th/Rio Grande                                       
##  2 Plaza Saltillo                                        
##  3 22.5/Rio Grande                                       
##  4 Electric Drive/Sandra Muraida Way @ Pfluger Ped Bridge
##  5 East 6th/Medina                                       
##  6 Barton Springs/Riverside                              
##  7 21st/Guadalupe                                        
##  8 8th/Lavaca                                            
##  9 5th/Bowie                                             
## 10 Veterans/Atlanta @ MoPac Ped Bridge                   
## 11 4th/Guadalupe @ Republic Square                       
## 12 Dean Keeton/Park Place                                
## 13 8th/San Jacinto                                       
## 14 4th/Sabine
```

------------------------------------------------------------------------

#### **Try it! Find how many checkout kiosk start with a number.**


```r
metrobike |>
  # for distinct values
  distinct(checkout_kiosk) |>
  # Filter cities starting with A or B
  filter(str_detect(checkout_kiosk, "^[0-9]"))
```

```
## # A tibble: 25 × 1
##    checkout_kiosk                
##    <chr>                         
##  1 23rd/San Jacinto @ DKR Stadium
##  2 21st/Speedway @ PCL           
##  3 6th/Congress                  
##  4 28th/Rio Grande               
##  5 22.5/Rio Grande               
##  6 26th/Nueces                   
##  7 3rd/West                      
##  8 21st/Guadalupe                
##  9 8th/Lavaca                    
## 10 23rd/Pearl                    
## # ℹ 15 more rows
```

**Write sentences here.**

------------------------------------------------------------------------

### d. Special characters

In regular expressions, some characters have special meanings (e.g., `.` matches any character, `^` indicates the start of a string, etc.). Sometimes, we may want to search for these special characters themselves rather than their functionality.

To do this, we can "escape" them using a backslash (`\`).


```r
# Actually referring to a quote for a string
'\''
```

```
## [1] "'"
```

The trick is that `\` is a special character itself so we sometimes have to use a few of those `\\`:


```r
# Compare these two pieces of code:
str_replace_all("Happy Halloween.", pattern = ".", replacement = "!")
```

```
## [1] "!!!!!!!!!!!!!!!!"
```

```r
str_replace_all("Happy Halloween.", pattern = "\\.", replacement = "!")
```

```
## [1] "Happy Halloween!"
```

------------------------------------------------------------------------

## Group practice

In a group of 3-4 students, investigate the following stories hidden in the `metrobike` dataset for Halloween night in 2023.

1.  The Haunted Hour! A rumor says the busiest checkout hour on Halloween night is exactly at the spookiest time: midnight (00:00). Is this true?


```r
# Write and submit code here!
```

2.  The Phantom’s Route! Some ghostly cyclists seem to haunt kiosks, returning the bike to the same kiosk. Can you find which kiosks are haunted?


```r
# Write and submit code here!
```

3.  Follow the Spooky Places! Check the article <https://do512.com/p/where-to-get-scared-in-austin> and retrieve the names of some places to get scared in Austin. Are there any kiosks near one of these places? Hint: one of these hotels is haunted!


```r
# Write and submit code here!
```

4.  The Longest Ride! Legend has it that one cyclist took an incredibly long trip across town. Can you find at what time was the bicycle that had the longest ride checked out and specify the route?


```r
# Write and submit code here!
```

5.  The Curse of the 13th Ride! A chilling tale says that one bike was cursed after completing exactly 13 trips on Halloween night. Riders mysteriously vanished after using this bike. Can you uncover if the curse is real?


```r
# Write and submit code here!
```

6.  The Haunted Path of 6th Street! 6th Street, known for its lively (and perhaps otherworldly) nightlife, may have seen eerie bike trips on Halloween. How many spirits rode the bikes down this ghostly path?


```r
# Write and submit code here!
```

7.  Unravel your own spooky story!


```r
# Write and submit code here!
```

Share your favorite story on your [group's slide] (<https://docs.google.com/presentation/d/1Jknm_4urUcZmychWMq4u0dd0kpaytJGqqLF8OuqUtBI/edit?usp=sharing>)
