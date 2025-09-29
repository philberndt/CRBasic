# UpperCase (Convert to Uppercase)

The UpperCase instruction is used to convert a string to all uppercase characters.

## Syntax

String =UpperCase(SourceString)

The following program uses the UpperCase and LowerCase instructions to change the case of the SourceString and store the result in new variables.

```
Public SourceString AS STRING * 35
Public LowString AS STRING * 35, UpString AS STRING * 35

BeginProg
SourceString="tHe weAthEr rePOrt iS FaVORablE"
Scan (1,Sec,3,0)
LowString =LowerCase(SourceString)
UpString =UpperCase(SourceString)
NextScan
EndProg
```

**NOTE:** String functions are case sensitive. Uppercase or [lowercase](lowercase.md) can be used to convert to all one case prior to processing the string if desired.
