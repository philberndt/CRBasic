# Constant Customization

The Constant Customization feature allows you to define values for one or more constants in a program prior to performing a conditional compile (Compile | Conditional Compile and Save). The constants can be set up with an edit box, a spin box for selecting/entering a value, or with a list box. A step increase/decrease can be defined for the spin box, as well as maximum and minimum values.

To set up Constant Customization, place the cursor on a blank line within the CRBasic Editor then click the **Tools** menu and select **Set Up Constants Customization Section**. This will insert two comments into the program:

```crbasic
'Start of Constants Customization Section

'End of Constants Customization Section
```

Within these two comments, define the constants. Following each constant, use the following keywords (formatted as a comment) to set up edit boxes, spin boxes, or list boxes for the constant values. The fields are edit boxes by default. If a maximum/minimum are defined for a constant, the field will be a spin box. If a discrete list is defined for the constant, the field will be a list box.

- **ConstantName=(value)**: Sets a default value for a constant.

- **Min=(value)**: Sets a minimum value for a spin box control.

- **Max=(value)**: Sets a maximum value for a spin box control.

- **Inc=(value)**: The number of steps for a value each time the up or down control on the spin box is selected.

- **Value=(value)**: Defines a list value.

The Constant Customization syntax may be best understood by looking at an example. Consider the following program code:

```crbasic
'Start of Constants Customization Section
ConstSInterval = 10
'Min = 5
'Max = 60
'Inc = 5

ConstSUnits= sec
'value = sec
'value = min

ConstReps = 1

ConstNumber = 0
'Min=-100
'Max = 100

ConstTableName="OneSec"
'value="OneMin"
'value="OneHour"
'value="OneDay"

'End of Constants Customization Section
```

This code will create the following constant customization window:

- The constant **SInterval** is defined with a default value of 10, a maximum of 60 and a minimum of 5, with a step of 5 each time the up or down control is selected.

- The constant **SUnits** has a list box with sec and min; sec is the default.

- The constant **Reps** is defined with a default value of 1. It is an edit box, into which any value can be entered.

- The constant **Number** is defined with a default value of 0, a minimum of -100 and a maximum of 100. The value will increase by 1 each time the up or down control is selected.

- The constant **TableName** is defined with a list box of "OneSec", "OneMin", "OneHour", and "OneDay"; the default value is "OneSec".

Before compiling the program, open the Customize Constants window (as shown previously), select the constant values you want to compile into the program, and then perform the **Conditional Compile and Save**. See also [Compile Menu.](compilemenu.md)
