# Commenting Code in CRBasic

| It is often useful to provide comments in your datalogger program so that when you review the program at a later date, you will know what each section of code does. Comments can be inserted into the program by preceding the text with a single quote. When the program is compiled, the datalogger compiler will ignore any text that is preceded by a single quote. A comment can be placed at the beginning of a line or it can be placed after program code. If Syntax Highlighting is enabled (**View | Editor Preferences | Syntax Highlighting**), commented text will appear formatted differently than other lines of code. |

```
'CR300
```

'The following program is used to measure

```
'4 thermocouples
```

'VARIABLE DECLARATION

```
DimTCTemp(4)'Dimension TC measurement variable
```

AliasTCTemp(1)=EngineCoolantT'Rename variables

```
AliasTCTemp(2)=BrakeFluidT
```

AliasTCTemp(3)=ManifoldT

```
AliasTCTemp(4)=CabinT
```

In this sample code, the commented text (formatted in a bold green font style by the Syntax Highlighter) is ignored by the datalogger compiler.
