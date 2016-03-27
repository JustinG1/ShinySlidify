# ShinySlidify
For the coursera assignment

This is documentation for the app located at https://jgold.shinyapps.io/Shiny/

The app will take an input number between -100 and 100 and calculate the square.
The calculate button on the left panel must be pressed before seeing any results in the right panel.
A number greater than 100 will yeild the result: 'Sorry, that number is too high. Are you trying to kill me?'
A number less than 100 will yeild the result: 'Sorry, that number is too low. You are trying to break me aren't you?.'
An entry of 1 or zero will yeild:  '1 squared is 1! Is that some kind of joke?! I can handle higher numbers, trust me!' AND 'Well, 0 squared is 0... come on! Give me something harder!' respectively.
Otherwise, the number will be squared and the result given.

ui.R

shinyUI( pageWithSidebar(
        headerPanel("The Super Awesome Number Squarer Shiny App!"),
        sidebarPanel(
                textInput('sqr', 'Please enter a number between -100 and 100', value = ""),
                h5('Please press the Calculate button after you have entered a number between -100 and 100'),
                actionButton("goButton", "Calculate!")
        ), 
        mainPanel(
                h2('Square and Cube any number between -100 and 100!'),
                h5('This app will square the number input in the sidebar to the left.'),
                h3('This is where the magic happens!'),
                h4('You entered:'),
                verbatimTextOutput("inputValue"),
                h4('Behold! Your number will now magically be squared'),
                verbatimTextOutput("outputValue"),
                h4('WOW! Isn\'t that amazing??')
                )
                )
)

Server.R

library(shiny)
sqcb <- function(input,output) {
        returnValue <- "Nothing entered yet."
        if (input > 100) {
                returnValue1 <- 'Sorry, that number is too high. Are you trying to kill me?'
        }
        else if (input < -100) {
                returnValue1 <- 'Sorry, that number is too low. You are trying to break me aren\'t you?.'
        }
        else if (input == 0){
                returnValue1 <-  'Well, 0 squared is 0... come on! Give me something harder!'
        }
        else if (input == 1){
        returnValue1 <-  '1 squared is 1! Is that some kind of joke?! I can handle higher numbers, trust me!'
        }
        else if (input > -101 & input < 101) {
                returnValue1 <- input^2
        }
        returnValue1
}
shinyServer( 
        function(input, output) {
                output$inputValue <- renderPrint({as.numeric(input$sqr)})
                output$outputValue <- renderText({
                        if (input$goButton == 0) "You have not pressed the calculate button!"
                        else if (input$goButton >= 1) sqcb(as.numeric(input$sqr), number)
                })
        }
)
