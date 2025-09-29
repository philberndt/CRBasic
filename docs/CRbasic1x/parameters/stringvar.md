# StringVar (String Variable)

The variable name of the string that will be evaluated using the Len function. The size of the StringVar must be set large enough to accommodate the size of the actual string (including the null-termination character), or the maximum value Len will return will be the size of the string (even if the actual string is larger). Note that strings are null-terminated; the null termination character counts as one of the characters in the string. If a Size is not specified when the StringVar is defined, the default string size is 24 (23 usable bytes and 1 null terminator).

Type: Variable defined as a String
