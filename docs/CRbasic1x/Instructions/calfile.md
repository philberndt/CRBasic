# CalFile (Store Calibration Data to a File)

The CalFile instruction provides a way to store sensor calibration data from a program into a file located on the CPU: driveor computer card drive (CRD).

## Syntax

CalFile(Source/Dest,NumVals,"Device:filename",Option)

In the following example program, 25 values are written to an array and subsequently stored to a file on the CPU called Calfile.cal. The values are then read from the file and compared with the original array.

```
const numvals = 25
public tfail, tdone
public array1(numvals), array2(numvals)
dim i

BeginProg
for i = 1 to numvals
array1(i) = i'write values into array
next i
CalFile(array1,numvals,"CPU:calfile.cal",0)'store the values to the file
CalFile(array2,numvals,"CPU:calfile.cal",1)'read the values from array2
for i = 1 to numvals'test retrieved values
if array2(i) <> array1(i) then
tfail = 1
endif
next i
tdone = 1
EndProg
```

## Remarks

The CalFile instruction can also be used to write one or more stored calibration files to the datalogger's non-volatile flash memory. When the datalogger is powered up, all such files located in flash memory are loaded into SDRAM memory, which can then be accessed by the program through the use of the CalFile instruction.

The data in the file is stored as 4 byte binary single precision floating point values (in the native format of the datalogger) with a 2 byte signature appended to the end of the data. This signature is checked (if reading) to verify that the file is not corrupt. When writing to a file, the signature is calculated and stored with the data.

## Parameters

# Source/Dest (Source or Destination)

A variable array specifying where to read data from or write data to.

Type: Variable Array

# NumVals (Number of Values)

The number of values that should be read from or written to the calibration file.

Type:Constant

**Note:** A non-varying fixed number.

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

# Option

A numeric code used to determine whether to create a calibration file or to read from a calibration file. Right-click the parameter to display a list of valid options:

| Option | Description                                                           |
| ------ | --------------------------------------------------------------------- |
| 0      | Write source array to a file                                          |
| 1      | Read data from a file and if the signature matches, write to an array |
