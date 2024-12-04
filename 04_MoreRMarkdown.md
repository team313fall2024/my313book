---
title: "SDS 313  More R Markdown"
author: "Instructor"
date: "2024-12-02"
output:
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
    number_sections: true
    theme: journal
  pdf_document:
    toc: true
    toc_depth: '3'
---

In this worksheet, we will:

-   Discuss more advanced metadata and code chunk options
-   Use the results of our code to format tables
-   Knit into other formats such as pdf, word, or slides
-   Create your own R Markdown file

We will discuss the structure of the R Markdown file with YAML metadata, text editing, and embedded code.

# More YAML metadata

There are many different options added at the top of the document compared to last time! Note that the options after `hmtl_document:` are exclusively for `hmtl` outputs. For example, the `toc` options add a table of contents.

# More code chunk options

We can control some global options for the code chunks at the beginning of the document:



We could still set different options for each code chunk. For example, hide the following code chunk:



# Format tables

Sometimes the outputs of our code are nice enough that they could be formatted into tables directly with `kable()`:


```r
# Compare means of IMDB across genres
knitr::kable(
  # Table to show: median for each genre with aggregate
  aggregate(IMDB ~ Genre, data = films, FUN = median), 
  # Options for the table 
  col.names = c("Movie Genres", "Median IMDB"), 
  digits = 1
  )
```



|Movie Genres     | Median IMDB|
|:----------------|-----------:|
|Action/Adventure |         7.1|
|Animation        |         7.0|
|Comedy           |         6.6|
|Drama            |         7.6|

For even more options, you could use the `kableExtra`package. Check out the [R Documentation](https://www.rdocumentation.org/packages/kableExtra/versions/1.4.0) for more details! 

# Knitting into different formats

Let's try generating other types of output. Click on `File > New File > R Markdown` then choose one of the following options:

-   Document: `HMTL`, `PDF`, `Word`

-   Presentation: any of these options

Note that: inserting `\newpage` or `\pagebreak` to add a new page for pdf or word documents respectively while the headers `##` define a new slide for the presentation.

------------------------------------------------------------------------

# Group Practice

You will explore the `shelter` dataset, containing information about a sample of cats and dogs that arrived at the Austin Animal Center (data obtained from the [City of Austin data portal](https://data.austintexas.gov/)).

Within your group:

1.  Create your own R Markdown file and save it as `Group_*yournumber*`.
2.  Change the title and add all contributors as authors.
3.  Import the `shelter` dataset, briefly introduce the data and cite its source. Include a cute picture of cats/dogs that you found online (or if you have cats/dogs, include a picture of your own!).
4.  Take a quick look at the data. Come up with a data question involving at least two variables from this dataset.
5.  Write a sentence or two to interpret your findings. 
6.  Knit your R Markdown file in different formats. 

Upload your favorite output [here](https://drive.google.com/drive/folders/1rfFLhdBU482dBbfURgR5Qph4Rs0dx1Uw?usp=sharing).
