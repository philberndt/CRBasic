# FromFileName

The name of the file to be copied. It is a string entered in the format "Device:FileName". If a Device is not specified, the CPU drive will be assumed. Valid devices are:

| Device | Description          |
| ------ | -------------------- |
| CPU:   | Internal CPU         |
| CRD:   | External Memory Card |
| USR:   | User-Defined Drive   |
| USB:   | SC115                |

The USR device is an area of memory that can be set up by the user by assigning a value to the datalogger's UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512-byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up).
