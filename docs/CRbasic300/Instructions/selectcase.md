# Select Case (Select Case Statement)

The Select Case instruction is used to execute one of several statement blocks depending on the value of anexpression.

## Syntax

Select CaseTestExpression

CaseExpressionList1

[StatementBlock-1]

CaseExpressionList2

[StatementBlock-2]

```
Case Else
```

[StatementBlock-n]

Case Iscomparative expression

[StatementBlock-n]

Expression

EndSelect

### Example #1

The example uses Select Case to display a message, based upon the value of a thermocouple.

```
Public RefTemp, TCTemp
Public Message As String * 30

BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

Select CaseTCTemp
Case0 to 20
Message = "It is cold"
Case20.1 to 23
Message = "It is cool"
Case23.1 to 25
Message = "It is comfortable"
Case25.1 to 27
Message = "It is warm"
Case27.1 to 35
Message = "It is hot"
Case Is> 35
Message = "Possible Temperature error"
Case Else
Message = "Run for the hills!"
EndSelect

NextScan
EndProg
```

### Example #2

The following program example shows the use of Case statements to return wind direction as an alphabetical representation (N, NNE, NE, ENE, E, ESE, SE, SSE, S, SSW, SW, WSW, W WNW, NW, NNW).

```
'Declare Variables and Units
Public BattV'external battery voltage
Public WS_ms'RM Young 05103
Public WindDir
Public WDtxt As String * 5'wind direction classified into 16 bins, centered on cardinal directions

Units BattV=Volts
Units WS_ms=meters/second
Units WindDir=degrees

'Define Data Tables
DataTable(FftnMin,True,-1)
DataInterval(0,15,Min,10)
WindVector (1,WS_ms,WindDir,FP2,False,180,0,0)
FieldNames("WS_ms_S_WVT,WindDir_D1_WVT,WindDir_SD1_WVT")
EndTable

DataTable(Daily,True,-1)
DataInterval(0,1440,Min,10)
Minimum(1,BattV,FP2,False,False)
EndTable

'Main Program
BeginProg
'Main Scan
Scan(1,Sec,1,0)
'Datalogger Battery Voltage measurement 'BattV'
Battery(BattV)

'05103 Wind Speed & Direction Sensor measurements 'WS_ms' and 'WindDir'
PulseCount(WS_ms,1,P_LL,1,1,0.098,0)
BrHalf(WindDir,1,mv2500,3,Vx1,1,2500,True,0,4000,355,0)

If WindDir>=360 Then WindDir=0

'classify wind direction into 16 bins
Select Case WindDir
Case Is >=0 AND Is <=11.25
WDtxt = "N"
Case Is >11.25 AND Is <=33.75
WDtxt = "NNE"
Case Is >33.75 AND Is <=56.25
WDtxt = "NE"
Case Is >56.25 AND Is <=78.75
WDtxt = "ENE"
Case Is >78.75 AND Is <=101.25
WDtxt = "E"
Case Is >101.25 AND Is <=123.75
WDtxt = "ESE"
Case Is >123.75 AND Is <=146.25
WDtxt = "SE"
Case Is >146.25 AND Is <=168.75
WDtxt = "SSE"
Case Is >168.75 AND Is <=191.25
WDtxt = "S"
Case Is >191.25 AND Is <=213.75
WDtxt = "SSW"
Case Is >213.75 AND Is <=236.25
WDtxt = "SW"
Case Is >236.25 AND Is <=258.75
WDtxt = "WSW"
Case Is >258.75 AND Is <=281.25
WDtxt = "W"
Case Is >281.25 AND Is <=303.75
WDtxt = "WNW"
Case Is >303.75 AND Is <=326.25
WDtxt = "NW"
Case Is >326.25 AND Is <=348.75
WDtxt = "NNW"
Case Is >348.75 AND Is <=360
WDtxt = "N"
EndSelect

'Call Data Tables and Store Data
CallTable(FftnMin)
CallTable(Daily)
NextScan
EndProg
```

## Remarks

Multiple expressions or ranges can be used in each Case clause (i.e., Case 1 To 4, 7 To 9, 11, 13). Select Case statements can be nested.

## Parameters

# Select Case

The Select Case statement begins the Select Case decision control structure. It must appear before any other part of the Select Case structure.

# TestExpression

The TestExpression is any numeric or string expression. If TestExpression matches the ExpressionList associated with a Case clause, the StatementBlock following that Case clause is executed up to the next Case clause, or for the final one, up to the EndSelect. Control then passes to the statement following EndSelect. If TestExpression matches more than one Case clause, only the statements following the first match are executed.

# Case

The Case statement sets apart a group of program statements to be executed if an expression in the ExpressionList matches TestExpression.

# ExpressionList

The ExpressionList consists of a comma-delimited list of one or more of the following forms.

- Expression (for example, Case 99).

- Expression To Expression (for example, Case 1 to 20).

- Is compare-operator Expression (for example, Case Is > 99, Case Is = 10).

- StatementBlock.

- Elements StatementBlock-1 to StatementBlock-n consist of any number of program statements on one or more lines.

Where Expression is any numeric expression and To is a keyword used to specify a range of values. If you use the To keyword to indicate a range of values, the smaller value must precede To.

# Case Else

The Case Else statement is a keyword indicating the StatementBlock to be executed if no match is found between the TestExpression and an ExpressionList in any of the other Case selections. When there is no Case Else statement and no expression listed in the Case clauses matches TestExpression, program execution continues at the statement following EndSelect. The Case Else statement is optional, but it is a good idea to include the statement to handle unforeseen TestExpression values.

# Case Is

The Case Is statement is a keyword that is used before a comparison operator (=, <>, <, <=, >, or >=). If the Is keyword is omitted (for example, Case < 10), it is implied.

# Expression

A single expression can be entered to indicate a case of Case Is = Expression.

# EndSelect

The EndSelect statement marks the end of the Select Case control structure. It must appear after all other statements in the Select Case control structure.
