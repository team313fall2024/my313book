---
title: "Dates and Times"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---



In this worksheet, we will:

-   Parse dates and times from strings into R-compatible formats using `lubridate`
-   Manipulate dates and times, including extracting specific components (year, month, day, hour, etc.)
-   Calculate differences between two dates/times
-   Visualize time-based data trends and summarize data over time units

## 0. Datasets and Libraries

Let's load `tidyverse` which contains the `lubridate` package and `nycflights13` for datasets:


```r
# Load packages
library(tidyverse)
library(nycflights13)
```

Here are the main datasets we will manipulate today:


```r
# Take a look at txhousing and flights
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

```r
head(flights)
```

```
## # A tibble: 6 × 19
##    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
## 1  2013     1     1      517            515         2      830            819
## 2  2013     1     1      533            529         4      850            830
## 3  2013     1     1      542            540         2      923            850
## 4  2013     1     1      544            545        -1     1004           1022
## 5  2013     1     1      554            600        -6      812            837
## 6  2013     1     1      554            558        -4      740            728
## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
## #   hour <dbl>, minute <dbl>, time_hour <dttm>
```

In what format are dates/times reported in each dataset?

**Only time_hour is in a date/time format.**

## 1. Parsing dates and times

Parsing dates and times means converting a string or numeric representation of a date or time into a proper object that R can understand and work with.

### a. R formats

Here are two typical date/time formats in R:


```r
# Look at what date/time is today/now!
today()
```

```
## [1] "2024-12-02"
```

```r
now()
```

```
## [1] "2024-12-02 17:07:11 CST"
```


```r
# How R identifies these formats
class(today())
```

```
## [1] "Date"
```

```r
class(now())
```

```
## [1] "POSIXct" "POSIXt"
```

By default, R considers:

-   *dates* as "yyyy-mm-dd" (year-month-day)

-   *times* as "hh:mm:ss" (hours:minutes:seconds)

-   *date/times* as "yyyy-mm-dd hh:mm:ss"

Here is an example of a date entered in the year-month-day but does R recognize it as a date?


```r
# Check the format of the date entered
class("2024-10-03")
```

```
## [1] "character"
```

We can convert a character/string and convert it as a date in an R format:


```r
# year, month, day
ymd("2024-10-03")
```

```
## [1] "2024-10-03"
```

```r
# Check the format now
class(ymd("2024-10-03"))
```

```
## [1] "Date"
```

Here are some examples of other formats of dates than can be converted:


```r
# day, month, year
dmy("3.10.2024")
```

```
## [1] "2024-10-03"
```

```r
dmy("3/10/2024")
```

```
## [1] "2024-10-03"
```

```r
dmy("3-10-2024")
```

```
## [1] "2024-10-03"
```

```r
dmy("the 3rd of October 2024")
```

```
## [1] "2024-10-03"
```

```r
dmy("03-octobre-2024") # this one did not work, why?
```

```
## Warning: All formats failed to parse. No formats found.
```

```
## [1] NA
```

```r
# month, day, year
mdy("10/3/2024")
```

```
## [1] "2024-10-03"
```

```r
mdy("October 3rd, 2024")
```

```
## [1] "2024-10-03"
```

Similarly, we can convert strings into time:


```r
# date in year, month, day and time
ymd_hms("2024-10-03 03:15:00 PM")
```

```
## [1] "2024-10-03 15:15:00 UTC"
```

```r
# also check other date functions with _hms or _hm, or simply the function hm() and hms()
hm("03:15 PM")
```

```
## [1] "3H 15M 0S"
```

```r
hms("03:15:00 AM")
```

```
## [1] "3H 15M 0S"
```

### b. Combining date/time components

We can combine the different parts of a date with `make_date()` or also add time with `make_datetime()`.


```r
# Combine year and month into a date
txhousing |>
  mutate(new_date = make_date(year, month))
```

```
## # A tibble: 8,602 × 10
##    city     year month sales   volume median listings inventory  date new_date  
##    <chr>   <int> <int> <dbl>    <dbl>  <dbl>    <dbl>     <dbl> <dbl> <date>    
##  1 Abilene  2000     1    72  5380000  71400      701       6.3 2000  2000-01-01
##  2 Abilene  2000     2    98  6505000  58700      746       6.6 2000. 2000-02-01
##  3 Abilene  2000     3   130  9285000  58100      784       6.8 2000. 2000-03-01
##  4 Abilene  2000     4    98  9730000  68600      785       6.9 2000. 2000-04-01
##  5 Abilene  2000     5   141 10590000  67300      794       6.8 2000. 2000-05-01
##  6 Abilene  2000     6   156 13910000  66900      780       6.6 2000. 2000-06-01
##  7 Abilene  2000     7   152 12635000  73500      742       6.2 2000. 2000-07-01
##  8 Abilene  2000     8   131 10710000  75000      765       6.4 2001. 2000-08-01
##  9 Abilene  2000     9   104  7615000  64500      771       6.5 2001. 2000-09-01
## 10 Abilene  2000    10   101  7040000  59300      764       6.6 2001. 2000-10-01
## # ℹ 8,592 more rows
```

By default, the day on the date was set to the first day of the month.

### c. Extracting part(s) of the date

On the contrary, we might want to extract some specific date/time information from a date:


```r
# Extract year, month, day and time
year(now())
```

```
## [1] 2024
```

```r
month(now())
```

```
## [1] 12
```

```r
week(now())
```

```
## [1] 49
```

```r
day(now())
```

```
## [1] 2
```

```r
wday(now()) # what is that?
```

```
## [1] 2
```

```r
hour(now())
```

```
## [1] 17
```

```r
minute(now())
```

```
## [1] 7
```

```r
second(now())
```

```
## [1] 12.04936
```

Check the `label` and `abbr` options for `month()` and `wkday()`:


```r
# Convenient options
month(now(), label = TRUE, abbr = FALSE)
```

```
## [1] December
## 12 Levels: January < February < March < April < May < June < ... < December
```

```r
wday(now(), , label = TRUE, abbr = TRUE)
```

```
## [1] Mon
## Levels: Sun < Mon < Tue < Wed < Thu < Fri < Sat
```

------------------------------------------------------------------------

#### **Try it! In the `flights` dataset, extract the information of the weekday from the `time_hour` variable. Does each day have the same amount of flights?**


```r
# Write and submit code here!
flights |>
  mutate(day_in_week = wday(time_hour, , label = TRUE, abbr = TRUE)) |>
  ggplot() + geom_bar(aes(x = day_in_week))
```

<img src="08_DatesTimes_files/figure-html/unnamed-chunk-13-1.png" width="672" style="display: block; margin: auto;" />

**Write sentences here.**

------------------------------------------------------------------------

## 2. Manipulating dates and times

### a. Finding differences between dates and times

We can find date/time differences with `difftime()`:


```r
# How many days between now and the first day of the year?
difftime(now(), mdy_hms("1-1-2024 00:00:00 am"), units = "days")
```

```
## Time difference of 336.9633 days
```

```r
# What if we want to find the difference with another unit?
difftime(now(), mdy_hms("1-1-2024 00:00:00 am"), units = "weeks")
```

```
## Time difference of 48.13762 weeks
```

```r
difftime(now(), mdy_hms("1-1-2024 00:00:00 am"), units = "hours")
```

```
## Time difference of 8087.12 hours
```

```r
difftime(now(), mdy_hms("1-1-2024 00:00:00 am"), units = "mins")
```

```
## Time difference of 485227.2 mins
```

```r
difftime(now(), mdy_hms("1-1-2024 00:00:00 am"), units = "secs")
```

```
## Time difference of 29113633 secs
```

Note that the output reports the time difference with a unit. If we would like to find the value of the difference, we can use the function `as.numeric()`:


```r
# Report only a value
as.numeric(difftime(now(), mdy_hms("1-1-2024 00:00:00 am"), units = "days"))
```

```
## [1] 336.9633
```

### b. Summarizing date/time data

Depending on the level of detail we would like to focus on, we can aggregate the data by specific time units. For example, we can compare summaries over years, months, days of the week, or by the hour, minute, second.

------------------------------------------------------------------------

#### **Try it! We looked at the number of `flights` per day before. Compare the number of flights at another time unit. Do you notice any differences?**


```r
# Write and submit code here!
flights |>
  ggplot() + geom_bar(aes(x = minute))
```

<img src="08_DatesTimes_files/figure-html/unnamed-chunk-16-1.png" width="672" style="display: block; margin: auto;" />

**Write sentences here.**

------------------------------------------------------------------------

We can also represent the values of a variable over time:


```r
# Comparing sales over time
txhousing |>
  group_by(date) |>
  summarize(total_sales = sum(sales, na.rm = TRUE)) |>
  ggplot() + geom_line(aes(x = date, y = total_sales))
```

<img src="08_DatesTimes_files/figure-html/unnamed-chunk-17-1.png" width="672" style="display: block; margin: auto;" />

And compare if there is the same pattern over a repeating time unit (for example, months repeat every year):


```r
# Comparing monthly sales for each year
txhousing |>
  group_by(year,month) |>
  summarize(total_sales = sum(sales, na.rm = TRUE)) |>
  ggplot() + geom_line(aes(x = month, y = total_sales)) +
  facet_wrap(~year)
```

<img src="08_DatesTimes_files/figure-html/unnamed-chunk-18-1.png" width="672" style="display: block; margin: auto;" />

------------------------------------------------------------------------

#### **Try it! Compare the maximum distance for a flight per hour of the day. When do longer flights depart from NYC airports?**


```r
# Write and submit code here!
flights |>
  group_by(hour) |>
  slice_max(distance, n = 1) |>
  ggplot() + geom_line(aes(x = hour, y = distance))
```

<img src="08_DatesTimes_files/figure-html/unnamed-chunk-19-1.png" width="672" style="display: block; margin: auto;" />

**Write sentences here.**

------------------------------------------------------------------------

### c. A few remarks

Here are some common pitfalls to look out for:

-   Different date formats (e.g., MM/DD/YYYY vs. DD/MM/YYYY) can lead to incorrect parsing. Always specify the date format explicitly when converting strings to dates.

-   Take into account that not all years are 365 days (leap years), not all days are 24 hours (daylight saving time), and not all months have the same amount of days. Most `lubridate` functions are designed to take those facts into account when manipulating dates/times.

-   The time is not the same depending on where the data was collected. Convert dates/times between time zones with some `lubridate` functions such as `with_tz()`.


```r
# Time in France
now(tzone = "Europe/Paris")
```

```
## [1] "2024-12-03 00:07:15 CET"
```

```r
# Convert to our current time zone
with_tz(now(tzone = "Europe/Paris"), tzone = "America/Chicago")
```

```
## [1] "2024-12-02 17:07:15 CST"
```

------------------------------------------------------------------------

## Group Practice

Let's explore birthdays from students in this class!

1.  Enter your birthday (including year) on this [spreadsheet](https://docs.google.com/spreadsheets/d/1DDB1l_IyVz7yVPKJKt9bQd4pCV3gJnIwU41a19zRd94/edit?usp=sharing). Once everyone has entered their birthdays, download the spreadsheet as a `.csv` file and import it in `R`.

2.  Does any birthday need to be recoded? Parse the birthday into a date format that R can work with.

3.  Make an analysis of the birthdays and paste any plot or statistics you create [here](https://docs.google.com/presentation/d/1cDttg4oytMW-S_dhXDSuymQWPsqzzYo1ix9pI8sNH18/edit?usp=sharing).


```r
# Group practice
Birthdays <- read_csv("Birthdays.csv")
Birthdays |>
  mutate(birthdays = mdy(birthdays),
         month = month(birthdays),
         day = day(birthdays)) |>
  group_by(month, day) |>
  summarize(n = n()) |>
  filter(n > 1)
```

```
## # A tibble: 4 × 3
## # Groups:   month [4]
##   month   day     n
##   <dbl> <int> <int>
## 1     6    30     2
## 2     7     6     2
## 3     8     4     2
## 4    12    16     2
```
