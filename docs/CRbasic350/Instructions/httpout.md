# HTTPOut (HTTP Output)

The HTTPOut instruction is used to define a line of HTML code to be used in a datalogger generated HTML file.

## Syntax

WebPageBegin ("WebPageName", WebPageCmd )

HTTPOut( "<p>html string to output" + variable + "additional string to output</p>" )

HTTPOut( "<p>html string to output" + variable + "additional string to output</p>" )

WebPageEnd

The following example program creates two web pages: one named "default.html" and the other named "datatables.html". "Default.html" is the first page displayed by the datalogger when it is connected to with the browser. "Datatables.html" is accessed by a link from the "default.html" page.

"Datatables.html" is actually the code generated automatically by the datalogger if no "default.html" file is found. It is used in this example to show how the Commands function can be used in a web page.

**NOTE:** In this example, some of the lines of HTML code in the HTTPOut instructions may display as two or more lines in the help window. Each HTTPOut instruction should be a single line in the datalogger program.

This program was written for aCR350, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
Dim Commands As String * 200
Public Temp, Time(9), RefTemp

DataTable (CR350Temp,True,-1)
DataInterval (0,1,Min,10)
Sample (1,Temp,FP2)
Average (1,Temp,FP2,False)
EndTable

WebPageBegin("default.html",Commands)
HTTPOut("<!DOCTYPE HTML PUBLIC " + CHR(34) + "-//W3C//DTD HTML 4.0 Transitional//EN" + CHR(34)+ ">")
HTTPOut("<html>")
HTTPOut("<head>")
HTTPOut("<title>CR350Datalogger</title>")
HTTPOut("<meta http-equiv=" + CHR(34) +"refresh"+ CHR(34) + "content="+ CHR(34) +"60"+ CHR(34)+ ">")
HTTPOut("</head>")
HTTPOut("<body>")
HTTPOut("<img src=" + CHR(34) + "/CPU/SHIELDWEB.jpg" + CHR(34)+ ">")
HTTPOut("<h1>CR350Datalogger</h1>")
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
HTTPOut("<TITLE>CR350Data Table Links</TITLE>")
HTTPOut("</HEAD>")
HTTPOut("<BODY>")
HTTPOut("<h1>CR350Datalogger Data Table Links</h1>")
HTTPOut("<ul><li><a href="+ CHR(34) + "command=NewestRecord&table=Status"+ CHR(34) + ">Newest Record from Status</a></li></ul>")
HTTPOut("<ul><li><a href="+ CHR(34) + "command=NewestRecord&table=CR350Temp"+ CHR(34) + ">Newest Record fromCR350Temp</a></li></ul>")
HTTPOut("<ul><li><a href="+ CHR(34) + "command=TableDisplay&table=CR350Temp&records=1"+ CHR(34) + ">Display Last 1 Records from DataTableCR350Temp</a></li></ul>")
HTTPOut("<ul><li><a href="+ CHR(34) + "command=NewestRecord&table=Public"+ CHR(34) + ">Newest Record from Public</a></li></ul>")
HTTPOut("</BODY>")
HTTPOut("</HTML>")
WebPageEnd

BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (Temp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

RealTime (Time())
CallTable (CR350Temp)
NextScan
EndProg
```

## Remarks

HTTPOut instructions must be enclosed in a WebPageBegin/WebPageEnd declaration. HTML tags and text strings must be enclosed in quotes. If a quote is required in the HTML code, use [CHR](chr.md)(34), or it is interpreted by the editor as the beginning or ending of a string. If a single quote is required use CHR(39), since single quotes are used in the editor to denote comments. Variables are referenced by name, and if added within strings are appended to the text using the + sign (for example, "some text string" + variablename + "more text"). If a web pages references a file stored on the datalogger, use /path/ to denote the datalogger drive on which the file is stored (i.e., /CPU/file.jpg).
