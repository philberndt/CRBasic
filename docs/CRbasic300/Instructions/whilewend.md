# While...Wend

The While Wend instructions are used to execute a series of statements in a loop as long as a given condition is true.[See alsoDo...Loop.](doloop.md)

## Syntax

```
While
```

Condition

[StatementBlock]

```
Wend
```

This example creates a While...Wend that is exited only if Reply is within a range.

```
Public Reply'Declare variable.

BeginProg
WhileReply < 90
Reply = Reply + 1
Wend
EndProg
```

## Remarks

While Wend loops can be nested.

## Parameters

# While

The While statement begins the While...Wend loop control structure.

# Condition

The Condition is anyexpressionthat can be evaluated True (nonzero) or False (0 and Null). If Condition is true, all statements in StatementBlock are executed until the Wend statement is encountered. Control then returns to the While statement and Condition is again checked. If Condition is still true, the process is repeated. If Condition it is not True, execution resumes with the statement following the Wend statement.

# StatementBlock

The StatementBlock is the portion of the program that should be repeated until the loop is terminated. These instructions lie between the While and Wend statements.

# Wend

The Wend statement ends the While...Wend control structure.

**NOTE:** The [Do...Loop](doloop.md) instruction provides more structure and a more flexible way to perform looping.
