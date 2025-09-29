# EC100Result

A variable that contains a value indicating the success or failure of the command. A result code of 0 means that the command was successfully executed. If reading a setting, 0 in the result code means that the value in the DestSource variable is the value the desired setting has in the EC150 or EC155. When writing a setting if the result code is 0 the value and setting were compatible, but the value was not changed because it contained the same value that was sent. A return code of 1 from the set operation means that the value was valid, different, set and acknowledged. This allows CRBasic code to control whether or not to save the settings. NAN indicates that the setting was not changed or acknowledged or a signature failure occurred.

Type: Variable
