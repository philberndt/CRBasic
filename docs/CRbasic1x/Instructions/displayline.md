# DisplayLine

The DisplayLine instruction is used to display a full line of read-only text in a custom menu.

## Syntax

DisplayLine(Value)

The following is aCR1000X Seriesdatalogger program that shows how DisplayLine is used. Different sized strings are used for the Value parameter, so that you can see an example of how the values will be displayed when running in a program.

This program creates a custom menu item named Main. The DisplayLine strings can be viewed from that menu.

This program was developed for aCR1000X, but this program, or a similar one, should run in other dataloggers that support this instruction.

```
Public batt_volt
Public Counter

Const Short = "Short constant"
Const Lng = "Long constant that is too long to fit on one line"

DisplayMenu ("Main",0)
DisplayLine("Short text")
DisplayLine("Long text, longer than 20 characters.")
DisplayLine("Longer text that cannot be displayed in a 20 character by 7 line display. It is so long that when you press the > arrow to see the entire string, you will also have to Page Up/Page Down or use up/down arrows to see all text.")
'DisplayLine(String values have to be in quotes)
DisplayLine(Short)
DisplayLine(Lng)
'Display a variable and some text
DisplayLine(batt_volt & "volts")
'Display a variable and some text, while formatting the variable to have 2 decimal places
DisplayLine(FormatFloat(batt_volt,"%0.2f") & " volts")
DisplayLine(Counter & ": Counter displayed at front. Notice how it updates. It updates while displayed on the menu but not after you've arrowed over.")
EndMenu

BeginProg
Scan (1,Sec,0,0)
Battery (batt_volt)
Counter = Counter + 1
NextScan
EndProg
```

## Remarks

The value to display can be a string, variable, constant, evaluation, or an expression containing a combination of these items. String values must be enclosed in quotes. Use an ampersand, &, to concatenate variables with strings.

When the custom menu is viewed using a datalogger's keyboard display, only 20 characters of text can be displayed in the viewable menu window. If the DisplayLine exceeds 20 characters, press the right or left arrow key to display up to 7 lines of 20 characters on the screen. If Value includes more than 7 lines of text, use the up and down arrow keys, or page up/page down, to scroll through the remaining text.

## Parameter

# Value

The Read-only text to display in the custom menu. String values must be enclosed in quotes. Use an ampersand, &, to concatenate variables with strings.

Type: String, Variable, Constant, Expression
