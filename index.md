---
title       : Coursera Slidify Project
subtitle    : Car miles/gallon prediction
author      : xmflower
job         : a coder
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Overview
  
1. This Shiny application is to predict car's miles/(US) gallon by its transmission, number of cylinders, and gross horsepower.  
2. The prediction model is created from mtcars of R datasets.   
3. The model is a linear model which has transmission, number of cylinders, gross horsepower as predictors, and miles/gallon as responder.   

--- 

## How to Use
  
To use the application, do following steps in the left panel:  
  
1. select the car's transmission    
2. select the car's number of cylinders  
3. enter the car's gross horsepower  
4. click "submit" button  
  
Then you get the prediction of the car's miles/(us) gallon in the right panel.  

---

## Details of ui.R

In the sidebarPanel, create a radioButton, a selectInput, a numericInput and a submitButton:

```r
radioButtons("am", "Transmission: ", c("automatic" = 0, "manual" = 1))
selectInput("cyl", "Number of cylinders: ", c("4" = 4, "6" = 6, "8" = 8))
numericInput("hp", "Gross horsepower (50-350):", 50, min = 50, max = 350, step = 1)
submitButton("submit")
```
In the mainPanel, create instructions, show the values user entered, and the prediction of MPG:

```r
h3("Instuctions:")
# ...
h4("Transmission")
verbatimTextOutput("oam")
# ...
h4("Prediction of Miles/(US) gallon:")
verbatimTextOutput("ompg")
```

--- 

## Details of server.R

Put the input parameters into output parameters, create a linear model of mpg by am, cyl and hp, predict with input data, and then put the prodiction to the corresponding output parameter:

```r
function(input, output){
           
    output$oam <- renderPrint({if (input$am == 0) "automatic" else "manual"})
    output$ocyl <- renderPrint({input$cyl})
    output$ohp <- renderPrint({input$hp})
    
    output$ompg <- renderPrint({
        fit <- lm(mpg ~ am + cyl + hp, data = mtcars)
        car <- data.frame(am = as.numeric(input$am), cyl = as.numeric(input$cyl), hp = input$hp)
        predict(fit, newdata = car)[[1]]})
}
```





