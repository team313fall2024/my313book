---
title: "Shiny Basics"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---

In this worksheet, we will:

-   Explore what Shiny Apps can do.
-   Define the basic structure of Shiny Apps.
-   Build some simple Shiny Apps.

## 1. Shiny App Gallery

Shiny is a way to build web-based apps that allow users to interact with R in some way. Let's start by taking a look at some really nice (and complicated) examples:

1.  Pick 2 or 3 of the apps from the [Shiny Gallery](https://shiny.posit.co/r/gallery/#user-showcase) to play around with. Try interacting with all of the possible options.

2.  What happens when you change values or select different options?

3.  For each app you explored, list one functionality that you found especially interesting below:

    -   slider (changing values)
    -   interactive map (clicking on some points to get data)
    -   filter some categories (check on/off)
    -   changing colors/opacity on a map
    -   updated data every 15 minutes (NY transportation)

## 2. Basic Structure of a Shiny App

### a. Components

Shiny apps all have three basic components (don't run the code below - I've included it just to show you the outline of an app):


```r
ui <- ...		             # 1. user interface

server <- function( ) {	 # 2. server function
...
  }

shinyApp(...)			 	     # 3. call to run the app
```

We'll start by building the user interface section, which is where you set the layout of the page (title, sidebars, panes, etc.) and define user inputs through widgets, which we'll see later today.

------------------------------------------------------------------------

### Group practice

> Open up the "Hello_App.R" to see an example.

Make the following changes to "Hello_App.R" *Hint: use the documentation to look up anything you need that we haven't covered yet!*

1.  Update the title of the shiny app to your name.
2.  Center the title of the side bar, change it to read "My favorite book," and add a line break before displaying the title and author of your favorite book.
3.  Change the title of the main panel to read "Book description" in italics and in a different color. Then write a short 1-2 sentence summary of the book in a different font.

------------------------------------------------------------------------

Here are some more examples of basic Shiny apps:


```r
runExample("01_hello")      # a histogram
runExample("02_text")       # tables and data frames
runExample("03_reactivity") # a reactive expression
runExample("04_mpg")        # global variables
runExample("05_sliders")    # slider bars
runExample("06_tabsets")    # tabbed panels
runExample("07_widgets")    # help text and submit buttons
runExample("08_html")       # Shiny app built from HTML
runExample("09_upload")     # file upload wizard
runExample("10_download")   # file download wizard
runExample("11_timer")      # an automated timer
```

### b. Reactive output

Now let's incorporate the ability for our Shiny app to produce **reactive output**, which means that an output will changed based on a selection by the user.

To do this, we'll need to:

1.  Add [widgets](https://shiny.posit.co/r/gallery/widgets/widget-gallery/) to our user interface.
2.  Update the server function to render the output we want based on input from the widgets.

Here are some common widgets: ![](https://shiny.posit.co/r/getstarted/shiny-basics/lesson3/images/widgets-gallery.png)

------------------------------------------------------------------------

### Group practice

> Open up the "Widget_App.R" to see an example.

1.  Change the radio buttons for starting values to have 5 options that are different than the current ones. Include both negative and positive values.
2.  Instead of choosing a number to add to the starting value, update the select box to have three options of numbers to multiply by the starting values. Update all relevant text in the app as well as the output.

------------------------------------------------------------------------

### Project 3

Your final project for the course is going to be building a Shiny app. Take a minute now to read over the instructions for [Project 3](https://utexas.instructure.com/courses/1399995/assignments/6764341) to see what you are going to be required to do.
