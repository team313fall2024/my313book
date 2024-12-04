---
title: "Reproducibility"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---

In this worksheet, we will:

-   Discuss ways to code that make it easier to share your work with 1) your future self, and 2) your [collaborators](https://phdcomics.com/comics/archive.php?comicid=1689).
-   Reproduce some results that someone shared by publishing their code and data (which is still not very common though).

## 1. Coding practices

Imagine that you ran some analyses about a year ago and got some feedback on your report. Now you want to rerun what you did with some updates, but when you open up your old code, it looks like this:


```r
install.packages('tidyr')
library(tidyr)
billboard_long <- pivot_longer(billboard, cols = starts_with("wk"), names_to = "week", values_to = "rank")
billboard_long$rank <- as.numeric(billboard_long$rank)
length(unique(billboard_long$artist))
table(billboard_long$artist)
hist(billboard_long$rank, col='grey', breaks = 100)
billboard_long$week_num <- as.numeric(gsub("wk", "", billboard_long$week))
model_glm <- glm(rank ~ week_num, data=billboard_long, family=gaussian)
summary(model_glm)
plot(billboard_long$week_num,billboard_long$rank)
hist(residuals(model_glm))
plot(fitted(model_glm), residuals(model_glm))
abline(h=0, col='red')
install.packages('lubridate')
library(lubridate)
billboard_long$month <- month(billboard_long$date.entered)
plot(billboard_long$month,billboard_long$rank)
model2 <- lm(rank ~ month, data=billboard_long)
summary(model2)
hist(residuals(model2))
plot(fitted(model2), residuals(model2))
abline(h=0, col='blue')
mean_rank_by_artist <- tapply(billboard_long$rank, billboard_long$artist, mean)
install.packages('ggplot2')
library(ggplot2)
ggplot(billboard_long, aes(x=week_num, y=rank)) +
  geom_line(aes(color=artist)) +
  geom_smooth(method='lm', se=FALSE) +
  labs(title="Ranking Over Time", x="Week Number", y="Rank")
install.packages('dplyr')
library(dplyr)
billboard_filtered <- billboard_long %>% 
  filter(artist == "U2" | artist == "Madonna" | artist == "Mariah Carey")
billboard_long <- billboard_long |> 
  group_by(artist,track) |> 
  mutate(rank_diff = rank - lag(rank))
```

Looking at the code chunk above, list at least 3 things that you wish your past-self had done differently to make it easier to re-run this analysis:

1.  Add indentations, comments `#`

2.  Break into chunks

3.  Call tidyverse instead of each individual package, put at the top

4.  Use the new pipe `|>`

------------------------------------------------------------------------

#### **Try it! Which of the following do you think is the best way to name a variable?**


```r
x <- 156.2
myweightonmondayaftercheesepizza <- 156.2
my_weight_on_monday_after_cheese_pizza <- 156.2
My_Weight_Monday <- 156.2
weight_monday <- 156.2
weight.monday <- 156.2

_
```

**Write sentences here.**

------------------------------------------------------------------------

## 2. Reproducibility

Reproducibility is **the ability to reproduce data analysis results given the dataset(s) and code.** The current standard of reproducibility calls for data and code to be made publicly available. *Why do you think this standard is important?*

Keep in mind that analysis that is reproducible is not always robust [ ](https://the-turing-way.netlify.app/_images/reproducible-definition-grid.svg) (or correct).

Why aren't all quantitative research papers reproducible? Here are some challenges:

-   Getting permission to publish raw/identifiable data.
-   Complexities of data cleaning.
-   Version control of files *(what we're talking about next week with GitHub!).*
-   Package/software updates.
-   Random simulations.

With this last one, you can "set seed" to get identical results with a "random" simulation:


```r
# From 100 possibilities
x <- 1:100

# Everyone will get a different answer here (random observation)
sample(x,size=1)
```

```
## [1] 99
```


```r
# Everyone will get the same answer here (same "random" observation)
set.seed(313)
sample(x,size=1)
```

```
## [1] 64
```

------------------------------------------------------------------------

### Group Practice

Using the *analysis-datasharing.Rmd* and *Data-Sharing-Policies_2017-01-25.xlsx*, attempt to reproduce the following from the [Vasilevsky et al.'s article](https://peerj.com/articles/3208/):

-   Table 1
-   Figure 1

What were the challenges your group faced when trying to reproduce these results?
