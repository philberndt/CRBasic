# OutStat (Output Status)

A variable that holds a value indicating whether or not a new file has been stored. If a new file is written when the instruction is executed, a -1 will be stored. If a new file is not written, a 0 will be stored. If this parameter is set to 0 instead of a variable, it will be ignored. Note that this variable is updated one scan behind when the file is written.

Type: Variable
