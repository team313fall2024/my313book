---
title: "SDS 313  R Markdown"
output: html_document
---

In this worksheet, we will:

-   Understand the *source* of .Rmd files
-   Play around with advanced markdown edits
-   Include code to investigate a dataset
-   Knit .html reports

Here are some examples of knitted R Markdown files:

-   [Our textbook, using "bookdown"](https://r4ds.had.co.nz/)
-   The "handouts" from the second week of class
-   [.pdf articles](https://github.com/svmiller/svm-r-markdown-templates/blob/master/article-example/svm-rmarkdown-article-example.pdf)

We will discuss the structure of the R Markdown file with YAML metadata, text editing, and embedded code.

## 1. YAML metadata

The settings and parameters controlling the appearance and structure of the document are defined at the top of the file, enclosed by triple dashes `---`. This metadata provides key information about the document, such as the `title`, `author`, `output` format, with sometimes other settings like table of contents, theme, and code chunk options.

## 2. Text editing

Let's try out basic Markdown features with different headers, numbered or bulleted lists, bold or italic text, ...

### a. Headers

# For

## Example

### These

#### Are

##### Different

###### Headers

######Does not knit as a header

Make sure to leave a space after \#'s so that they are interpreted as headers. Similarly, leave a blank line between sections.

### b. Text formatting

You can use some basic formatting to highlight some part of the text:

**bold**, *italic*, or ***bold and italic***

~~strikethrough text~~

> Create a blockquote

To refer to R objects (variables, datasets, functions, ...) within the text, we usually use the slanted quotes. Remember the `ggplot()` function?

### c. Lists and bullet points

Create a list:

1.  Here
2.  Are
3.  Four
4.  Things

Or some bullet points:

-   bullet 1

    -   sub-bullet 1

-   bullet 2

    -   sub-bullet 2

-   bullet 3

    -   sub-bullet 3

### d. HTML hyperlinks, images, tables, formulas, etc.

We can include external links and images:

[Here is a hyperlink to Canvas](https://canvas.utexas.edu)

Below is an image from a URL (for local images, just specify the file path in place of the URL):

![](https://news.utexas.edu/wp-content/uploads/2021/10/bevo-9841-2100x1398-e2000d2b-a7a1-448c-83d5-281310430e66-1024x682.jpg)

We can format tables:

| Tuesday              | Thursday                   |
|----------------------|----------------------------|
| Introduce R Markdown | Create your own R Markdown |

And here is a math formula:

$$mean(X) = \frac{\sum x_i}{n}$$

> Note: text editing is fairly easy to do with the `Visual` mode: it works more like a standard text editor.

## 2. Embedded R code

The advantage of a R Markdown document is that it incorporates R code in chunks within the text.

Code chunks will be executed when knitting and the output will be shown in the output file (usually a *html* output in this course).


```r
# We already tried this!
5+1+2
```

```
## [1] 8
```

### a. Packages and R objects

R Markdown is designed to be independent of the R workspace and each time you knit, it acts like it starts with a clean R session. All packages should be loaded and all objects used in the code should be created **within the document**.


```r
# Load functions from the ggplot2 package
library(ggplot2)
```

```
## Warning: package 'ggplot2' was built under R version 4.3.3
```

When importing a dataset, make sure it is in the same folder as the R Markdown file so you do not have to worry about specifying a path:


```r
# Import a dataset
library(readr)
```

```
## Warning: package 'readr' was built under R version 4.3.3
```

```r
films <- read_csv("films.csv")
```

```
## Rows: 151 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Title, Studio, Genre, Rating
## dbl (7): Year, Gross, PercentDomestic, Rotten, IMDB, Days, Budget
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Take a quick look at the dataset:


```r
head(films)
```

```
## # A tibble: 6 × 11
##   Title      Studio Genre  Year  Gross PercentDomestic Rotten  IMDB Rating  Days
##   <chr>      <chr>  <chr> <dbl>  <dbl>           <dbl>  <dbl> <dbl> <chr>  <dbl>
## 1 Avatar     Fox    Acti…  2009 2.78e9           0.273     83   8   PG-13    238
## 2 Titanic    Par.   Drama  1997 2.19e9           0.301     88   7.6 PG-13    219
## 3 Harry Pot… WB     Acti…  2011 1.34e9           0.284     96   8.1 PG-13    133
## 4 Skyfall    Sony   Acti…  2012 1.11e9           0.275     92   7.8 PG-13    122
## 5 The Dark … WB     Acti…  2012 1.08e9           0.413     87   8.6 PG-13    147
## 6 Star Wars… Fox    Acti…  1999 1.03e9           0.462     57   6.5 PG       261
## # ℹ 1 more variable: Budget <dbl>
```

### b. Options for code chunks

We can choose to evaluate, or not, a code chunk when knitting with `eval =`:


```r
# There is an error in this code, can you fix it? 
hist(films$Rotten)
```

*Note: R Markdown does not knit if there is an error in any code chunk.*

We can also choose to hide, or not, a code chunk when knitting with `echo =`:

<img src="RMarkdown_files/figure-html/unnamed-chunk-6-1.png" width="672" />

### c. Inline code

You can also execute code within text! For example, in the `films` dataset, the mean value of IMDB is 7.0350993 or 7.04 when rounded by 2 decimals.

------------------------------------------------------------------------

## Group Practice

Within your group, pick one of the following question to investigate:

a.  Is there a difference in review ratings across the different genres?

b.  Is there a relationship between the two types of review ratings?

c.  Is there a difference in review ratings across the different studios?

d.  *Come up with your own!*

Then investigate the question by:

1.  Making an appropriate plot
2.  Reporting appropriate statistics formatted in a table
3.  Writing a sentence in bold to interpret your findings

Finally, knit this R Markdown file and take a screenshot including the plot, table, and sentence to post it on your group's slide [here](https://docs.google.com/presentation/d/1Ty6tcYxFG_iABj5zfzZQ65gLuVL3wiloD52QVS7XPG0/edit?usp=sharing).


```r
# Group practice code:
```

------------------------------------------------------------------------

Check out the [RMarkdown Cheat sheet](https://www.rstudio.com/wp-content/uploads/2015/02/rmarkdown-cheatsheet.pdf) for more practice!
