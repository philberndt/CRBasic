# Boolean Values, True and False

In CRBasic aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

value is a variable that evaluates as true (-1) or false (0). The predefined constants **True** and **False** can be used for the state of a Boolean variable. Although CRBasic considers any value that evaluates as **0** (zero) to be false and any non-zero value to be true, if the predefined constant True is used in a conditional statement, the expression must evaluate to -1 for the datalogger to consider it true. As an example:

Public Flag(2)

If Flag(1) = True Then (do some action)

If Flag(1) Then (do some action)

In the first instance, the condition is true only when Flag(1) = -1. Any other non zero value is considered false. That is because the datalogger checks for equality using the predefined True value of -1.

In the second instance, because the predefined constant True has not been used, any non-zero expression will evaluate as true.

If a variable is declared as a Boolean, the only valid values are 0 and -1. Any non-zero value placed into a Boolean is converted to a -1. Thus, in the first example, if Flag(1) was declared as a Boolean, the condition would be true with -1 or any non-zero value.

Variables can be declared as Boolean using [Public](../Instructions/public.md) or [Dim](../Instructions/dim.md) instructions (for example, Public MyVar as Boolean).
