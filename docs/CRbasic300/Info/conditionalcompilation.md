# Conditional Compilation

CRBasic allows the user to include or exclude code in the program, based on logger type or on some other condition in the datalogger program (such as a user defined constant). This is called conditional compilation, and it is accomplished using #If, #Else, #ElseIf, #EndIf, and #UnDef syntax.

The #If_no_remove declaration can be used in place of #If to specify conditional code that should not be removed from the resulting program when a **Conditional Compile and Save** is performed.

## Conditional Compilation Based on Datalogger Type (LoggerType)

Many CRBasic dataloggers have a default file extension that indicates the type of datalogger the program was compiled for (for example, \*.cr300for aCR300). Also, all dataloggers (except the CR200 and CR9000) will accept and compile a program with a \*.DLD extension, which allows the user to create a program that can be used in multiple datalogger types. In addition, all GRANITE dataloggers use a common \*.CRB extension. The \*`.CRB`extension is also valid for CR300 dataloggers, CR350 dataloggers, CR6 dataloggers with OS11 and greater, and CR1000X dataloggers with OS5 and greater.

By using conditional compilation with a \*.DLD or \*.CRB extension, the user can create a program that can be used in multiple datalogger types. Valid LoggerType constants for the \*.DLD extension are: CR1000, CR1000X, CR3000, CR800, CR300, CR6, CR5000, CR9000X, GRANITE6, GRANITE9, and GRANITE10 (conditional compilation is not available for the CR200 or the CR9000). The \*.CRB extension is also valid for CR300 dataloggers, CR350 dataloggers, CR6 dataloggers with OS11 and greater, and CR1000X dataloggers with OS5 and greater.

A common use for conditional compilation based on datalogger type is a program where the same program is to be run on datalogger types that have different options for a given CRBasic instruction. For example, many dataloggers have different voltage ranges, and Conditional If/Else statements can be used to define the range codes to be used for each datalogger type. This is best explained by reviewing the example program below:

```
Public PTemp, TCTemp

'Set the constant "Range" based on LoggerType
#IfLoggerType=CR300
Const Range=mv34
#ElseIfLoggerType=CR1000
Const Range=mv2_5C
#EndIf

DataTable (TempTab,True,-1)
DataInterval (0,1,Min,10)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)

'Range will bet set tomv34for aCR300or mv2_5c for a CR1000
TCDiff (TCTemp,1,Range,1,TypeT,PTemp,True ,0,4000,1.0,0)
CallTable (TempTab)
NextScan
EndProg
```

## Conditional Compilation Based on Evaluation of Constants

The following example program shows how conditional compilation can be used to include/exclude code that is compiled in the datalogger based on the state of constants in the program. This program also uses the Custom Menu capability.

Using a keyboard display, the user selects which sensors are connected to the datalogger. If a sensor is marked as true all the code associated with that sensor (measurements and data table information) is included in the program when compiled.

**NOTE:** The keyboard display is not available for theCR300.

```
Public Batt_Volt, Ptemp_C

'Constants for True/False
Const No = False
Const Yes = True

'Constant table for choosing Sensor types
'Set Values to no at the beginning of the program
ConstTable
Const Add107 = No
Const Addtc = No
Const Addcs700 = No
Const Addte525 = No
EndConstTable

'MenuRecompile var
Public Recomp

'Create the Custom Menu
DisplayMenu ("CR300Menu",-3)
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
MenuRecompile ("Compile Now", Recomp)
MenuPick (Yes, No)
EndSubMenu
EndMenu

DataTable (TempPrecip,True,1000)
DataInterval (0,1,Min,10)
Sample (1,Batt_Volt,FP2)

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
'Wiring panel & battery
PanelTemp (PTemp_C,4000)

Battery (Batt_Volt)

'Conditional code for temp sensors
#If Add107 Then
'107 Temperature Probe measurement T107_C
Therm107(Temp107,1,1,Vx1,0,4000,1,0)
#EndIf

#If Addtc Then
'Type T (copper-constantan) Thermocouple measurements Temp_C
TCDiff(TempTC,1,mv34,3,TypeT,Ptemp_C,True,0,4000,1,0)
#EndIf

'Conditional code for rain gauges
#If Addcs700 Then
'CS700 Rain Gauge measurement Rain_in
PulseCount(RaininCS700,1,P_LL,2,0,0.01,0)
#EndIf

#If Addte525 Then
'TE525/TE525WS Rain Gauge measurement Rain_in_2
PulseCount(RainIn525,1,P_LL,2,0,0.01,0)
#EndIf

CallTable (TempPrecip)
NextScan
EndProg
```

## Conditional Compilation Based on Presence of Constants

The following program shows how conditional compilation with #IfDef can be used to determine which Public variables are present in a program based on what constants are defined.

```
Public Ptemp, Batt_Volt

BeginProg
Scan (1,Sec,0,0)

PanelTemp (PTemp,4000)
Battery (batt_volt)
'If Const Final declared, then a Public variable named Testing is present
Const FINAL = 1
#IfDef FINAL Then
Public Testing
#Else
'If Const Final not declared, then a Public variable named Not_Testing is present
Public Not_Testing
#EndIf
NextScan
EndProg
```

## #UnDef

**NOTE:** Before using #UnDef, consider [IncludeSection()](../Instructions/includesection.md).

#UnDef is commonly used with [Const](../Instructions/const1.md) and #If/EndIf to create and stitch together libraries or sections of code located in [Include](../Instructions/include.md) files. This approach is most commonly used for systems with many components and measurements. These large system programs often use multiple Include files for specific system-component configurations. #If/#UnDef allows conditional declaration of program sections such that multiple Include files can be condensed into one. For example, in the sample code below, #If is used to declare a constant named Section twice in a main program. The first Section is used to pull in sensor settings from an Include file named "cpu:Sensor_PT500_Lib.crb", and the second Section is used to configure data storage using the same Include file. Without #UnDef, declaring the same constant twice would result in a compile error. Using #If along with #UnDef increases program efficiency and decreases the number of Include files required for system configuration.

```
'From Main program:
Const Section ="Section_PT500_Settings"
Include"cpu:Sensor_PT500_Lib.crb"
#UnDefSection

Const Section ="Section_PT500_Storage"
Include"cpu:Sensor_PT500_Lib.crb"
#UnDefSection

'///////////////////////////////////////////////////////////////////////

'From an Include file named "cpu:Sensor_PT500_Lib.crb"
#If Section = "Section_PT500_Settings"
Const AIR_GRASS_TEMP = True
Const GROUND_TEMP = False
#EndIf

#If Section = "Section_PT500_Storage"
DataTable(pt500_12sec, True, -1 )
TableFile("CRD:pt500_12sec", 8, -1, 0, 1, Day, outstat, lastfilename)
DataInterval(0, 12, Sec, 10)
Sample(1, pt500_air, IEEE4, False)
Sample(1, pt500_grass, IEEE4, False)
Sample(1, pt500_t1, IEEE4, False)
EndTable
#EndIf
```
