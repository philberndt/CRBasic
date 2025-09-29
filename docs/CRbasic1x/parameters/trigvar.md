# TrigVar (Trigger Variable)

The constant, variable, or expression that will be tested to determine whether or not to trigger output to the DataTable. If the TrigVar is 0, a new record will not be saved to the datatable. If it is any non-zero number, a new record will be written.

| Logic | Value | Result         |
| ----- | ----- | -------------- |
| False | 0     | Do not trigger |
| True  | ≠ 0   | Trigger        |

Type: Constant, Variable, or Expression
