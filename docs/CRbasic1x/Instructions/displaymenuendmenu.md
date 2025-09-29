# DisplayMenu, EndMenu

The DisplayMenu/EndMenu instructions are used to mark the beginning and ending of a custom menu.

## Syntax

DisplayMenu("MenuName",AddToSystem,Cursor[optional] )

menu definition

EndMenu

### Example #1

The following program demonstrates the use of the custom menu functionality in the datalogger. The custom menu is named DataView; it will appear as the main menu in the datalogger. DataView has three submenus (PanelTemps, TCTemps, and the System menu) and one menu item (Counter) on the first level menu. Counter has a pick list of 4 values. Each submenu is displaying two values from the program. DisplayValue in the first submenu is displaying a value from a data table. All of the other values, which use MenuItem, are displaying current measurement values.

This program was written for aCR1000X, but other dataloggers can use similar code for custom menus (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
'Declare Variables
Public PTemp, Counter
Public TCTemp(2)

'Define DataTable Temp
DataTable (Temp,1,1000)
DataInterval (0,60,Sec,10)
Sample (1,PTemp,FP2)
Sample (2,TCTemp(),FP2)
EndTable

'Define Custom Menus
DisplayMenu("DataView",-1)'Create Custom Menu named DataView; set as main menu
SubMenu("PanelTemps")'Create Submenu named PanelTemps
MenuItem("Scan",PTemp)'Item for Submenu - Scan
DisplayValue("Final Stg",Temp.PTemp(1,1))'Item for submenu - Final Stg
EndSubMenu'End of Submenu
MenuItem("Counter",Counter)'Create menu item Counter
MenuPick(15,30,45,60)'Create a pick list for Counter
SubMenu("TCTemps")'Create submenu TCTemps
MenuItem("TC Temp 1",TCTemp(1))'Item for Submenu - TCTemps(1)
MenuItem("TC Temp 2",TCTemp(2))'Item for Submenu - TCTemps(2)
EndSubMenu'End of Submenu
EndMenu'End custom menu creation

'Main Program
BeginProg
Scan (1,Sec,3,0)
PanelTemp (PTemp,15000)
TCDiff (TCTemp(),2,mv200C,1,TypeT,PTemp,True,0,15000,1.0,0)

Counter=Counter-1
If Counter <=0 Then
Counter=0
EndIf
CallTable Temp
NextScan
EndProg
```

### Example #2

This example demonstrates how to create a custom menu to edit values in a Constant table, using MenuItem and MenuPick.

```
Public ApplyAndRestart As Boolean

ConstTable
Const A = 0
Const B = 1
Const C = "Hello"
Const D = "World"
EndConstTable

DisplayMenu("Config",-3)
MenuItem("A-Pick",A)'Create a pick list for Constant A
MenuPick(A,0,5,10,15,20,30)
MenuItem("B-Free",B)'Free form entry for Constant B
MenuPick(B)
MenuItem("C-Pick",C)'Create pick list for Constant C
MenuPick(C,One,Two,Three)
MenuItem("D-Pick",D)'Free form entry for Constant D
MenuPick(D)
MenuRecompile("ApplyAndRestart",ApplyAndRestart)
MenuPick(TRUE,FALSE)
EndMenu

BeginProg
Scan(1,Sec,3,0)
NextScan
EndProg
```

## Remarks

The DisplayMenu instruction marks the beginning of a custom menu. EndMenu marks the end of the custom menu definition. The [MenuItem](menuitem.md) and [DisplayValu](displayline.md) e instructions are used to define the values that will be displayed in the custom menu.

## Parameters

# MenuName

Used to specify the text that will be shown on the datalogger's display for the menu. The string is limited to 20 characters, and it should be enclosed in quotation marks.

Type: Text

# AddToSystem

Determines how the newly created menu will be displayed in the datalogger's menuing system:

| Option | Description                                                                                                                                                           |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0      | Upon power up, a starting window is displayed. When a key is pressed, the system menu will be displayed. The custom menu will appear as a submenu to the system menu  |
| -1     | Upon power up, a starting window is displayed. When a key is pressed, the custom menu will be displayed. The system menu will appear as a submenu to the custom menu. |
| -2     | Upon power up, the custom menu is displayed. The system menu will appear as a submenu to the custom menu.                                                             |
| -3     | Upon power up, the custom menu is displayed. The system menu is hidden from the user.                                                                                 |
| -4     | Upon power up, the custom menu is displayed. All of the system menu, except for Display Settings, is hidden from the user.                                            |

Type: Constant

## Optional Parameter

# Cursor

A constant that is used to position the cursor on a specific line (1 through 7) when the menu is entered.

Type: Constant
