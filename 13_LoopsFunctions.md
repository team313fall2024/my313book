---
title: "Loops and Functions"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---

In this worksheet, we will:

-   Discuss ow to write a for loop to iterate through some process
-   Define the basic structure of functions
-   Include if/else statements to control the flow of loops/functions
-   Consider When loops/functions are or aren't useful tools

Because we always come back to `tidyverse`:


```r
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

## 1. Loops

### a. Basic for-loops

Suppose you want to perform the same task over and over. You can use loops to perform iterative tasks with the following structure:


```r
for(i in sequence){
  # task
}
```

Let's consider this basic loop:


```r
# i takes the value of each element in the sequence 1,2,3,...,5
for(i in 1:5){
  print(i)
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```

Printing is nice but what if we want to save the values generated by the loop into a vector? We would need to initialize such a vector and define each element:


```r
# Initialize an empty vector
myoutput <- c()

# Remember i takes the value of each element in the sequence 1,2,3,...,10
for(i in 1:5){
  # so we can use the values of i to index the new vector
  myoutput[i] <- i # the ith element is i
}

# Take a look!
myoutput
```

```
## [1] 1 2 3 4 5
```

------------------------------------------------------------------------

#### **Try it! Find the mean of the squares for the numbers 1 through 10.**


```r
# Write and submit code here!
myoutput2 <- c()

for (i in 1:10) {
  myoutput2[i] <- i^2
}

mean(myoutput2)
```

```
## [1] 38.5
```

```r
mean((1:10)^2)
```

```
## [1] 38.5
```

------------------------------------------------------------------------

We can also define a specific vector (not necessarily numbers!) and go through each element of the vector:


```r
# Define a vector
basket <- c('Apple', 'Orange', 'Passion fruit', 'Banana')

# Apply a loop to go through each element of that vector
for(fruit in basket){ 
  print(str_length(fruit)) # fruit takes the value of each element in basket
}
```

```
## [1] 5
## [1] 6
## [1] 13
## [1] 6
```

We could also create an object to save each length in a vector BUT `str_length` can apply to vectors so we can just do:


```r
# A function already exists for doing that!
str_length(basket)
```

```
## [1]  5  6 13  6
```

For-loops could slow down the computation. We prefer *vectorization* over for-loops since it results in shorter and clearer code. A vectorized function is a function that will apply the same operation on each value of the vectors.

### b. Conditional Statements

We can execute some code under certain conditions with the *if-else* structure:


```r
if (condition) {
  #code to run when condition is TRUE
  } else {
  #code to run when condition is FALSE
}
```

Let's try a basic example:


```r
# Set value of x
x <- 5  

# Test if x is less than 5 or not
if(x < 5){
  print("This number is less than five!")
} else {
  print("This number is at least five!")
}
```

```
## [1] "This number is at least five!"
```

Now what if we want to repeat this for several values? Use a for-loop!


```r
# What if x was 5
for (i in 1:5) { 	
  if(i < 5){
  print("This number is less than five!")
    } 
  else {
  print("This number is at least five!")
    }
} 
```

```
## [1] "This number is less than five!"
## [1] "This number is less than five!"
## [1] "This number is less than five!"
## [1] "This number is less than five!"
## [1] "This number is at least five!"
```

We can add many conditions!


```r
if (condition1){
  #code to run when condition1 is TRUE
  code
  } else if (condition2) {
  #code to run when condition2 is TRUE
  code
  }
  } else if (condition3) {
  #code to run when condition3 is TRUE
  code
  }
  } else {
  #code to run when all conditions are FALSE
  code
}
```

------------------------------------------------------------------------

## Group Practice

Consider the following dataset (get more information by running `?msleep` in the console):


```r
head(msleep)
```

```
## # A tibble: 6 × 11
##   name    genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>   <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Cheetah Acin… carni Carn… lc                  12.1      NA        NA      11.9
## 2 Owl mo… Aotus omni  Prim… <NA>                17         1.8      NA       7  
## 3 Mounta… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
## 4 Greate… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
## 5 Cow     Bos   herbi Arti… domesticated         4         0.7       0.667  20  
## 6 Three-… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

We can obtain the names of the variables into a vector with `names`:


```r
names(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

Using a for-loop, an if/else statement, and base R plots, make an appropriate plot for numeric variables and another type of plot that is appropriate for categorical variables.


```r
# Write code here
for(i in names(msleep)){
  print(i)
}
```

```
## [1] "name"
## [1] "genus"
## [1] "vore"
## [1] "order"
## [1] "conservation"
## [1] "sleep_total"
## [1] "sleep_rem"
## [1] "sleep_cycle"
## [1] "awake"
## [1] "brainwt"
## [1] "bodywt"
```

```r
for(variable in msleep){
  # if numeric, histogram
  if(is.numeric(variable)){
    hist(variable)
  }
  # if categorical, bar plot
  else if (is.character(variable)) {
  #code to run when condition2 is TRUE
  barplot(table(variable))
  }
}
```

<img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-1.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-2.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-3.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-4.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-5.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-6.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-7.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-8.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-9.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-10.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-14-11.png" width="672" />

------------------------------------------------------------------------

How nice would that be if we could create these basic plots for investigating variables from any datasets?

## 2. Functions

Sometimes you will find yourself needing to perform the same operations over and over (like creating appropriate graphs to investigate variables). Functions are another way to perform repetitive tasks.

### a. R functions

All of the functions we have used in this course were written by someone and then integrated either into base R or an external package. Because R is open-source, you can check the documentation with `?function` or view the source code of any of these functions:


```r
View(fivenum)
```

Think of a few of your favorite functions and view the source code:


```r
# View some other functions:
View(table)
```

If all you see is *UseMethod( )*, it just means that the function name you specified has different versions for different classes of arguments. You can view all related functions with *methods( )*:


```r
View(mean)
methods(mean)
View(mean.default)
```

### b. Our own functions

Suppose we want to create our own function to either simplify a process we do often or to share with a collaborator.

To write our own functions, we need to:

-   Define the function **name** and what **arguments** it will take.
-   Specify the function **body** and **return**, enclosed within the function's {}.
-   Call the function which runs the function after it has been defined.


```r
# Structure of a function
function_name <- function(arguments){
  # perform operations on inputs and produce output
  body/return
}
```

Let's define a function for finding the sum of two numbers:


```r
mysum <- function(x,y){
  xplusy <- x + y
  return(xplusy)
}
```

Note that after running this function definition, it appears in the Environment. Now we can call the function:


```r
# Find some sums
mysum(2,6)
```

```
## [1] 8
```

```r
mysum(1:10,100)
```

```
##  [1] 101 102 103 104 105 106 107 108 109 110
```

------------------------------------------------------------------------

#### **Try it! Using your code from the previous group practice, create a function that makes an appriopate plot for each variable in a dataset depending on its type (numeric/categorical). Test your function with `msleep` and `mtcars`.**


```r
appropriate_plot <- function(dataset){
  for(variable in dataset){
    # if numeric, histogram
    if(is.numeric(variable)){
      hist(variable)
    }
    # if categorical, bar plot
    else if (is.character(variable)) {
    #code to run when condition2 is TRUE
    barplot(table(variable))
    }
  }
}

appropriate_plot(mtcars)
```

<img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-1.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-2.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-3.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-4.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-5.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-6.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-7.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-8.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-9.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-10.png" width="672" /><img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-21-11.png" width="672" />

------------------------------------------------------------------------

## 3. Don't reinvent the wheel

There are many built-in functions in R that can make your programming easier. Look up the documentation or search forums to see if a function already exists before you build it.

One example for applying a function to all variables in the dataset:


```r
# Check out sapply
sapply(msleep[, sapply(msleep, is.numeric)], mean, na.rm = TRUE)
```

```
## sleep_total   sleep_rem sleep_cycle       awake     brainwt      bodywt 
##  10.4337349   1.8754098   0.4395833  13.5674699   0.2815814 166.1363494
```

One example for creating univariate graphs for each variable in a dataset using a new package:


```r
# Install a new package
install.packages("DataExplorer")
```


```r
# Load the library
library(DataExplorer)
```

```
## Warning: package 'DataExplorer' was built under R version 4.3.3
```

```r
# Plot dataset structure with appropriate graphs for each variable
plot_bar(msleep)        
```

```
## 2 columns ignored with more than 50 categories.
## name: 83 categories
## genus: 77 categories
```

<img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-24-1.png" width="672" />

```r
plot_histogram(msleep)
```

<img src="13_LoopsFunctions_files/figure-html/unnamed-chunk-24-2.png" width="672" />

------------------------------------------------------------------------

## Group Practice

Consider the following dataset (get more information by running `?quakes` in the console):


```r
head(quakes)
```

```
##      lat   long depth mag stations
## 1 -20.42 181.62   562 4.8       41
## 2 -20.62 181.03   650 4.2       15
## 3 -26.00 184.10    42 5.4       43
## 4 -17.97 181.66   626 4.1       19
## 5 -20.42 181.96   649 4.0       11
## 6 -19.68 184.31   195 4.0       12
```

Suppose we want to find out what feature of an earthquake can impact its magnitude.

1.  Create a plot to compare each variable in `quakes` to the magnitude, `mag`. Which variable seems to have the strongest relationship with magnitude?


```r
# Write code here
```

2.  Define each variable in the dataset, except `mag`, as a categorical variable with low/high values: a low value refers to a value below the median for that variable, a high value refers to a value above the median for that variable. Call them `variablename_cat`.


```r
# Write code here
```

3.  Create a plot to compare each categorical variable to the magnitude. Which variable shows the largest difference in magnitude between low and high values?


```r
# Write code here
```

4.  Select the plots for what you think is 1) the best numeric variable associated with magnitude, 2) the best categorical variable showing differences in magnitude. Share them on your [group's slide] (<https://docs.google.com/presentation/d/1ZgCYrh-aSaPenIRh8I_oHvbHuWBfCWGOSB9ZbCTrG-M/edit?usp=sharing>)