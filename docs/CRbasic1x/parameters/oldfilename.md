# OldFileName (Original File Name for File Rename)

The name of the file to be renamed. It is a string entered in the format "Device:FileName". If Device = CPU, the file is stored in datalogger memory. If Device = CRD, the file is stored on a memory card. If Device = USB, the file is stored onthe SC115. If Device = USR, the file is stored in a user-created memory space.

The USR drive is an area of memory that can be set up by the user by assigning a value to the datalogger's UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512 byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up). If a Device is not specified, the CPU drive will be assumed.

Type: String
