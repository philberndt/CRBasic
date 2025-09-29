# FormatLong (Convert a Long Value)

The FormatLong function is used to convert a Long value to a decimal, hexadecimal, or octal string.

Consider using [Sprintf](sprintf.md) rather than FormatLong, which provides greater flexibility and functionality.

## Syntax

_Variable_=

```
FormatLong
```

(LongVar,FormatString)

The following test program will allow you to enter a value into "LongVar" and then choose a formatting string by entering 1 through 5 in the "Control" variable for each of the five options. The result is stored in the "Formatted" variable.

This test program allows you to see the effects of each of the options on a long variable. The "Type" variable is for informational purposes it is a a string that lets you see which formatting option has been chosen. Values can be changed using the numeric monitor of a datalogger support software program.

```
Public LongVar As Long = 99999, FormatString As String, Control = 1, Type As String
Public Formatted as String

BeginProg
Scan (1,Sec,3,0)

If Control = 1 Then
FormatString = "%d"
Type = "d"
ElseIf Control = 2 Then
FormatString = "%i"
Type = "i"
ElseIf Control = 3 Then
FormatString = "%x"
Type = "x"
ElseIf Control = 4 Then
FormatString = "%X"
Type = "X"
ElseIf Control = 5 Then
FormatString = "%o"
Type = "o"
EndIf

Formatted=FormatLong(LongVar,FormatString)

NextScan

EndProg
```

## Remarks

FormatLong can be used to store a formatted Long into a string, or it can be used within another instruction to format a Long.

## Parameters

# LongVar (Long Variable)

The variable that will be formatted.

Type: Long variable

# FormatString (Format String)

A code used to apply formatting to the Long variable. Options are:

| Code | Description                        |
| ---- | ---------------------------------- |
| %d   | Signed decimal integer (same as i) |
| %i   | Signed decimal integer (same as d) |
| %u   | Unsigned decimal integer           |
| %x   | Hexadecimal integer, lower case    |
| %X   | Hexadecimal integer, upper case    |
| %o   | Octal (base 8)                     |
| %b   | Binary (base 2)                    |

Type: Variable

The formatted Long can be padded with zeros (or spaces), using the notation %#code (right justified) or %-#code (left justified), where # is the number of spaces for the field.

Consider the following examples, where the Long variable is 12345.

- If FormatString = %8d the result is (space)(space)(space)12345

- If FormatString = %-8d the result is 12345(space)(space)(space)

- If FormatString = %08d the result is 00012345

- If FormatString = %-08d the result is 12345000

Note that multiple format specifiers are not allowed.
