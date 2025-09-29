# ConstTable/EndConstTable (Declare Constants)

The ConstTable/EndConstTable instructions are used to declare one or more constants that can be changed using software or the C command in terminal mode. The program is then recompiled with the new values.

## Syntax

```
ConstTable
```

( TableName, Hidden )'TableName and Hidden are optional parameters

```
'declare constants
```

Const A =_value _

Const B =_ value_

```
EndConstTable
```

With the following program loaded into the datalogger, a Constant Table menu item appears under the **Configure, Settings** menu and is visible using datalogger support software. The table contains three values that can be edited: A, B, and C.

When loaded into the datalogger, this program uses Option 2 as described in the help. Options 1 and 3 are commented out, but shown for reference purposes.

```
Public PTemp, SettableA, SettableB, SettableC

'ConstTable 'Option 1; No parameters
ConstTable(NewConstTable,0)'Option 2, Table is created and displayed by software
'ConstTable (HiddenTable, 1) 'Option 3; Table called HiddenTable is created; made visible in software by datalogger security
Const A = 1
Const B = 10
Const C = 100
EndConstTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)
SettableA=A
SettableB=B
SettableC=C
NextScan
EndProg
```

## Remarks

The ConstTable declaration should appear in the declarations section of the program, prior to the start of the main program. The intent of this declaration is to define one or more constants in the program that will be listed in a special table in the datalogger. The constants in this special table can be edited and the program recompiled to use the new values. In some instances, recompiling the program in this manner will reset the data tables in the datalogger, so all data should be collected before editing a value in the constant table.

The ConstTable provides a way to have a changeable value in an instruction parameter that requires a constant (for instance, the interval for the Scan instruction will not accept a variable). For users who are familiar with CR10X, CR23X, and CR510 dataloggers, the ConstTable is similar to the \*4 Table functionality.

There are three different options that can be used to set up a constant table using this instruction.

**_Option 1, no parameters_**

```
ConstTable
```

'define constants

```
EndConstTable
```

This option sets up a table in the datalogger s menu system named Constant Table. The menu system is accessed using thedatalogger s terminal emulator. The constants in the constant table can be edited by entering a **C** at the datalogger prompt.

**_Option 2, table name_**

```
ConstTable
```

( TableName )'where TableName is the name you want to assign

```
'define constants
```

EndConstTable

```
This option sets up a constant table in the datalogger and enables the table to be displayed and edited in the numeric monitor or table monitor found in datalogger support software. Set ApplyAndRestart to**True**to effect the changes. The constant table can also be edited using terminal emulation mode. Note that TableName cannot be any of the predefined table names in the datalogger; for example, Public, Status, Settings, or DataTableInfo.

***Option 3, hidden custom table***

```

ConstTable

```
( TableName, 1 )'where TableName is the name you want to assign

```

'define constants

```
EndConstTable
```

This option sets up a constant table in the datalogger and enables the table to be displayed and edited in the numeric monitor or table monitor found in datalogger support software. However, in order for the table to be displayed in software, the user must connect to the datalogger with the highest level of security. Set ApplyAndRestart to **True** to effect the changes. The constant table can also be edited using terminal emulation mode. Note that TableName cannot be any of the predefined table names in the datalogger; for example, Public, Status, Settings, or DataTableInfo.

## Usage notes

When a constant table is edited in the datalogger, the datalogger must first make a copy of the program in memory. Thus, there must be enough available memory to hold two copies of the program until the new one is compiled. If the datalogger s memory is too small, the operation will fail. This can occur with large programs, with smaller memory dataloggers, or if too many files are stored on the datalogger's CPU drive. In the case of too many files, delete unnecessary files to free up memory.
