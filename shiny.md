---
title: "Creating Web App using R"
author:    
    -  "RSN_Thinklabz_Chennai"

output:
  html_document:
    keep_md: true
    
---



# Introduction
   This notes provides vital guidelines and a route map for creating effective Web application using Shiny and related packages in R. The ultimate aim is to create a web app which is appealing to users as well as organized in a way to deliver the intended information about data which is paramount in Data Science. 

**<span style="color:darkgreen">  Why shiny ? </span>**   
Adding interactivity to a data report is highly effective way of communicating since it enables users to explore their interested features of the dataset.
   
**<span style="color:darkgreen">  About Shiny </span>**

   Shiny is an R package that makes it easy to build interactive web apps  straight from R. A web application is an interactive, dynamic web page where user can click on buttons, check box or input text to change how and what data is presented. The apps can be further customized using CSS themes, html widgets, and JavaScript actions.  shinydashboard is yet another package which makes it easy to use Shiny to create dashboards. It provides structure for communicating between user interface and data server, allowing users to interactively change code. 

# Guidelines 
The following are most important guidelines to be followed in order to produce most efficient Application which can be used to obtain insight from the data.   

<span style="color:darkviolet"> 
   1. Understand the context of the problem   
   2. Identify the variable of interest   
   3. List out possible predictors to build meaningful analysis plan including storytelling   
   4. If possible group the predictors under suitable titles   
   5. Do the necessary cleaning process
   6. Prepare Metadata (in case not available) and decide the quantities for numerical and visual summary based on Step 3   
   7. Present the story connecting Steps 3, 4, and 6
</span>   

Having looked at the preparatory step for designing the app, let us dive in to Shiny's world and see what are all the tools we can make use of to create such amazing web application.
 


# Shiny Application Outline
   Shiny application can be created using 4 basic steps in R which are
      
   1. Load the package       
   1. Create and Customize UI (User Interface) part    
   1. Define all the required tasks in server      
   1. Run the shinyapp   
   
Shiny app can be created using a simple R script with the above components in it, anyhow it is advised to keep the app in a separate folder.         
Let us use both shiny and shinydashboard to learn creating an app.  A simple app can be created using following code.  


```r
library(shiny)
library(shinydashboard)

# Defining UI 
user_interface <- dashboardPage(
  dashboardHeader(title = "App title"),
  dashboardSidebar(),
  dashboardBody()
)



# Defining
server_part <- function(input, output) {
  
}

# Running the app
shinyApp(ui = user_interface, server = server_part)
```

We specify all the details for user interface within a Dashboardpage and store it in any name (Here we have used user_interface), similarly we specify rules for actions to be performed as a function and store it in any name (Here we have used server_part) and finally we use shinyApp function to generate the app where we use arguments **`ui`** and **`server`** to mention user interface and server.

##  User Interface (ui)
UI defines the display of the app. Since we are using shiny dashboard for creating apps, the UI part is specified within Dashboard page, which has components as sidebar and body. within the dashboard body we can arrange our objects in tabitems as individual tabitem, within each tabitem we can have various tabs within each tab item. Each reactive component in this ui has an unique ID which will be used for calling and specifying about the component.

## Server
The server of the shiny app defines the processes the data that will be displayed in the UI. Server is a program running on computer that receives request and provides content based on the request. 

      
**<span style="color:darkgreen"> Basic Layout of Shiny app</span>**   
Having known how to create a basic app, we shall look at the layout of application and hierarchical structure of user interface.  

```{=html}
<div id="htmlwidget-e25f0175c5fad4c4bde5" style="width:100%;height:480px;" class="grViz html-widget"></div>
<script type="application/json" data-for="htmlwidget-e25f0175c5fad4c4bde5">{"x":{"diagram":"digraph Shiny_Layout{\n  node[shape=oval\n       fontname=Cursive\n       penwidth=3,color = darkseagreen2, style = filled, \n       fontcolor = indianred4]\n  \"shiny app\";\"ui\";\"server\";\"Dashboard page\";\"Dashboard sidebar\";\n  \"sidebar menu\";\"Dashboard body\"; \"Tab Items\"; \"Tab Item\"; \"Tab set panel\";\n  \"Tab Panel 1\";\"Tab Panel 2\";\"Tab Panel 3\"; \"Fluid rows and Columns in Tab 1\";\n  \"Fluid rows and Columns in Tab 2\";\"Fluid rows and Columns in Tab 3\"; \n  \n  \n  edge[arrowhead=vee]\n  \"shiny app\"->\"ui\";\n  \"shiny app\"->\"server\";\n  \"ui\"->\"Dashboard page\"->\"Dashboard sidebar\"->\"sidebar menu\"\n  \"Dashboard page\"->\"Dashboard body\"-> \"Tab Items\"-> \"Tab Item\"-> \"Tab set panel\"->\n  \"Tab Panel 1\"->\"Fluid rows and Columns in Tab 1\";\n   \"Tab set panel\"->\"Tab Panel 2\"->\"Fluid rows and Columns in Tab 2\";\n    \"Tab set panel\"->\"Tab Panel 3\"->\"Fluid rows and Columns in Tab 3\";\n  graph[nodesep=0.1]\n  \n  }\n  ","config":{"engine":"dot","options":null}},"evals":[],"jsHooks":[]}</script>
```
   
 User interface or display of web app can be customised using following arguments

1. Dashboardheader -  Title of the application      
1. DashboardSidebar - Menu area where tab names of the application is displayed
1. DashboardPage - The main body of the app where report is displayed.

## Components of Sidebar
Within Sidebar we have Sidebar menu which has an **`id`** followed by menuitem which represents the tab names, each of them has unique id called **`tabName`**, this id can be used to navigate between tabs, an example can be seen below

```r
# Defining UI 
user_interface <- dashboardPage(
  dashboardHeader(title='Title of the app'),
  dashboardSidebar(
    sidebarMenu(id = "sidebar_id",
                menuItem("1st Tab title",
                         tabName = "1st Tab ID"),
                menuItem("2nd Tab title", 
                         tabName = "2nd Tab ID"))),
  dashboardBody()
)

# Defining
server_part <- function(input, output) {
}

# Running the app
shinyApp(ui = user_interface, server = server_part)
```


## Layout of the Dashboardpage
A dashboardpage can contain several tabs called **`tabitem`** which are placed within a set(bag) called **`tabitems`**  and within each of the tab we can have several sub-tabs called as **`tabpanel`** which are placed within a Set(bag) called **`tabsetpanel`**. An example can be seen below.



```r
library(shiny)
library(shinydashboard)

# Defining UI 
user_interface <- dashboardPage(
  dashboardHeader(title='Title of the app'),
  dashboardSidebar(sidebarMenu(id = "sidebar_id",
                menuItem("1st Tab title",
                         tabName = "1st Tab ID")
                )
                ),
  dashboardBody(
    tabitems(
    tabItem(tabName = '1st Tab ID',
          tabsetPanel(
            id="tabset_panel ID",
            selected = 'ID of selected tab 
            in this tabset panel',
            type='which type we wish to display tabs with',
            tabPanel("title", value = "id",
##            ...Our report which includes graphs and
##story of data is placed here
                          ),
            tabPanel("title", value = "id_2"
          ))
    )
  )
))
# Defining
server_part <- function(input, output) {
  
}

# Running the app
shinyApp(ui = user_interface, server = server_part)
```

tab panel is the area where we have our customized tables, diagrams and explanations in the app

## Layout of page within tab panel
We can adjust position of items and how much space they occupy by specifying row position and columns, any page in the app can have a maximum of 12 columns and unlimited number of rows which can be scrolled to view. For specifying this we use **`fluidRow()`** and **`column()`**,within column we use **`width`** argument to specify how wide our item must be **`offset`** is used to move the item to particular position from left margin

```r
fluidRow(
    column(width = 4,offset = 2,
      "4"
          )
        )
```

## Reactive components of Shiny
The following are the reactive components which we use   

1. Control widgets - Gets input from user
1. Reactive outputs - Gives back output
   
    Before going in to detail let us also know about Shiny workflow      
     
**<span style="color:goldenrod"> Workflow  </span>**  
  The following graph shows direction of workflow of control widget, render function and reactive output.
   

```{=html}
<div id="htmlwidget-999daa3db1063ab6676b" style="width:100%;height:480px;" class="grViz html-widget"></div>
<script type="application/json" data-for="htmlwidget-999daa3db1063ab6676b">{"x":{"diagram":"digraph UI_Server{\n  node[shape=box\n       fontname=Cursive\n       penwidth=2,color = darkseagreen2, style = filled, \n       fontcolor = blue]\n  \"Control widget in UI\";\n  \"Render function in server\";\n  \"Reactive output in UI\"; \n  \n  \n  edge[arrowhead=diamond]\n  \"Control widget in UI\"->\n  \"Render function in server\"->\n  \"Reactive output in UI\";\n \n  graph[nodesep=0.1]\n  \n  }\n  ","config":{"engine":"dot","options":null}},"evals":[],"jsHooks":[]}</script>
```

**<span style="color:darkgreen">Control widgets</span>**    
These are elements that allows users to provide input to server. They store important values which are automatically updated as the user interacts with the widget. Some of the control widgets are

1. slider
1. dropdown box
1. text input box 
   
Let us see how we can create a Control widget    

**<span style="color:blue">Following components of shiny app should be mentioned in pair one in UI and another in Server</span>**
    
**<span style="color:darkviolet">Slider</span>**    
Slider is used when we wish to control the input values of a dataset within a specific range, we can have the slider to adjust either lower limit or upper limit and also to adjust both. To obtain slider in user interface we use **`sliderInput()`** function, with some important arguments **`inputId`** for ID,**`label`** for slider name,**`min`** minimum value of slider,**`max`** maximum value of slider,**`value`** to specify current value which slider selects, if this value is single number slider has just upper value adjustment available, if it has 2 values both ends of slider can be adjusted.

   Slider for adjusting just upper limit we specify single value
      
   Slider with both upper and lower limits is like a vector with 2 values 
`input$slider[1]` gives us the lower value
`input$slider[2]` gives us the upper value.

```r
### UI_part
sliderInput(inputId="slider_ID",
            label = h3("Slider_name"),
            min = 'minimum value of slider', 
        max = 'maximum value of slider',
        value = 'either single or 2 values of slider limits')

### Server_part
## slider values will be stored in server
## as input object 

# 1st value gives lower limit
# 2nd value gives upper limit
input$slider_ID[1] 
input$slider_ID[2] 
```

**<span style="color:darkviolet">Radio buttons</span>**    
We can use these to give options to be selected by users, several buttons can be kept but only one can be pressed at any time. The widget will return the value of the selected button to the server as a character string.Created using  **`radioButtons()`**, **`choices`** is used to specify options to be selected as a list with element as displaying option and value can be used to update the input value, **`selected`** specify current selected item

```r
### UI_part
radioButtons("ID", label = 'title',
    choices = list("Choice 1" = 1, "Choice 2" = 2, "Choice 3" = 3), 
    selected = 1)

### Server_part
## radio button value will be stored in server
## as input object 
input$radio 
```

**<span style="color:darkviolet">Select box</span>**   
Creates a drop-down list that you can use to select one or more items. The widget will pass the value of the selected items to the server as a vector of character strings. **`selectInput()`** function is used, we specify arguments such as **`ID, label, choices, selected`**, **`multiple`** argument is used to enable multiple option selection 

```r
### UI_part

selectInput("Select_ID", label = 'title', 
    choices = list("Choice 1" = 1, "Choice 2" = 2, "Choice 3" = 3), 
    selected = 1)

### Server_part
## stored at input
 input$Select_ID
```
   
**<span style="color:darkviolet">Text input</span>**   
Users can be asked to give any text as input an this text can be used to produce a message to them. This create a text field to enter text in. The widget will pass the text displayed to the server as a character string. We use **`textInput()`** function to create a text input

```r
### UI_part
textInput("text_ID", label = 'title', value = "Initial text")

### Server_part
input$text_ID
```

   
**<span style="color:darkgreen">Reactive Outputs and Render function</span>**  

These are elements in UI that displays dynamic (changing) content produced by the server. Some outputs formats we use in shiny are text, plot, plotly, table and datatable. For all these outputs, we specify as output in UI and use **`render()`** function in server part. 

**<span style="color:pink">Note: While using more than one statement within render function, we must use curly braces also eg. `renderPlot({ statements})` </span>**

   
**<span style="color:darkviolet">Table Output</span>**      
We can display tables using the table output, this table can be controlled by any of the control widgets. Created using **`tableOutput()`** in ui and **`renderTable`** is used in server to display rectangular data structures

```r
### UI_part
tableOutput('table_ID')

### Server_part

output$table_ID <- renderTable('data')
```

**<span style="color:darkviolet">DataTable Output</span>**    
Data table have sophisticated options like scrolling, searching, paging etc for this we use these kind of outputs. Created using **`dataTableOutput()`** and **`renderDataTable`**. We use **DT** package for this.

```r
### UI_part
dataTableOutput('table')

### Server_part
output$table <- renderDataTable('data_name')
```

**<span style="color:darkviolet">plot Output</span>**   
We include plots in a shiny app using this option. Created using **`plotOutput()`** and **`renderPlot`**

```r
### UI_part
plotOutput(
  outputID="plot_ID")

### Server_part
output$plot_ID <- renderPlot({
  #Prepare data suitably according to widgets    
  d <- data()
  # create plot   
   plot()
    })
```

**<span style="color:darkviolet">plotly Output</span>**   
We can include plotly figures in a shiny app using this option. Created using **`plotlyOutput()`** and **`renderPlotly`**  


```r
### UI_part
plotlyOutput(outputID)
### Server_part
renderPlotly({
  #Prepare data suitably according to widgets    
  d <- data()
  # create plot   
  plot_ly()
})
```

**<span style="color:darkviolet">Text Output</span>**
	These are useful to display texts. Created using **`verbatimTextOutput()`** and **`renderTable`**


```r
### UI_part
verbatimTextOutput(outputId)

### Server_part
output$outputID <- renderText({ input$txt })
```

**<span style="color:darkviolet">ui Output</span>**   
   If we wish to control the input options in an interactive way we can use **`uiOutput`**. In the server we include the Control widgets.


```r
 #ui part
uiOutput(outputId)

## server part
output$outputId=
```

   
**<span style="color:darkviolet">Action buttons</span>**   
These action buttons can be assigned to various tasks if they are pressed. we often use these to navigate from one page to another.Created using **`actionButton(inputId)`** and **`ObserveEvent`**. We use **`updateTabsetPanel`** to change tab panel, **`updateTabItems`** to change tab item and similarly we can change some other items also.



```r
### UI_part
actionButton(inputId)

### Server_part
observeEvent(input$inputId, {
    updateTabsetPanel("tab_panel_ID",selected = "tab_item_ID")
  }) 
```


**<span style="color:darkviolet">Download buttons</span>**         
We can place download buttons for the analysis we have dne using this option.when clicked, it will initiate a browser download. Created using **`downloadButton()`** which has arguments **`outputId`** for ID,**`label`** for label to display and **`icon`** is used to display icon. Server part includes **`downloadHandler()`** function which has arguments **`filename`** which is inturn stored as function with name of the file which will be downloaded,**`content`** which has the data to be downloaded stored as a function,


```r
### UI_part
downloadButton(
  outputId,
  label = "Download",
  icon = shiny::icon("download")
)

### Server_part
output$downloadData <- downloadHandler(
    filename = function() {
      paste("data-", Sys.Date(), ".csv", sep="")
    },
    content = function(file) {
      write.csv(data, file)
    }
  )
```



# Final note
This notes attempted to provide guidelines for creating interactive web application using shiny and shinydashboard. there are many options to explore in the R shiny world. So it is up to learner's quest to improve the app in all the way possible

<div class="tocify-extend-page" data-unique="tocify-extend-page" style="height: 0;"></div>
