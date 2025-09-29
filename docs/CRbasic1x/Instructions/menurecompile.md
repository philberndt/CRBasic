# MenuRecompile (Create Custom Menu for Recompiling Program)

The MenuRecompile instruction is used to create a custom menu item for recompiling a program after making changes to one or more Constant Table values.

## Syntax

MenuRecompile("CompileString",CompileVar)

MenuPick (No, Yes)

(where No has been declared as False, and True has been declared as Yes elsewhere in the program)

The following program creates a custom menu that allows a user to choose what sensors will be measured using a keyboard display, and then recompile the program to put the changes into effect. Conditional compiling is used so that only the code needed for the selected sensors will be added to the program.

The menu that is created looks similar to the layout below.

Main menu:

Custom Logger Menu

Compile Msg:

Sensor Setup

Save & Compile

Sensor Setup menu:

Add Temp107?

Add TCTemp?

Add CS700?

Add TE515?

Save & Compile menu:

Compile Now

The Sensor Setup and Compile Now menu options have Yes/No selections.

```
Public Batt_Volt, Ptemp_C

'Constants for True/False
Const No = False
Const Yes = True

'Constant table for choosing Sensor types
ConstTable
Const Add107 = No
Const Addtc = No
Const Addcs700 = No
Const Addte525 = No
EndConstTable

'MenuRecompile var
Public Recomp

DisplayMenu ("Custom Logger Menu",-3)
DisplayValue ("Compile Msg",status.compileresults)
SubMenu ("Sensor Setup")
MenuItem ("Add Temp107?",Add107)
MenuPick (Add107, Yes, No)
MenuItem ("Add TCTemp?",Addtc)
MenuPick (Addtc, Yes, No)
MenuItem ("Add CS700?",Addcs700)
MenuPick (Addcs700, Yes, No)
MenuItem ("Add TE525?",Addte525)
MenuPick (Addte525, Yes, No)
EndSubMenu
SubMenu ("Save & Compile")
MenuRecompile("Compile Now", Recomp)
MenuPick (Yes, No)
EndSubMenu
EndMenu

DataTable (TempPrecip,True,1000)
DataInterval (0,1,Min,10)
Sample (1,Batt_Volt,FP2)

'Conditionally added data table values
#If Add107 Then
Average (1,Temp107,FP2,False)
#EndIf

#If Addtc Then
Average (1,TempTC,FP2,False)
#EndIf

#If Addte525 Then
Totalize (1,RainIn525,FP2,False)
#EndIf

#If Addcs700 Then
Totalize (1,RaininCS700,FP2,False)
#EndIf

EndTable

BeginProg
'Conditionally added Measurement vars
#If Add107 Then
Public Temp107
#EndIf
#If Addtc Then
Public TempTC
#EndIf
#If Addte525 Then
Public RainIn525
#EndIf
#If Addcs700 Then
Public RaininCS700
#EndIf
Scan (1,Sec,3,0)
'battery

Battery (Batt_Volt)

'Conditional code for temp sensors
#If Add107 Then
'107 Temperature Probe measurement
Therm107(Temp107,1,1,Vx3,0,15000,1,0)

#EndIf

#If Addtc Then
'Type T (copper-constantan) Thermocouple measurements
PanelTemp (PTemp_C,15000)

TCDiff(TempTC,1,mv200C,5,TypeT,Ptemp_C,True,0,15000,1,0)

#EndIf

'Conditional code for rain gauges
#If Addcs700 Then
'CS700 Rain Gauge measurement
PulseCount(RaininCS700,1,C1,2,0,0.01,0)
#EndIf

#If Addte525 Then
'TE525/TE525WS Rain Gauge measurement
PulseCount(RainIn525,1,C2,2,0,0.01,0)
#EndIf

CallTable (TempPrecip)
NextScan

EndProg
```

## Remarks

The MenuRecompile instruction must appear within a custom menu declaration (DisplayMenu/EndMenu). It is used along with the MenuItem and MenuPick instructions to create a custom menu item with a pick list for changing values in a constant table ([ConstTable/EndConstTable](consttableendconsttable.md)). Typically a user must enter a value manually for a constant table value. The pick list provides a more convenient and less error-prone way to enter new values.

When used in this capacity, MenuItem's MenuVariable parameter should be the name of the constant defined in the constant table for which a pick list will be created. MenuPick's first Item should be the name of the constant, as well. MenuRecompile, followed by MenuPick, is then used to save the changes to the constant table and recompile.

## Parameters

# CompileString (Menu Name)

A line of text that will be used for the recompile menu item in the custom menu. Up to 11 characters will be displayed for the menu item in the custom menu, and up to 21 characters will be displayed as a header for recompile yes or no pick list.

Type: String

# CompileVar (Compile Variable)

A Boolean variable that is evaluated to determine whether or not the program should be compiled. This variable is set when Yes or No is selected from the recompile menu, or it can be set by some other means in the program.

Type: Boolean variable
