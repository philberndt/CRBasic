# BooleanVariable (Boolean Variable)

A variable or variable array that is used to hold the result if the client sends one of the discrete on/off commands to the server (i.e., 01 Read Coil/Port Status, 02 Read Input Status, 05 Force Single Coil/Port, 15 Force Multiple Coils/Ports). This parameter must be dimensioned as aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

or a compiler error is returned.

If a 0 is entered for this parameter, then the discrete commands are mapped toC1, C2, SE1, SE2, SE3, and SE4instead. This will cause a compile warning, "A set coil function could conflict with a control port already in use." This is a warning, not an error, and the program can run successfully.

Type: Variable or Constant
