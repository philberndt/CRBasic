# WebPageBegin/WebPageEnd (Declare a Web Page)

The WebPageBegin/WebPageEnd instructions are used to declare a web page that will be displayed when a request for the defined HTML page comes from an external source.

## Syntax

WebPageBegin(WebPageName,WebPageCmd)

HTTPOut ( "<p>html string to output" + variable + "additional string to output</p>" )

HTTPOut ( "<p>html string to output" + variable + "additional string to output</p>" )

WebPageEnd

The following example program creates two web pages: one named "default.html" and the other named "datatables.html". "Default.html" is the first page displayed by the datalogger when it is connected to with the browser. "Datatables.html" is accessed by a link from the "default.html" page.

"Datatables.html" is actually the code generated automatically by the datalogger if no "default.html" file is found. It is used in this example to show how the Commands function can be used in a web page.

**NOTE:** In this example, some of the lines of HTML code in the HTTPOut instructions may display as two or more lines in the help window. Each HTTPOut instruction should be a single line in the datalogger program.

This program was written for aCR1000X, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
Dim Commands As String * 200
Public Temp, Time(9), RefTemp

DataTable (CR1000XTemp,True,-1)
DataInterval (0,1,Min,10)
Sample (1,Temp,FP2)
Average (1,Temp,FP2,False)
EndTable

WebPageBegin("default.html",Commands)
HTTPOut("<!DOCTYPE HTML PUBLIC " + CHR(34) + "-//W3C//DTD HTML 4.0 Transitional//EN" + CHR(34)+ ">")
HTTPOut("<html>")
HTTPOut("<head>")
HTTPOut("<title>MyCR1000XDatalogger</title>")
HTTPOut("<meta http-equiv=" + CHR(34) +"refresh"+ CHR(34) + "content="+ CHR(34) +"60"+ CHR(34)+ ">")
HTTPOut("</head>")
HTTPOut("<body>")
HTTPOut("<img src=" + CHR(34) + "/CPU/SHIELDWEB.jpg" + CHR(34)+ ">")
HTTPOut("<h1>MyCR1000XDatalogger</h1>")
HTTPOut("The time is " + time(4) + ":" + time(5))
HTTPOut("<br>")
HTTPOut("<br>")
HTTPOut("The temperature is " + Temp)
HTTPOut("<br>")
HTTPOut("<br>")
HTTPOut("<a href="+ CHR(34) + "datatables.html" + CHR(34) + ">Go to the Data Tables Link Page</a>")
HTTPOut("</body>")
HTTPOut("</html>")
WebPageEnd

WebPageBegin("datatables.html",Commands)
HTTPOut("<!DOCTYPE HTML PUBLIC " + CHR(34) + "-//W3C//DTD HTML 4.0 Transitional//EN" + CHR(34)+ ">")
HTTPOut("<HTML>")
HTTPOut("<HEAD>")
HTTPOut("<TITLE>CR1000XData Table Links</TITLE>")
HTTPOut("</HEAD>")
HTTPOut("<BODY>")
HTTPOut("<h1>CR1000XDatalogger Data Table Links</h1>")
HTTPOut("<ul><li><a href="+ CHR(34) + "command=NewestRecord&table=Status"+ CHR(34) + ">Newest Record from Status</a></li></ul>")
HTTPOut("<ul><li><a href="+ CHR(34) + "command=NewestRecord&table=CR1000XTemp"+ CHR(34) + ">Newest Record fromCR1000XTemp</a></li></ul>")
HTTPOut("<ul><li><a href="+ CHR(34) + "command=TableDisplay&table=CR1000XTemp&records=1"+ CHR(34) + ">Display Last 1 Records from DataTableCR1000XTemp</a></li></ul>")
HTTPOut("<ul><li><a href="+ CHR(34) + "command=NewestRecord&table=Public"+ CHR(34) + ">Newest Record from Public</a></li></ul>")
HTTPOut("</BODY>")
HTTPOut("</HTML>")
WebPageEnd

BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,15000)
TCDiff (Temp,1,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

RealTime (Time())
CallTable (CR1000XTemp)
NextScan
EndProg
```

## Remarks

The WebPageBegin/WebPageEnd instructions enclose the [HTTPOut](httpout.md) instructions that comprise the text of the web page. The web page declaration must appear before the BeginProg instruction. Multiple web page declarations can be defined in the program.

When the WebPageName is requested from the datalogger, the datalogger will provide an HTML page comprised of the code in the HTTPOut instructions. This method allows a way for the datalogger to provide the most recent values for variables in an HTML format.

## Parameters

# WebPageName (Web Page Name)

The name for the HTML page created by the datalogger. The HTML file name must be enclosed in quotes. If this parameter is set to "default.html", the datalogger will replace the page it displays by default with the one defined in the web page declaration.

Type: String enclosed in quotes

# WebPageCmd (Incoming Web Command)

A string that holds the incoming command from the web browser, which the datalogger will subsequently parse. The string needs to be sized large enough to accommodate any command that might be received.

Commands can be passed from the web page to the datalogger, which the datalogger will then parse and act upon (all commands after the "?" are parsed). Commands that are supported natively by the operating system are:

| Command       | Syntax                                                                          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ------------- | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| HomePage      | command = homepage                                                              | Goes to the datalogger s home page. This can be used by bypass the default.htm that replaces the datalogger s home page, if a default has been created.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| SetValue      | command = SetValue&table = _tablename _&field = _ fieldname _&value = _ value _ | Sets a value in the Public or Status table.* Tablename *is the name of the table in the datalogger (Public or Status),* fieldname *is the variable in the table to set, and* value *is the value to set the variable to. To restrict access to setting values in the datalogger, SetValue must be used in conjunction with a user account with Read/Write access.                                                                                                                                                                                                                                                                                        |
| NewestRecord  | command = NewestRecord&table = _ tablename _                                    | Retrieves the most recent record from a table in the datalogger.* Tablename *is the name of the table from which to retrieve the record.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| TableDisplay  | command = TableDisplay&table = _ tablename _&records = _ NumberRecords _        | Displays the most recent X number of records from a data table.* Tablename *is the name of the table from which to display the records, and* NumberRecords *is an integer indicating the number of records to display.                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Units         | command = _ tablename _&units = true                                            | Displays the units, if defined, for the variables in a table.* Tablename *is the name of the table for which units should be displayed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| File          | command = File&file = _ FileName _                                              | Displays a file.* Filename *is the file to be displayed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| File Run      | command = File&File = _ FileName _&command = StartProgram                       | Runs a program file loaded in the datalogger.* Filename *is the name of the program to run.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| PakBusAddress | command = PakBusAddress                                                         | Returns the next PakBus address, equal to or greater than 100, that is not contained in the datalogger s routing table or neighbor list.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Security      | security = _ securitycode _                                                     | Unlocks security set in the datalogger.* Securitycode *is the code for the security level in the datalogger that you want to unlock (for example, http://192.168.1.23/security = 12345). If security is enabled in the datalogger, at least the lowest level of security (level 3) must be unlocked to access a web page. Level 2 must be unlocked to set a variable in the Public table, and level 1 just be unlocked to set a value in the Status table. The datalogger is also capable of basic web authentication using the datalogger Account Manager. This is the preferred method of securing the datalogger when it is acting as an HTTP server. |
| StyleSheet    | command = stylesheet = _ filename.css _                                         | Applies a cascading style sheet to NewestRecord and TableDisplay.* Filename.css*is the name of the style sheet to use.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

Note that these commands also can be used directly in a URL to the datalogger without having a web page in the datalogger. For example, the following typed into a URL will set a variable in the datalogger:

http://192.168.4.14/?command = SetValue&table = public&field = SetPoint&value = 70.2

Type: String Variable
