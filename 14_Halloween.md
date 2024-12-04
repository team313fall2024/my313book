---
title: "Halloween candy"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---

In this worksheet, we will discuss how to predict the values of a variable (outcome/response/dependent variable) based on some other variable(s) (predictor/explanatory/independent variable).

## 1. Set up

We will use the `tidyverse` package as always:


```r
# Load packages
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

We will work with the data from the following article:

Hickey, W. (2007). The Ultimate Halloween Candy Power Ranking. FiveThirtyEight. <https://fivethirtyeight.com/videos/the-ultimate-halloween-candy-power-ranking/>


```r
# Upload data from GitHub
candy <- read_csv("https://raw.githubusercontent.com/laylaguyot/datasets/main//Halloween-candy.csv")
```

```
## Rows: 85 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (1): competitorname
## dbl (12): chocolate, fruity, caramel, peanutyalmondy, nougat, crispedricewaf...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
# Take a quick look
head(candy)
```

```
## # A tibble: 6 × 13
##   competitorname chocolate fruity caramel peanutyalmondy nougat crispedricewafer
##   <chr>              <dbl>  <dbl>   <dbl>          <dbl>  <dbl>            <dbl>
## 1 100 Grand              1      0       1              0      0                1
## 2 3 Musketeers           1      0       0              0      1                0
## 3 One dime               0      0       0              0      0                0
## 4 One quarter            0      0       0              0      0                0
## 5 Air Heads              0      1       0              0      0                0
## 6 Almond Joy             1      0       0              1      0                0
## # ℹ 6 more variables: hard <dbl>, bar <dbl>, pluribus <dbl>,
## #   sugarpercent <dbl>, pricepercent <dbl>, winpercent <dbl>
```

This dataset is the result of an experiment: "Pit dozens of fun-sized candy varietals against one another, and let the wisdom of the crowd decide which one was best. While we don’t know who exactly voted, we do know this: 8,371 different IP addresses voted on about 269,000 randomly generated matchups."

Here are the top 19 winners:

![](https://pbs.twimg.com/media/FA6KdxlXsAAo7VI.jpg)

We are interested on determining what features of the candy might affect its win percentage. In that case, what is the outcome? What do you think could be a good predictor?

**Outcome is win percentage, predictors are sugar/chocolate/price...**

------------------------------------------------------------------------

#### **Try it! There is one variable that would not be helpful as a predictor. Which one?**


```r
# Write and submit code here!
head(candy)
```

```
## # A tibble: 6 × 13
##   competitorname chocolate fruity caramel peanutyalmondy nougat crispedricewafer
##   <chr>              <dbl>  <dbl>   <dbl>          <dbl>  <dbl>            <dbl>
## 1 100 Grand              1      0       1              0      0                1
## 2 3 Musketeers           1      0       0              0      1                0
## 3 One dime               0      0       0              0      0                0
## 4 One quarter            0      0       0              0      0                0
## 5 Air Heads              0      1       0              0      0                0
## 6 Almond Joy             1      0       0              1      0                0
## # ℹ 6 more variables: hard <dbl>, bar <dbl>, pluribus <dbl>,
## #   sugarpercent <dbl>, pricepercent <dbl>, winpercent <dbl>
```

**Not competitor name: it's too specific.**

------------------------------------------------------------------------

#### **Try it! There are two ghosts in this dataset: two observations that are not actually candies! Remove them from the dataset.**


```r
candy <- candy |>
  filter(sugarpercent != 0.011)

candy <- candy |>
  filter(!str_detect(competitorname, pattern = "^One"))
```

**Write sentences here!**

------------------------------------------------------------------------

## 2. Exploring relationships

We can visually inspect if there is a relationship between a potential predictor and the outcome.

------------------------------------------------------------------------

## Group Practice

1.  Each group member picks the predictor that they think would best relate to the win percentage of a candy. Use a plot to represent the relationship between `winpercent` and the predictor with an appropriate graph. Does there appear to be a relationship to predict the win percentage?


```r
# Write code here
```

**Write sentences here!**

2.  Now represent the relationship between `winpercent` and each potential predictor with an appropriate graph. Which predictor(s) seem to have the strongest relationship with the win percentage?


```r
# Write code here
```

**Write sentences here!**

------------------------------------------------------------------------

## 3. Machine Learning

Machine learning involves using algorithms to uncover patterns in data and make predictions based on those patterns. There are different types of algorithms for machine learning that fall into 3 categories:

![](https://datasciencedojo.com/wp-content/uploads/ml-ds-algos.jpg)

We focus on the main two types: supervised vs unsupervised learning.

![](https://www.nimblework.com/wp-content/uploads/2021/02/Machine-learning-Supervised-and-Unsupervised.png)

Here are some fundamental machine learning algorithms:

-   linear and logistic regression for modeling relationships and predicting outcomes

-   k-nearest neighbors (kNN) for classifying data points by similarity

-   decision trees for making decisions based on feature splits

-   clustering for grouping similar data points

-   principal component analysis (PCA) for reducing dimensionality and visualizing complex data

By applying these algorithms to the Halloween candy dataset, we can see how different candy characteristics can influence popularity, gaining insights into how each technique approaches learning from data and making predictions.
