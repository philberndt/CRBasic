# MenuPick (Create Selectable Menu Options)

The MenuPick instruction is used to create a list of selectable options that can be used when editing a MenuItem value.

## Syntax

MenuPick(Item1, Item2, Item3...)

### Example #1

The following program demonstrates the use of the custom menu functionality in the datalogger. The custom menu is named DataView; it will appear as the main menu in the datalogger. DataView has three submenus (PanelTemps, TCTemps, and the System menu) and one menu item (Counter) on the first level menu. Counter has a pick list of 4 values. Each submenu is displaying two values from the program. DisplayValue in the first submenu is displaying a value from a data table. All of the other values, which use MenuItem, are displaying current measurement values.

This program was written for aCR1000X, but other dataloggers can use similar code for custom menus (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
'Declare Variables
Public PTemp, Counter
Public TCTemp(2)

'Define DataTable TempMain Program
DataTable (Temp,1,1000)
DataInterval (0,60,Sec,10)
Sample (1,PTemp,FP2)
Sample (2,TCTemp(),FP2)
EndTable

'Define Custom Menus
DisplayMenu("DataView",-1)'Create Custom Menu named DataView; set as main menu
SubMenu ("PanelTemps")'Create Submenu named PanelTemps
MenuItem ("Scan",PTemp)'Item for Submenu - Scan
DisplayValue("Final Stg",Temp.PTemp(1,1))'Item for submenu - Final Stg
EndSubMenu'End of Submenu
MenuItem ("Counter",Counter)'Create menu item Counter
MenuPick(15,30,45,60)'Create a pick list for Counter
SubMenu ("TCTemps")'Create submenu TCTemps
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

**NOTE:** If using MenuPick to change values in a constant table, the constant must be in the constant table and the constant name must be the first parameter in MenuPick.

```
Public ApplyandRestart as Boolean

ConstTable
Const A = 0
Const B = 1
Const C = "Hello"
Const D = "World"
EndConstTable

DisplayMenu("Config",-3)
MenuItem ("A-Pick",A)'Create pick list for Constant A
MenuPick(A,0,5,10,15,20,30)
MenuItem ("B-Free",B)'Free form entry for Constant B
MenuPick(B)
MenuItem("C-Pick",C)'Create pick list for Constant C
MenuPick(C,One,Two,Three)
MenuItem ("D-Pick",D)'Free form entry for Constant D
MenuPick(D)
MenuRecompile("ApplyAndRestart",ApplyAndRestart)
MenuPick(TRUE,FALSE)
EndMenu

BeginProg
Scan (1,Sec,3,0)
NextScan
EndProg
```

## Remarks

If a pick list of options is desired for a MenuItem, the MenuPick instruction must immediately follow the MenuItem for which the list is being generated. Each item in the list is separated by a comma. The pick list is limited to a total of 512 characters. The Items in the pick list must be constants.

MenuPick can also be used to create a custom menu item with a pick list for changing values in a constant table. To change the value of a constant in a constant table, the constant must be in the constant table and the constant name must be the first parameter in MenuPick (see example program 2). Refer to [MenuRecompile](menurecompile.md) for additional information.

## Parameter

# MenuPick Item

Enter the items to be used for the pick list for a MenuItem. Each item is separated by a comma. The pick list is limited to a total of 512 characters.

Type: Constant
