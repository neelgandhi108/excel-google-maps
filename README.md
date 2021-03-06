# excel-google-maps
> I choose a lazy person to do a hard job. Because a lazy person will find an easy way to do it.
> - Bill Gates

I must admit that I am a lazy person and I do not like to do things that are constantly being repeated. I like to learn new things and I like to create things in Node JS. Sometimes in my job I need to find the travel distance between two addresses, for example when I need to calculate travel costs in the bid. And I find it unnecessary to enter addresses into Google Maps and then rewrite the distance back to Excel.

This code uses Express to create an endpoint. Function written in the VBA calls this endpoint to get distance between two addresses. The function is embedded in Excel into a module to write a simple function into the cell. So for example I can write expressions with distance results.

![Example usage in Excel](https://raw.githubusercontent.com/LabZoneSK/excel-google-maps/master/example/Excel-Sample.jpg)

You can also take this mini project as a simple mental exercise and learning session that connects multiple technologies such as Node JS, Express, and Visual Basic (VBA).

*BONUS - Running on Heroku :-)*

## Pre-requisitions
* Node and NPM must be installed on your machine
* Have account on [Heroku](https://www.heroku.com) (optional) 

## Installation
Clone or download source code from GitHub and run:
`npm install`

## Usage
Take a look on attached XSL with macro support or write function as module to your worksheet:
```vba
Public Function findDistance(ByVal Origin As String, _
                         ByVal Destination As String) As Double
    ' Return the distance
    Dim objHTTP As New WinHttp.WinHttpRequest
    URL = "https://excel-google-maps.herokuapp.com/api/v1/distance/" & Origin & "/" & Destination
    objHTTP.Open "GET", URL, False
    objHTTP.setRequestHeader "Content-Type", "application/json"
    objHTTP.send
    
    ' Conversion required for my regional settings. If you not require do not replace comma and dots
    findDistance = Replace(CStr(objHTTP.ResponseText), ".", ",")

End Function
```

Then you can use function in expression in a cell. For example:
`=ROUNDUP(findDistance(B2;C2);0)`
(calculates distance between two addresses (in cells B2 and C2) and round up number without decimals)

*DISCLAIMER - I am not VBA expert. I know that VBA expert can write this purely in VBA without need for Express side. Please, do not blame me for my weakness. :-)*