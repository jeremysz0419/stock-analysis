# stock-analysis
## Overview of Project
The purpose of this project was to refactor our code so that the runtime would be more efficient. While Steve liked our initial code, it might not be ideal for a large amount of information on stocks. Therefore, by refactoring our code, we can reduce our runtime and make our analysis more efficient and can accept a larger pool of data. 
## Results
### Refactored Code
In this analysis, we introduced the tickerIndex variable in our code to help guide the entire macro. With tickerIndex we can easily keep track and loop over each element for each stock. By keeping the tickerIndex element a consistent variable throughout our code, Steve can also follow our code logic much more easily. Additionally, by reducing the amount of code we can increase the speed in which the analysis is performed. Please see our refactored code below. 

    yearValue = InputBox("What year would you like to run the analysis on?")

    startTime = Timer
    
    'Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis").Activate
    
    Range("A1").Value = "All Stocks (" + yearValue + ")"
    
    'Create a header row
    Cells(3, 1).Value = "Ticker"
    Cells(3, 2).Value = "Total Daily Volume"
    Cells(3, 3).Value = "Return"

    'Initialize array of all tickers
    Dim tickers(12) As String
    
    tickers(0) = "AY"
    tickers(1) = "CSIQ"
    tickers(2) = "DQ"
    tickers(3) = "ENPH"
    tickers(4) = "FSLR"
    tickers(5) = "HASI"
    tickers(6) = "JKS"
    tickers(7) = "RUN"
    tickers(8) = "SEDG"
    tickers(9) = "SPWR"
    tickers(10) = "TERP"
    tickers(11) = "VSLR"
    
    'Activate data worksheet
    Worksheets(yearValue).Activate
    
    'Get the number of rows to loop over
    RowCount = Cells(Rows.Count, "A").End(xlUp).Row
    
    '1a) Create a ticker Index
    tickerIndex = 0

    '1b) Create three output arrays
    Dim tickerVolumes(12) As Long
    Dim tickerStartingPrices(12) As Single
    Dim tickerEndingPrices(12) As Single
    RowCount = Cells(Rows.Count, "A").End(xlUp).Row
    ''2a) Create a for loop to initialize the tickerVolumes to zero.
    For j = 0 To 11
        tickerVolumes(j) = 0
    Next j
    ''2b) Loop over all the rows in the spreadsheet.
        For i = 2 To RowCount
    
        '3a) Increase volume for current ticker
            tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 8).Value
        
        '3b) Check if the current row is the first row with the selected tickerIndex.
        'If  Then
             If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i - 1, 1).Value <> tickers(tickerIndex) Then
                tickerStartingPrices(tickerIndex) = Cells(i, 6).Value
            End If
            
            
        'End If
        
        '3c) check if the current row is the last row with the selected ticker
         'If the next rowâ€™s ticker doesnâ€™t match, increase the tickerIndex.
        'If  Then
            If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
                tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
            End If
            

            '3d Increase the tickerIndex.
            If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
                tickerIndex = tickerIndex + 1
            End If
            
        'End If
    
        Next i
   
    '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
    For i = 0 To 11
        
        Worksheets("All Stocks Analysis").Activate
        Cells(4 + i, 1).Value = tickers(i)
        Cells(4 + i, 2).Value = tickerVolumes(i)
        Cells(4 + i, 3).Value = tickerEndingPrices(i) / tickerStartingPrices(i) - 1
    Next i
    
    'Formatting
    Worksheets("All Stocks Analysis").Activate
    Range("A3:C3").Font.FontStyle = "Bold"
    Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
    Range("B4:B15").NumberFormat = "#,##0"
    Range("C4:C15").NumberFormat = "0.0%"
    Columns("B").AutoFit

    dataRowStart = 4
    dataRowEnd = 15

    For i = dataRowStart To dataRowEnd
        
        If Cells(i, 3) > 0 Then
            
            Cells(i, 3).Interior.Color = vbGreen
            
        Else
        
            Cells(i, 3).Interior.Color = vbRed
            
        End If
        
    Next i
 
    endTime = Timer
    MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

End Sub
### Data
The data provided by Steve included 12 tickers (stocks), the date of the stock price, the open/close prices of the stock, the high/low for that day, adjusted closing proce, and the volume for the day. Per Steve's instructions we were to create a new worksheet in which we find the total daily volume of each ticker (stock) and the net return in percentage. Furthermore, we were instructed to color code the return based on net positive or net negative gains. We also needed to separate our analysis based on the year which was inputted by the user. This was all performed on the code shown above. 

### Result
Based on our analysis, we can clearly see that Steve's stock profile for two years are drastically different. In 2017, with the exception of one ticker, Steve made quite a significant amount of gains in his returns. However, in 2018, we can see that most of stocks in his profile took a big dip and resulted in negative returns. Based on what we're seeing, this huge dip may be out of Steve's control and may be a result of an economic downturn in general. Therefore, diversifying his stock might not fix his profile as this negative return is too widespread. 

## Summary
### Refactoring in General
Refactoring our code can create two major advantages. The first is that we can increase the efficiency of our analysis by reducing the amount of clutter in our code. The second advantage is that in a professional setting when there are multiple individuals working on a project, we can reduce confusion and errors by using consistent variables throughout our code. By refactoring we reduce the amount of new variables introduced in our code and thus standardize our most important variables and functions. Should we need to use that function/code over and over again, we can just call it without typing out new code to clutter our project. 

## Refactored Project
After refactoring our code, we can clearly see the advantages via the runtime of our project. In our original script our runtimes for the year 2017 and 2018 were 0.65 and 0.64 seconds respectively. This runtime we reduced to around 0.1 seconds for both years in our new refactored code. While this difference may seem minute, the difference can become apparent when we are running thousands of tickers in our data. 
![2017 Runtime Refactored](https://github.com/jeremysz0419/stock-analysis/blob/main/Resources/2017%20Run%20Time.PNG)
![2018 Runtime Refactored](https://github.com/jeremysz0419/stock-analysis/blob/main/Resources/2018%20Run%20Time.PNG)
