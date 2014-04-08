---
layout: post
title:  "Otra aplicación Shiny"
description: "R Development."
date:   2014-04-08 15:00:00
categories: programacion
tags: [R,programación,shiny]
image:
  feature: texture-feature-02.jpg
  credit: José Manuel Prieto Blázquez
  creditlink: http://jmprietob.github.io
comments: true
share: true
---

Utilizando unos datos espaciales de puntos con una serie de valores, la aplicación muestra un par de gráficos ggplot para una serie de datos de varios años. 
El mapa de puntos se genera para la especie y el año seleccionado. El color depende del valor de la variable y se dibuja sobre un mapa de google.

El código de la aplicación es el siguiente:

### server.R

{% highlight r %}
library(shiny)
library(ggplot2)
library(gridExtra)
library(ggmap)
library(markdown)

load("datos/madrid.RData")

shinyServer(function(input, output) {

  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ## Reactive Functions
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
  especie <- reactive({
    subset(defoliacion, Especie==input$especie) 
  })
  
  espano <- reactive({
    subset(defoliacion, Especie==input$especie & Temporada==input$year)
  })
  
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ## Output 1 - plot
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  
  output$plot <- renderPlot({
    
    datosEsp <- especie()
    
     graficoCaja <- ggplot(datosEsp, aes(factor(Temporada), defo))+
       geom_boxplot(fill="grey80",colour="#56B4E9",size = 1,outlier.colour="#56B4E9")+
       labs(x = "temporada", y = "%")
     graficoLinea <- ggplot(datosEsp, aes(Temporada, defo))+
       geom_point(colour="#56B4E9",size = 1)+stat_smooth(method="loess")+
       stat_smooth(method="lm",size = 1,fill="blue",colour="darkblue")+
       labs(x = "temporada", y = "%")
     
    #output
    multiplot<-grid.arrange(graficoCaja,graficoLinea, ncol=2, main = input$especie)
    
    print(multiplot)
    
  })
  
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ## Output 2 - Map
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  
  output$map <- renderPlot({
    
    datosEspAno <- espano()
    
    map.base <- get_googlemap(
      center= cbind(-3.75,40.55),
      maptype = 'terrain', ## (roadmap, terrain, satellite, hybrid)
      zoom = 9, ## 
      scale = 2, ## Set it to 2 for high resolution output
    )    
    map.base <- ggmap(map.base, extend = "panel") + coord_cartesian() + coord_fixed(ratio = 1.5)
    
    ## Main ggplot
    map.final <- map.base +
      geom_point(data=datosEspAno, aes(x=x, y=y),colour='white', size=9)+
      geom_point(data=datosEspAno, aes(x=x, y=y, colour = defo),size=8) +
      geom_text(data=datosEspAno,aes(x=x, y=y,label=round(defo, digits = 1)),colour="white",size=6, fontface=2,hjust=0, vjust=0)+
      geom_text(data=datosEspAno,aes(x=x, y=y,label=round(defo, digits = 1)),colour="black",size=6, hjust=0, vjust=0)+
      scale_colour_gradient(low = "forestgreen",high = "greenyellow")      
    
    print(map.final)
    
  }, width = 800, height = 800)

})
{% endhighlight %}

### ui.R
{% highlight r %}
library(shiny)
library(ggplot2)
library(ggmap)
library(markdown)

shinyUI(pageWithSidebar(
  
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ## Application title
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  headerPanel("Demo shiny application"),
  
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ## Sidebar Panel
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  sidebarPanel(
    
    wellPanel(
      helpText(HTML("<b>R, Mapas, ggplot</b>")),
      HTML("Select the data you want to display"),
      submitButton("Refresh")
    ),
    
    wellPanel(
      helpText(HTML("<b>BASIC SETTINGS</b>")),     
      helpText("Data from year 2002 to 2012"),
      sliderInput("year", "Year of analysis:",min = 2002, max = 2011, step = 1, value = 2011),
      helpText(HTML("<b>Selection of tree species</b>")),
      selectInput("especie", "Select one:",choice = c('Quercus ilex', 'Quercus pyrenaica', 'Pinus silvestris', 'Pinus pinea'))
    )   
  ),
  
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ## Main Panel
  ## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  
  mainPanel(
    tabsetPanel(
      tabPanel("Intro", includeMarkdown("docs/introduction.md")),     
      tabPanel("Grafs", plotOutput("plot")),
      tabPanel("Map", plotOutput("map")),
      tabPanel("About", includeMarkdown("docs/acercade.md"))        
    )
  )
  
))
{% endhighlight %}

### La aplicación
El código y los datos se puede descargar del
[repositorio de Github](https://github.com/jmprietob/rapps/tree/master/demo). Se puede probar la aplicación shiny alojada en [shinyapps](https://my.shinyapps.io/) en [Aplicación demo](https://jmprietob.shinyapps.io/demo/).

