# "Device:FileName" (Device and File Name)

"Device:FileName" is the device and name of the program file that will be executed (RunProgram) or manipulated (FileManage). The Device on which the file is stored must be specified and the entire string must be enclosed in quotation marks.Valid devices are:

| Device | Description          |
| ------ | -------------------- |
| CPU:   | Internal CPU         |
| CRD:   | External Memory Card |
| USR:   | User-Defined Drive   |
| USB:   | SC115                |

The USR device is an area of memory that can be set up by the user by assigning a value to the datalogger's UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512-byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up).

Type: Variable string
