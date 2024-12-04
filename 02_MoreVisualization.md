---
title: "SDS 313  More Visualizing and Describing Data"
output: html_document
---

In this worksheet, we will:

-   Create plots with many different options using `ggplot`
-   Consider different themes and scales
-   Export our graphs as a *pdf* file

A very popular graphics package that can make it easier to create nice looking graphs is [ggplot2](https://ggplot2.tidyverse.org/index.html). Let's check that we have the necessary package installed:


```r
library(ggplot2)
```

```
## Warning: package 'ggplot2' was built under R version 4.3.3
```

The code for `ggplot` functions looks a little different than base R plots.

## 0. Import Dataset

The following data was collected at Baystate Medical Center, Springfield, MA in 1986.


```r
birthwt <- read.csv('birthwt.csv')
```

| **Variable** | **Description**                                       |
|--------------|-------------------------------------------------------|
| `low`        | Indicator of birth weight less than 2.5 kg            |
| `age`        | Mother's age (years)                                  |
| `lwt`        | Mother's weight (lbs)                                 |
| `race`       | Mother's race (1 = white, 2 = black, 3 = other).      |
| `smoke`      | Smoking status during pregnancy                       |
| `ptl`        | Number of previous premature labors                   |
| `ht`         | History of hypertension                               |
| `ui`         | Presence of uterine irritability                      |
| `ftv`        | Number of physician visits during the first trimester |
| `bwt`        | Birth weight (grams)                                  |

What code can we use to get familiar with the dataset?


```r
# Get familiar with the dataset
str(birthwt)
```

```
## 'data.frame':	189 obs. of  10 variables:
##  $ low  : chr  "no" "no" "no" "no" ...
##  $ age  : int  19 33 20 21 18 21 22 17 29 26 ...
##  $ lwt  : int  182 155 105 108 107 124 118 103 123 113 ...
##  $ race : chr  "black" "other" "white" "white" ...
##  $ smoke: chr  "no" "no" "yes" "yes" ...
##  $ ptl  : chr  "no" "no" "no" "no" ...
##  $ ht   : chr  "no" "no" "no" "no" ...
##  $ ui   : chr  "yes" "no" "no" "yes" ...
##  $ ftv  : chr  "none" "two or more" "one" "two or more" ...
##  $ bwt  : int  2523 2551 2557 2594 2600 2622 2637 2637 2663 2665 ...
```

```r
summary(birthwt)
```

```
##      low                 age             lwt            race          
##  Length:189         Min.   :14.00   Min.   : 80.0   Length:189        
##  Class :character   1st Qu.:19.00   1st Qu.:110.0   Class :character  
##  Mode  :character   Median :23.00   Median :121.0   Mode  :character  
##                     Mean   :23.24   Mean   :129.8                     
##                     3rd Qu.:26.00   3rd Qu.:140.0                     
##                     Max.   :45.00   Max.   :250.0                     
##     smoke               ptl                 ht                 ui           
##  Length:189         Length:189         Length:189         Length:189        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      ftv                 bwt      
##  Length:189         Min.   : 709  
##  Class :character   1st Qu.:2414  
##  Mode  :character   Median :2977  
##                     Mean   :2945  
##                     3rd Qu.:3487  
##                     Max.   :4990
```

```r
head(birthwt)
```

```
##   low age lwt  race smoke ptl ht  ui         ftv  bwt
## 1  no  19 182 black    no  no no yes        none 2523
## 2  no  33 155 other    no  no no  no two or more 2551
## 3  no  20 105 white   yes  no no  no         one 2557
## 4  no  21 108 white   yes  no no yes two or more 2594
## 5  no  18 107 white   yes  no no yes        none 2600
## 6  no  21 124 other    no  no no  no        none 2622
```

## 1. Creating plots with `ggplot`

### a. Define a plot

The `ggplot()` function helps us build a plot. Within this function, we specify the dataframe to explore:


```r
# Create a ggplot
bw_ggplot <- ggplot(birthwt)

# How does the plot looks like?
bw_ggplot
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-4-1.png" width="672" />

It’s empty because we haven’t specified how to represent the variables! We need to represent variables by mapping them with a geometric object using `aes()`. The type of geometric object depends on the type of data.


```r
# 1 numeric variable
bw_ggplot + geom_histogram(aes(x = bwt))
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-5-1.png" width="672" />

```r
bw_ggplot + geom_boxplot(aes(x = bwt))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-5-2.png" width="672" />

```r
# 1 categorical variable
bw_ggplot + geom_bar(aes(x = smoke))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-5-3.png" width="672" />

```r
# 2 numeric variables
bw_ggplot + geom_point(aes(x = lwt, y = bwt))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-5-4.png" width="672" />

### b. More on histograms

When looking at the histogram of the birth weight, can you tell which range of values is the most common?


```r
bw_ggplot + geom_histogram(aes(x = bwt))
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-6-1.png" width="672" />

See the message: by default, the number of bins is 30 in `ggplot`. Let's adjust that:


```r
bw_ggplot + geom_histogram(aes(x = bwt), 
                 # Set bin width and center (half of the bin width)
                 binwidth = 1000, center = 500)
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-7-1.png" width="672" />

Try some different bin widths! Note how the general shape of the histogram might change depending on how we define the bins. We usually recommend to have at least 10 different bins to be able to “see” the variation in our data.


```r
bw_ggplot + geom_histogram(aes(x = bwt), 
                 # Set bin width and center (half of the bin width)
                 binwidth = 100, center = 50)
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-8-1.png" width="672" />

```r
bw_ggplot + geom_histogram(aes(x = bwt), 
                 # Set bin width and center (half of the bin width)
                 binwidth = 500, center = 250)
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-8-2.png" width="672" />

```r
bw_ggplot + geom_histogram(aes(x = bwt), 
                 # Set bin width and center (half of the bin width)
                 binwidth = 10, center = 5)
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-8-3.png" width="672" />

### c. Map to color, shape, size

We can change customize colors on our plot (outside of the aesthetics):


```r
# What does color vs fill do? 
bw_ggplot + geom_histogram(aes(x = bwt), color = "blue", fill = "orange")
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-9-1.png" width="672" />

```r
bw_ggplot + geom_boxplot(aes(x = bwt), color = "blue", fill = "orange")
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-9-2.png" width="672" />

```r
bw_ggplot + geom_bar(aes(x = smoke), color = "blue", fill = "orange")
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-9-3.png" width="672" />

```r
bw_ggplot + geom_point(aes(x = lwt, y = bwt), color = "blue", fill = "orange")
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-9-4.png" width="672" />


```r
# What does shape vs size vs alpha do? 
bw_ggplot + geom_point(aes(x = lwt, y = bwt), shape = 2)
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-10-1.png" width="672" />

```r
bw_ggplot + geom_point(aes(x = lwt, y = bwt), size = 10)
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-10-2.png" width="672" />

```r
bw_ggplot + geom_point(aes(x = lwt, y = bwt), alpha = 0.1)
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-10-3.png" width="672" />

All these options only change the general appearance of the graph but we can change the appearance of some part of the graph by mapping variables to other types of aesthetics:


```r
# Adjust color by the categories of a variable
bw_ggplot + geom_point(aes(x = lwt, y = bwt, color = smoke))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-11-1.png" width="672" />

```r
# Adjust shape by the categories of a variable
bw_ggplot + geom_point(aes(x = lwt, y = bwt, shape = low))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-11-2.png" width="672" />

```r
# Adjust size by the numeric values of a variable
bw_ggplot + geom_point(aes(x = lwt, y = bwt, size = age))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-11-3.png" width="672" />

```r
# All at once!
bw_ggplot + geom_point(aes(x = lwt, y = bwt, 
                           color = smoke, shape = low, size = age))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-11-4.png" width="672" />

Note: adding too many variables to a single plot can make it too difficult to interpret!

------------------------------------------------------------------------

## Group Practice

1.  Does smoking during pregnancy affects birth weight? Make a plot to display the relationship between these two variables.
2.  Post your graph on your group's slide [here](https://docs.google.com/presentation/d/1tldfCXcuuHV4n_y6pKYtpsUP0K-OZKlLGkHJ8xu5ry0/edit?usp=sharing).


```r
# Group practice plot:
```

------------------------------------------------------------------------

A special case for representing 2 categorical variables:


```r
bw_ggplot + geom_bar(aes(x = smoke, fill = low))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-13-1.png" width="672" />

```r
bw_ggplot + geom_bar(aes(x = smoke, fill = low), position = "fill") 
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-13-2.png" width="672" />

Does the label on the last y-axis make sense? Let's see how to fix it!

## 2. Layering

We can add more layers to our plot by adding other components with `+`.

### a. Adding labels

Remember to **always** add labels to your plots:


```r
bw_ggplot + geom_bar(aes(x = smoke, fill = low), position = "fill") +
  #  Add labels
  labs(
    # Title
    title = "Impact of smoking status on birth weight",
    # Caption with source of data
    caption = "Data obtained from Baystate Medical Center, Springfield, MA in 1986",
    # Label x-axis and y-axis
    x = "The mother smoked during pregnancy",
    y = "Proportion",
    # Legend of color
    fill = "Baby was born with a low birth weight")
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-14-1.png" width="672" />

### b. Controlling scales

We can adjust the limits on the axes:


```r
bw_ggplot + geom_point(aes(x = lwt, y = bwt)) +
  xlim(c(0,250)) + ylim(c(0,5000))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-15-1.png" width="672" />

We can set specific tick marks for better readability:


```r
bw_ggplot + geom_histogram(aes(x = bwt), binwidth = 500, center = 250,
                           color = "blue", fill ="orange") +
  # Adjust the tick marks of the x-axis
  scale_x_continuous(limits = c(0,5000), breaks = seq(0,5000,500))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-16-1.png" width="672" />

Or instead remove unnecessary scale on one axis:


```r
bw_ggplot + geom_boxplot(aes(x = bwt)) + 
  scale_y_continuous(labels = NULL)
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-17-1.png" width="672" />

### c. Faceting

We can produce a plot for each category of a variable with faceting:


```r
bw_ggplot + geom_histogram(aes(x = bwt)) +
  facet_wrap(~ftv)
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-18-1.png" width="672" />

```r
# or for better comparisons
bw_ggplot + geom_histogram(aes(x = bwt)) +
  facet_wrap(~ftv, nrow = 3)
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-18-2.png" width="672" />

### d. Looking for trends

We can look for trends in our data:


```r
# General trend
bw_ggplot + geom_point(aes(x = lwt, y = bwt)) +
  geom_smooth(aes(x = lwt, y = bwt))
```

```
## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-19-1.png" width="672" />

```r
# Linear trend
bw_ggplot + geom_point(aes(x = lwt, y = bwt)) +
  geom_smooth(aes(x = lwt, y = bwt), method = "lm", se = FALSE) +
  # Add lines representing means
  geom_vline(xintercept = mean(birthwt$lwt), color = "green") +
  geom_hline(yintercept = mean(birthwt$bwt), color = "orange")
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-19-2.png" width="672" />

```r
# Smooth histogram
bw_ggplot + geom_density(aes(x = bwt))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-19-3.png" width="672" />

```r
# Smooth boxplots
bw_ggplot + geom_violin(aes(x = bwt, y = smoke))
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-19-4.png" width="672" />

### e. Palettes and themes

We can customize many aspects of our graphs by hand (colors, scales, background color, grid, …) or we can use some themes or palettes other than the defaults.

If you want to get rid of classic "gray box" background:


```r
bw_ggplot + geom_point(aes(x = lwt, y = bwt)) + theme_classic()
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-20-1.png" width="672" />

We can use some specific palettes from the **color brewer** that are color-blind friendly:


```r
library(RColorBrewer)
display.brewer.all(colorblindFriendly = TRUE)
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-21-1.png" width="672" />


```r
bw_ggplot + geom_bar(aes(x = smoke, fill = low)) + 
  scale_fill_brewer(palette = 'Set2')
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-22-1.png" width="672" />

These palettes only works for a limited number of colors. What if we color our data with a numeric variable:


```r
bw_ggplot + geom_point(aes(x = lwt, y = bwt, color = age)) +
  scale_color_gradient(low = "lightblue", high = "darkblue")
```

<img src="02_MoreVisualization_files/figure-html/unnamed-chunk-23-1.png" width="672" />

------------------------------------------------------------------------

## Group Practice

Within your group, pick one of the following question to answer with a plot:

a.  Is smoking during pregnancy associated with a low birth weight (< 2.5 kg)?

b.  Is a history of hypertension associated with the presence of uterine irritability?

c.  Is the mother's age associated with more physician visits during the first trimester?

d. *Come up with your own!*

Make an appropriate plot to answer the question. Adjust the default labels and colors. Post your graph on your group's slide [here](https://docs.google.com/presentation/d/1tldfCXcuuHV4n_y6pKYtpsUP0K-OZKlLGkHJ8xu5ry0/edit?usp=sharing).


```r
# Group practice plot:
```

------------------------------------------------------------------------

## 3. Exporting a graph

You can use RStudio's user-friendly buttons to export plots, or use code. If using code, exporting a graph takes 3 steps:

1.  Naming a file to write to.
2.  Running the graph function(s).
3.  Closing the file.


```r
# Where is this plot saved to?
jpeg('mygraph.jpeg') 
bw_ggplot
dev.off()
```

```
## png 
##   2
```

------------------------------------------------------------------------

Here are some other resources that can help make your `ggplot`s look nicer:

-   [Function reference](https://ggplot2.tidyverse.org/reference/index.html)
-   [R Cookbook](http://www.cookbook-r.com/Graphs/)
-   [ggplot Cheat sheet](https://github.com/rstudio/cheatsheets/blob/main/data-visualization.pdf)
