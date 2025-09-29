# ApplyandRestartSequence/EndApplyandRestartSequence (Apply and Restart Sequence)

The ApplyandRestartSequence/EndApplyandRestartSequence is used to define code that should be run when the ApplyandRestart variable in the Constant table is set to true.

## Syntax

ApplyandRestartSequence

'Code to run after ApplyandRestart is set to True

EndApplyandRestartSequence

In the following example program, the scan interval for the program can be changed using the constant table named ScanConstTable. Code within the ApplyandRestartSequence is used to ensure the user-entered scan interval is greater than 0 prior to applying the change. If the user enters a value of 0 or less, the scan interval will be set to 1.

```
ConstTable (ScanConstTable,false)
Const ScanConst = 1
EndConstTable

ApplyAndRestartSequence
'Force ScanConst to be greater than 0
'If less than or equal to 0, default to 1
If ScanConstTable.ScanConst <= 0 Then
SetSetting("ScanConstTable.ScanConst",1)
EndIf

'After validation is complete, trigger
'logger to apply the Scan and restart.
SetSetting("ScanConstTable.ApplyAndRestart",1)
EndApplyAndRestartSequence

Public PTemp, batt_volt, counter As Long

'Define Data Tables.
DataTable (Test,1,9999)'Set table size to # of records, or -1 to autoallocate.
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,PTemp,FP2)
EndTable

'Main Program
BeginProg
Scan (ScanConst,Sec,0,0)
PanelTemp (PTemp,4000)
Battery (batt_volt)
counter = counter + 1
CallTable Test
NextScan
```

EndProg

```

## Remarks


The instructions contained within the ApplyandRestartSequence declaration will be run when the ApplyandRestart field of the ConstTable is set true by some means other than under program control (for example, using software or a keyboard display). The instructions within the declaration are run before the new values are set in the constant table and the program is recompiled.

The intent of the ApplyandRestartSequence is to allow a way for new values entered by a user into the ConstTable to be validated by the program before they are applied.

For additional information on setting up a constant table, see [ConstTable/EndConstTable (Declare Constants)](consttableendconsttable.md).
```
