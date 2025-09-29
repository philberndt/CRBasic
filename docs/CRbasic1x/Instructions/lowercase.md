# LowerCase (Convert to Lowercase)

The LowerCase instruction is used to convert a string to all lowercase characters.

## Syntax

String =LowerCase(SourceString)

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

## Parameter

# SourceString (Source String)

The string to be acted upon with this instruction.

**NOTE:** String functions are case sensitive. Uppercase or lowercase can be used to convert to all one case prior to processing the string if desired.

Type: String
