---
title: "Shiny Apps with Datasets"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---

In this worksheet, we will:

-   See more examples of widgets with reactive output.
-   Use a Shiny app to interact with a dataset.
-   Host a Shiny app on the web.

## 1. More on the Shiny structure

Recall that Shiny apps all have three basic components as shown below:


```r
ui <- ...		             # 1. user interface

server <- function( ) {	 # 2. server function
...
  }

shinyApp(...)			 	     # 3. call to run the app
```

### a. User interface

The user interface, `ui`, controls the layout and appearance of the app: add titles, text, images, and other HTML elements. We define the input of the app with widgets for example.


```r
# Define UI
ui <- fluidPage(
  
  # Add overall title
  titlePanel("title of the app"),
  
  # Define a side bar layout with a main panel
  sidebarLayout(
    
    # Define the side bar
    sidebarPanel(
    # Add some radio buttons
      radioButtons(inputId = "buttons", ,
                   label = h3("title of the buttons"),
                   choices = list("first choice label" = 1,
                                  "second choice label" = 2),
                   selected = 1), # value selected by default
    ), # end of side bar options
    
    # Define the main panel
    mainPanel(h3("title of the panel"),
            p('text in the panel'),
            plotOutput("name_output"),
            img(src = 'www/shiny.svg')
            )
    )
)
```

### b. Server

The `server` function contains the instructions needed to build the app: that's where we will have R code to produce an output based on any input information (for example, create a plot based on values of `buttons`).


```r
# Define server function required to produce an output
server <- function(input, output) {

  # Use renderPlot to define the output plot and indicate that:
  # 1. This object is "reactive": automatically re-executed when inputs change
  # 2. Its output type is a plot
  output$name_output <- renderPlot({

    # Execute code for each conditions for each input button
    if (input$buttons == 1){
      barplot(table(input$buttons))
      }
    else if (input$buttons == 2){
      hist(input$buttons)
      }
    })
}
```

### c. Run the app

Finally the `shinyApp` function creates Shiny app objects from an explicit `ui`/`server` pair.


```r
# To run the app
shinyApp(ui = ui, server = server)
```

------------------------------------------------------------------------

#### **Try it! Open a new R file. Combine the three sections in the code chunks above and run the app. There should be an error with the second button. Can you fix it?**

------------------------------------------------------------------------

## 2. Interact with a dataset

Now that we can see how some basic R code can run in the app, let's see how we can use one to interact with a dataset. *Note: Make sure your dataset is saved in the same folder as your app.*

------------------------------------------------------------------------

### Group practice

> Open up the `Films_App.R` to see an example.

1.  Run the app. Does it work? If not, why? Look at the message in the console.
2.  On which row(s), does the app define which variable to analyze? Change it to another variable.
3.  Add another check box that if selected, would also display the standard deviation of this variable.

------------------------------------------------------------------------

## 3. Adding a choice for selecting variables

Let's say we want to add some functionality to this app and allow the user to select between the *Days* and *Budget* variables. What would we need to update in our app to allow for this?

**Some radio buttons for choosing a variable. Output for each variable (if/else if).**

------------------------------------------------------------------------

### Group practice

Keep editing `Films_App.R`:

1.  Add the option to choose between the two variables, *Days* and *Budget*.
2.  Depending on the variable, make a histogram with appropriate labels.
3.  Depending on the variable, calculate the corresponding mean and standard deviation.

------------------------------------------------------------------------

## 4. Hosting your app on the web

For [Project 3](https://utexas.instructure.com/courses/1399995/assignments/6764341), you will make an app that is accessible through the web. There are several ways to do this, but [Posit](https://www.shinyapps.io/) allows you to streamline up to 5 apps for free.

Create a free account using your gmail or Github credentials, or set up a separate log-in. Once you sign in, you'll see your *shinyapps.io* dashboard which will walk you through the steps to launch your first app:


```r
install.packages('rsconnect')
```


```r
# Copy/paste the authorization code from the dashboard and run it here:
```


```r
# Update your file path below to launch your app!
library(rsconnect)
rsconnect::deployApp('path/to/your/app')
```

------------------------------------------------------------------------

## Summary

To create your own Shiny app:

-   Make a directory/folder named `myapp` for your app.

-   Save your `app.R` script inside that directory.

-   Launch the app with `Run App`.

-   Deploy your app online!

Refer to the tutorial for [Shiny Basics](https://shiny.posit.co/r/getstarted/shiny-basics/lesson1/) for step-by-step details.
