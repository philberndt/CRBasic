# "Device:Filename" (Device and Filename)

Specifies the device to be written to/read from and the name of the calibration file. The Device:Filename string must be enclosed in quotation marks.Valid devices are:

| Device | Description          |
| ------ | -------------------- |
| CPU:   | Internal CPU         |
| CRD:   | External Memory Card |
| USR:   | User-Defined Drive   |
| USB:   | SC115                |

The USR device is an area of memory that can be set up by the user by assigning a value to the datalogger's UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512-byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up).

Use a Device other than CPU if writing the calfile frequently. The datalogger s CPU has limited write cycles and writing to it frequently may exceed this limit.

Type:String

**Note:** A data type used when declaring a variable consisting of alphanumeric characters.

in quotes
