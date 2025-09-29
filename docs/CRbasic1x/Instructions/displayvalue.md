# DisplayValue

The DisplayValue instruction is used to define the name and associated data table value or variable for an item in a custom menu.

## Syntax

DisplayValue("MenuItemName", MenuExpression)

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

The MenuItemName parameter is the name that will appear on the custom menu. The name should be enclosed in quotation marks. The Expression parameter defines the value from a data table (tablename.fieldname) or variable to be displayed. When displaying a variable from the public table, use the variable name only ("Public" is not needed). Values displayed using DisplayValue cannot be edited.

**NOTE:** Use MenuItem to display editable variables in a custom menu.

## Parameters

# MenuItemName

The name that will be displayed on the custom menu for a measurement or datatable value. The name is limited to 512 characters, including characters for the associated measurement value. However, its practical size is much less because of the small size of the keyboard display. MenuItemName should be enclosed in quotation marks.

Type: Text, Variable, or Constant

# MenuExpression

Defines the value from a data table (tablename.fieldname) or variable to be displayed, or any valid expression. When displaying a variable, the table name (public) is not included.

Type: DataTable.Fieldname, Variable, or Expression
