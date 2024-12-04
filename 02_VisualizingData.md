---
output: html_document
---

# Visualizing and Describing Data

In this worksheet, we will:

-   Choose how to plot different types of data with basic R plots
-   Report univariate and bivariate descriptive statistics
-   Include titles and labels to our plots

We will be working with *R Markdown* files during lectures throughout the rest of the semester (you will learn to make our own in a couple of weeks). The advantage of a R Markdown document is that it incorporates R code in chunks within the text.

Hit the run button (little green triangle) in the upper right to see this in action:


```r
# Run this code chunk
5+1+2
```

```
## [1] 8
```

## 0. Import Dataset

The default working directory of any *R Markdown* file will be the folder where it is saved. Always make sure that your dataset is in the same folder as the *R Markdown* file, so you won't need need to worry about file paths:


```r
med <- read.csv('medicaldata.csv')
```

What code can we use to get familiar with the structure of this dataset? What type of variables does it contain?


```r
# Explore the structure of this dataset
str(med)
```

```
## 'data.frame':	155 obs. of  9 variables:
##  $ Subject    : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ Age        : int  56 70 54 38 55 53 42 53 45 64 ...
##  $ Diabetic   : int  1 0 0 1 1 1 1 0 0 1 ...
##  $ Edema      : int  0 1 1 0 0 0 0 0 0 0 ...
##  $ Cholesterol: int  302 176 244 279 322 280 562 259 281 231 ...
##  $ Glucose    : int  148 85 89 78 197 166 118 103 126 119 ...
##  $ BP         : int  72 66 66 50 70 72 84 52 88 80 ...
##  $ BMI        : num  33.6 26.6 28.1 31 30.5 25.8 45.8 43.3 39.3 29 ...
##  $ Platelet   : int  221 151 183 136 204 373 251 258 244 295 ...
```

We can also take a quick look at descriptive statistics for all variables in the dataset:


```r
# Summarize all variables in the dataset
summary(med)
```

```
##     Subject           Age          Diabetic          Edema        
##  Min.   :  1.0   Min.   :28.0   Min.   :0.0000   Min.   :0.00000  
##  1st Qu.: 39.5   1st Qu.:41.0   1st Qu.:0.0000   1st Qu.:0.00000  
##  Median : 78.0   Median :48.0   Median :0.0000   Median :0.00000  
##  Mean   : 78.0   Mean   :48.9   Mean   :0.3613   Mean   :0.07097  
##  3rd Qu.:116.5   3rd Qu.:56.0   3rd Qu.:1.0000   3rd Qu.:0.00000  
##  Max.   :155.0   Max.   :78.0   Max.   :1.0000   Max.   :1.00000  
##   Cholesterol        Glucose          BP             BMI           Platelet    
##  Min.   : 120.0   Min.   : 71   Min.   :44.00   Min.   :19.40   Min.   : 70.0  
##  1st Qu.: 249.0   1st Qu.: 99   1st Qu.:64.00   1st Qu.:27.95   1st Qu.:203.5  
##  Median : 302.0   Median :111   Median :70.00   Median :31.60   Median :265.0  
##  Mean   : 361.2   Mean   :119   Mean   :71.18   Mean   :32.91   Mean   :272.6  
##  3rd Qu.: 399.5   3rd Qu.:136   3rd Qu.:79.00   3rd Qu.:37.00   3rd Qu.:335.0  
##  Max.   :1775.0   Max.   :197   Max.   :98.00   Max.   :55.20   Max.   :563.0
```

The units for the variables are: `Age` (years), `Cholesterol` (mg/dL), `Glucose` (mg/dL), `BP` (mmHg), `BMI` (kg/m\^2), `Platelet` (count in thousands).

## 1. Summarizing one variable

### a. One Categorical Variable

A categorical variable defines membership in a group. When describing categorical variables, we pay attention to which category are the most and least common.


```r
# Frequencies of each category of Diabetic
table(med$Diabetic)
```

```
## 
##  0  1 
## 99 56
```

```r
# Proportions of each category of Diabetic (relative to total)
prop.table(table(med$Diabetic))
```

```
## 
##         0         1 
## 0.6387097 0.3612903
```


```r
# Represent frequencies with bars
barplot(table(med$Diabetic))
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-6-1.png" width="672" />

What do 0 and 1 represent though? We could choose to label the categories:


```r
# Change the labels of the categories
med$Diabetic <- factor(med$Diabetic, labels = c('No','Yes'))

# Update bar plot
barplot(table(med$Diabetic))
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-7-1.png" width="672" />

> Tip: we should always add a title and labels to our plot to ensure that there is enough information about the data being presented.


```r
# A better version of the bar plot
barplot(table(med$Diabetic),
        # add a title
        main = 'Frequency of Diabetic Status',
        # add labels to the axes
        xlab = 'Diabetic Status', ylab = 'Frequency of Patients',
        # adjust limits to the y-axis
        ylim = c(0,100),
        # adjust colors of the bars
        col=c('aquamarine','purple'))
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-8-1.png" width="672" />

In summary, for 1 categorical variable:

-   Describe with: *frequencies or proportions*
-   Display with: *bar plot*

### b. One Numeric Variable

A numeric variable is a quantitative measurement. When describing numeric variables, we pay attention to what a typical value is (center) and how the values vary from each other (spread), what values are most common and what values are rare.


```r
# Represent numeric values in bins with a histrogram
hist(med$Cholesterol)
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-9-1.png" width="672" />

In statistics, center is a measure that represents a typical value for a numeric variable (we typically report the *mean* or *median*). The *mean* is the arithmetic average of the numeric values: it is the sum of all data values divided by the number of observations. The *median* splits the data in two halves: into the lowest 50% values and the highest 50% values.


```r
# Compare the values of mean and median
mean(med$Cholesterol)
```

```
## [1] 361.1613
```

```r
median(med$Cholesterol)
```

```
## [1] 302
```

Since, usually, not all values are the same, we should also report the spread of a numeric variable. In statistics, we usually use standard deviation or the five-number summary. The standard deviation is the average distance between each data point and the mean of the dataset. In the five-number summary, the first quartile, Q1, separates the data from the lowest 25% values and the third quartile, Q3, separate the data from the highest 25% values).


```r
# Compare the values of sd vs Q1 and Q3
sd(med$Cholesterol)
```

```
## [1] 221.0252
```

```r
fivenum(med$Cholesterol)
```

```
## [1]  120.0  249.0  302.0  399.5 1775.0
```


```r
# Represent the five-number summary with a boxplot
boxplot(med$Cholesterol)
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-12-1.png" width="672" />

> Tip: we can use some options to improve our plots with more options.


```r
# A better version of the boxplot
boxplot(med$Cholesterol,
        # add a title
        main = 'Boxplot of Cholesterol',
        # add labels to the axes
        xlab = 'Cholesterol (mg/dL)',
        # adjust colors of the box
        col = 'lightgreen',
        # adjust the appearance of the points 
        pch = 20, 
        # make the boxplot horizontal
        horizontal = TRUE)
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-13-1.png" width="672" />

In summary, for 1 numeric variable:

-   Describe with:
    -   Center: *mean or median*
    -   Spread: *standard deviation or quartiles*
-   Display with: *histogram or boxplot*

------------------------------------------------------------------------

## Group Practice

1.  Pick a variable from this dataset (other than `Diabetic` or `Cholesterol`). Describe and display its distribution with an appropriate plot.
2.  Change the default color of your plot and update the title and axis labels.
3.  Post your graph on your group's slide [here](https://docs.google.com/presentation/d/1ppw-3etZL_fFghN8aRkW00gVpPgoMacZ1cwhSuJiNkI/edit?usp=sharing).


```r
# Group practice plot:
```

------------------------------------------------------------------------

## 2. Summarizing two variables (bivariate relationship)

### a. Two Numeric Variables

When comparing two numeric variables, we may wonder if high values on one variable are associated with high/low values for another variable.

Correlation describes the strength of a (linear) relationship between two variables. With the function `cor`, we refer by default to the Pearson correlation coefficient which takes values between -1 (strong negative correlation) and 1 (strong positive correlation) with 0 indicating that there is no correlation.


```r
# Find the correlation coefficient
cor(med$BMI,med$BP)
```

```
## [1] 0.4296074
```


```r
# Represent relationship with a scatterplot
plot(med$BMI,med$BP)
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-16-1.png" width="672" />

> Tip: we can use some options to improve our scatterplots.


```r
# A better version of the bar plot
plot(med$BMI,med$BP,
        # add a title
        main = 'Relationship between BMI and Blood Pressure',
        # add labels to the axes
        xlab = 'BMI (kg/m^2)', ylab = 'Blood Pressure (mmHg)',
        # adjust appearance of points
        pch = 20)
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-17-1.png" width="672" />

In summary, with 2 numeric variables:

-   Describe with: *correlation*
-   Display with: *scatterplot*

### b. One Numeric and One Categorical Variable

When comparing a numeric variable for different categories, we may wonder if the distribution of the numeric variable (center, spread) is about the same across all categories or not.


```r
# Compare means
aggregate(BP ~ Diabetic, data = med, mean)
```

```
##   Diabetic       BP
## 1       No 69.62626
## 2      Yes 73.92857
```

```r
# What about the standard deviations of each group?
aggregate(BP ~ Diabetic, data = med, sd)
```

```
##   Diabetic       BP
## 1       No 10.62111
## 2      Yes 12.13389
```


```r
# Compare distributions with a grouped boxplot
boxplot(med$BP~med$Diabetic, 
        main = 'Diastolic Blood Pressure',
        ylab = 'Blood Pressure (mg/dL)',
        xlab = 'Diabetic Status',
        col = c('blue','red'))
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-19-1.png" width="672" />


```r
# Create subsets for each Diabetic group:
Diabetic <- med[med$Diabetic == "Yes",]
NonDiabetic <- med[med$Diabetic == "No",]
```


```r
# Then make a histogram for each group (use same limits on the axes)
hist(Diabetic$BP, 
     main='Histogram of Blood Pressure for Diabetic Patients',
     xlab='Blood Pressure (mmHg))',
     col='lightgreen',
     ylim=c(0,25), xlim=c(40,100))
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-21-1.png" width="672" />

```r
hist(NonDiabetic$BP, 
     main='Histogram of Blood Pressure for Non-Diabetic Patients',
     xlab='Blood Pressure (mmHg))',
     col='lightblue',
     ylim=c(0,25), xlim=c(40,100))
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-21-2.png" width="672" />

In summary, with 1 numeric variable and 1 categorical variable:

-   Describe with: *compare center and spread of each group*
-   Display with: *grouped histogram or grouped boxplot*

### c. Two Categorical Variables

When comparing two categorical variables, we may wonder what are the most and least common categories of one variable across categories of the other variable.

Let's label the `Edema` status variable like we did for `Diabetic` status:


```r
# Change the labels of the categories
med$Edema <- factor(med$Edema, labels=c('No','Yes'))

# Build a contingency table
table(med$Edema, med$Diabetic, dnn = c("Edema", "Diabetic"))
```

```
##      Diabetic
## Edema No Yes
##   No  89  55
##   Yes 10   1
```

```r
# Two ways to look at proportions
prop.table(table(med$Edema, med$Diabetic, dnn = c("Edema", "Diabetic")), 1) # rows
```

```
##      Diabetic
## Edema         No        Yes
##   No  0.61805556 0.38194444
##   Yes 0.90909091 0.09090909
```

```r
prop.table(table(med$Edema, med$Diabetic, dnn = c("Edema", "Diabetic")), 2) # columns
```

```
##      Diabetic
## Edema         No        Yes
##   No  0.89898990 0.98214286
##   Yes 0.10101010 0.01785714
```


```r
# Compare Edema status for Diabetic vs Non-Diabetic
barplot(table(med$Edema, med$Diabetic),
        main = 'Diabetes and Edema Status',
        xlab = 'Diabetic Status', ylab = 'Frequency',
        col = c('lightcoral','lightgreen'),
        legend = TRUE)

# Add a legend
legend("topright", 
       legend = c("No", "Yes"), 
       fill = c('lightcoral','lightgreen'), 
       title = "Edema Status")
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-23-1.png" width="672" />


```r
# Compare Edema status for Diabetic vs Non-Diabetic, relatively
barplot(prop.table(table(med$Edema, med$Diabetic),2),
        main = 'Diabetes and Edema Status', 
        ylab = 'Proportion', xlab = 'Edema Status',
        col = c('lightcoral','lightgreen'),
        legend = TRUE)
```

<img src="02_VisualizingData_files/figure-html/unnamed-chunk-24-1.png" width="672" />

In summary, with 2 categorical variables:

-   Describe with: *row or column proportions*
-   Display with: *two-way frequency table or grouped bar chart*

------------------------------------------------------------------------

## Group Practice

1.  Pick a variable that might be related to Saliva Glucose Level (Glucose). Describe and display the bivariate relationship.
2.  Add your graph on your group's slide [here](https://docs.google.com/presentation/d/1ppw-3etZL_fFghN8aRkW00gVpPgoMacZ1cwhSuJiNkI/edit?usp=sharing).


```r
# Group practice plot:
```

------------------------------------------------------------------------

Here are some other resources that can help make your base R plots look nicer:

-   [Graph parameters](http://www.statmethods.net/advgraphs/parameters.html)
-   [Color list](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf)
-   [Nice color palettes](http://colorbrewer2.org/)
