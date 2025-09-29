# FileName (Device and File Name)

Specifies the device to write to and the name of the file to create. The created file will have a suffix of X.dat, where X is a number that will be incremented each time a new file is written.

FileName must be a constant and enclosed in quotes. It is entered in the format of "Device:FileName".The device choices areCRD (memory card), USR (user-defined drive),or USB (SC115).

The USR drive is an area of memory that can be set up by the user by assigning a value to the datalogger's UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512 byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up. It may also be rounded up if additional bytes are needed for overhead).

Type: Constant
