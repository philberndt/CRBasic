# Function

Used to configure the port. A binary value is entered to set each port location. 0 configures the port for input; 1 configures the port for output. Using the mask &B110, if the Function parameter is set to &B110, ports at bit no 3 and 2 will be configured for output (port at bit no 1 uses the code for input, but it is not affected because of the mask).

Type: Binary value (preceded by &B) or Integer (from 0 to 255 representing the binary value)
