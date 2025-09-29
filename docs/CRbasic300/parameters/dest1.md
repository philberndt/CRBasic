# Dest (Destination)

The destination variable array in which to store the fields of the record. The array must be dimensioned large enough to hold all of the fields in the record. However, if the Dest parameter is a single-dimensioned variable formatted as a string, all data values will be stored in the one variable. The data values will be separated by commas and the string will have a <crlf> termination. The record's time stamp will be the first data value. All time stamps and variables declared as strings will be enclosed in quotes. Time stamps will be displayed in International ISO format "yyyy-mm-dd hh:mm:ss" with a fractional seconds if present. Right-click the parameter to display a list of defined variables.

Type: Array
