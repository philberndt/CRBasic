# FileManage (Manage Files)

The FileManage instruction is used to manage files from within a running datalogger program.

## Syntax

FileManage("Device: FileName",Attribute)

The following program uses FileManage to run TC-TEMP.cr1x, which is stored on the datalogger CPU, after 120 scans of the program.

```
Public PTemp, TCTemp

BeginProg

'Run Program 120 scans
Scan (1,Sec,3,120)
PanelTemp (PTemp,15000)
TCDiff (TCTemp,1,mv200C,1,TypeT,PTemp,True,0,15000,1.0,0)

NextScan

'Branch to second program
Scan (1,Sec,3,0)
FileManage("CPU:TCTemp.cr1x",4)
NextScan

EndProg
```

## Remarks

FileManage is an instruction that allows the active datalogger program to manipulate program files that are stored in the datalogger.

## Parameters

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

# Attribute

A numeric code to determine what should happen to the file affected by the FileManage instruction. Attribute codes are a bit field. The codes are as follows:

| Bit        | Decimal | Description            |
| ---------- | ------- | ---------------------- |
| bit 0      | 1       | Program not active     |
| bit 1      | 2       | Run on power up        |
| bit 2      | 4       | Run now                |
| bits 1 & 2 | 6       | Run now/Run on powerup |
| bit 3      | 8       | Delete                 |
| bit 4      | 16      | Delete all             |
| bit 5      | 32      | Hide                   |

Setting a file's attributes to **Hide** makes it inaccessible using communications or the keyboard, but it can still be set as **Run Now** or **Run on Power Up**.

A program marked as ** Run on Power Up**can be disabled when power is first applied to the datalogger by pressing and holding the DEL key on the (optional) CR1000KD.

Type: Constant

The [Restart](restart.md) instruction can be used to restart a datalogger program under program control, rather than using FileManage to Run Now/Run On Powerup.
