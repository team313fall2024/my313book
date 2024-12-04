---
title: "Simulations"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---

In this worksheet, we will:

-   Discuss R's random number generating capabilities.
-   Build some common probability distributions.
-   Use loops and random number generation to simulate random events.

Because we always come back to `tidyverse` at some point:


```r
library(tidyverse)
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

## 1. Generating random numbers

Any software that can produce a "random" process typically relies on a **pseudo-random number generator.** In R, it is called the *Mersenne-Twister*, which is considered very good, repeating only once every $2^{19,937} - 1$ iterations.

### a. Random numbers among some values

We can use this generator to sample from a given list of values:


```r
# Define a list of values
x <- 1:10

# Take 1 sample value
sample(x, size = 1)
```

```
## [1] 5
```

Did we all get the same thing? Why/Why not?

**It's random!**

We can also get a sample of more than 1 value:


```r
# What's the difference between these two pieces of code?
sample(x, size = 5, replace = TRUE)
```

```
## [1] 1 8 3 5 3
```

```r
sample(x, size = 5, replace = FALSE)
```

```
## [1]  4  7 10  3  8
```

**Replace = TRUE can have duplicates.**

Note that we can make sure that we all have the same "random sample". Let's use `set.seed`:


```r
# Set a unique seed
set.seed(7)

# Then run the random process
sample(x, size = 3,replace = TRUE)
```

```
## [1] 10  3  7
```

We would have to set the seed each time we want to use a random process.

### b. Random numbers from a probability distribution

You will come across common probability distributions (uniform, normal, etc.) throughout the SDS curriculum. Here are some examples:


```r
# Uniform distribution of values between 0 and 1
runif(n = 10, min = 0, max = 1)
```

```
##  [1] 0.79201043 0.34006235 0.97206250 0.16585548 0.45910367 0.17174808
##  [7] 0.23147710 0.77281195 0.09630154 0.45344777
```

What if we sampled many values and plotted these values with a histogram:


```r
# Make a vector of one million values between 0 and 1
x_unif <- runif(n = 1000000, min = 0, max = 1)
hist(x_unif, main = 'Uniform (0,1) Distribution')
```

<img src="15_Simulation_files/figure-html/unnamed-chunk-6-1.png" width="672" />

Why does it make sense to see what we see?

**All values equally likely.**

Another common distribution is the normal distribution that is defined by a mean value and a standard deviation with a symmetric shape (bell-shaped).


```r
# Normal distribution of mean 0 and standard deviation 1
rnorm(n = 10, mean = 0, sd = 1)
```

```
##  [1]  1.2472739 -1.2330278 -0.8378650  0.8116156  0.4344979  0.9063994
##  [7]  0.6062468  0.4520080  0.3909159 -0.3648308
```

Representing many values:


```r
# Make a vector of one million values between 0 and 1
x_norm <- rnorm(n = 1000000, mean = 0, sd = 1)
hist(x_norm, main = 'Normal (0,1) Distribution')
```

<img src="15_Simulation_files/figure-html/unnamed-chunk-8-1.png" width="672" />

You can find all of the probability distributions R can sample from [here](https://www.statmethods.net/advgraphs/probability.html).

------------------------------------------------------------------------

### Group practice

Let's consider the following dataset:


```r
# Upload data from GitHub
pokemon <- read_csv("https://raw.githubusercontent.com/laylaguyot/datasets/main//pokemon.csv")
```

```
## Rows: 800 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (3): Name, Type1, Type2
## dbl (9): Number, Total, HP, Attack, Defense, SpAtk, SpDef, Speed, Generation
## lgl (1): Legendary
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
# Take a look 
head(pokemon)
```

```
## # A tibble: 6 × 13
##   Number Name           Type1 Type2 Total    HP Attack Defense SpAtk SpDef Speed
##    <dbl> <chr>          <chr> <chr> <dbl> <dbl>  <dbl>   <dbl> <dbl> <dbl> <dbl>
## 1      1 Bulbasaur      Grass Pois…   318    45     49      49    65    65    45
## 2      2 Ivysaur        Grass Pois…   405    60     62      63    80    80    60
## 3      3 Venusaur       Grass Pois…   525    80     82      83   100   100    80
## 4      3 VenusaurMega … Grass Pois…   625    80    100     123   122   120    80
## 5      4 Charmander     Fire  <NA>    309    39     52      43    60    50    65
## 6      5 Charmeleon     Fire  <NA>    405    58     64      58    80    65    80
## # ℹ 2 more variables: Generation <dbl>, Legendary <lgl>
```

In your group, you will:

1.  Pick one numeric variable (`Total`, `HP`, ..., or `Speed`). Take a look at the distribution of this variable with a histogram and find the value of the mean and standard deviation.


```r
# Write code here
```

2.  Take a sample of 30 values of this variable and find the mean. Do all group members get the same mean? How does this mean compare with the overall mean from above?


```r
# Write code here
```

3.  Now, repeat the sampling process 100 times: each time, take a sample of 30 values and find the mean. You should get 100 (different) mean values overall! Represent the distribution of these means with an histogram and find then find the mean (of the means!). How does this mean compare with the overall mean from way above?


```r
# Write code here
```

4.  Share both histograms (of the entire variable and of the means) along with the values of the means on your [group's slide] (<https://docs.google.com/presentation/d/1v_0h8w1YspWKaKt5K1CYiewiokwMyjETLVJBTm0AUEw/edit?usp=sharing>)

You just demonstrated an important theorem in probability, called the Central Limit Theorem!

------------------------------------------------------------------------

## 2. Some applications of simulations

We can use simulations to help us investigate probabilities.

### a. Rolling dice

Let's start with an intuitive example, like rolling dice:


```r
set.seed(313)
# This is a roll
sample(x = 1:6, size = 1)
```

```
## [1] 4
```

What was the probability of rolling a 4?

**1/6**

What if we roll the dice 10 times:


```r
# Roll the dice and check how many times it happened
myrolls <- sample(x = 1:6, size = 10, replace = TRUE)
barplot(table(myrolls))
```

<img src="15_Simulation_files/figure-html/unnamed-chunk-14-1.png" width="672" />

What appears to be the probability of rolling a 4 given these results?

**2/10**

What if we roll the dice 5,000 times:


```r
# Roll the dice and check how many times it happened
myrolls <- sample(x = 1:6, size = 5000, replace = TRUE)
barplot(table(myrolls))
```

<img src="15_Simulation_files/figure-html/unnamed-chunk-15-1.png" width="672" />

```r
table(myrolls)
```

```
## myrolls
##   1   2   3   4   5   6 
## 826 809 852 843 831 839
```

```r
843/5000
```

```
## [1] 0.1686
```

What appears to be the probability of rolling a 4 given these results?

**800/5000**

Let's keep track of the probability of rolling a 4 as we roll the dice many times (using a loop!):


```r
# Initialize values for rolls and probabilities
myrolls <- c()
myprob <- c()

for (i in 1:5000) {
  myrolls[i] <- sample(x = 1:6, size = 1) # roll 1 die
  myprob[i] <- sum(myrolls == 4)/i # divide how many times we rolled a 4 by how many times we rolled
}

plot(1:5000, myprob, type = 'l', xlab = "Number of rolls")
abline(h = 1/6, lty = 2, col = 'red')
```

<img src="15_Simulation_files/figure-html/unnamed-chunk-16-1.png" width="672" />

What do you notice about the probability of rolling a 4 as the number of rolls increases?

**Write sentences here.**

You just demonstrated an important theorem in probability, called the Law of Large Numbers! It is especially useful when the probability itself is not easy to determine but we could repeat an experiment a large number of times.

### b. Birthday problem

*What is the probability that at least 2 people in the room today share the same birthday?*

This question does not have an easy answer. But let's use a simulation to explore:


```r
# Create a vector representing all possible birthdays
birthdays = 1:365

# Pick 40 birthdays (uniformly = with an equal chance and with replacement), at random
mybirthdays <- sample(birthdays, size = 40, replace = TRUE)

# Are there any doubles? 
any(table(mybirthdays) > 1) # If TRUE, then at least two students have the same birthday
```

```
## [1] TRUE
```

Do we just get that result by chance?

------------------------------------------------------------------------

#### **Try it! Repeat the process above 5,000 times and find out how many times 2 out of 40 students did share a birthday.**


```r
# Initiate
shared_birthday <- c()

for(i in 1:5000){
  mybirthdays <- sample(birthdays, size = 42, replace = TRUE)
  # Are there any doubles? 
  shared_birthday[i] <- any(table(mybirthdays) > 1)
}

table(shared_birthday)
```

```
## shared_birthday
## FALSE  TRUE 
##   444  4556
```

```r
barplot(table(shared_birthday))
```

<img src="15_Simulation_files/figure-html/unnamed-chunk-18-1.png" width="672" />

```r
mean(shared_birthday)
```

```
## [1] 0.9112
```

------------------------------------------------------------------------
