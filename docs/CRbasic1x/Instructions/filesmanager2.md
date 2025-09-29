# SetSetting FilesManager Settings

The`FilesManager`setting lets you define the maximum number of files that are kept in a directory that come from a specific PakBus device. Once the maximum number of files is reached, the oldest file is deleted prior to writing the new file.

**NOTE:** The following examples show managing files on the USR drive.

The format for`FilesManager`is:

(PakBusAddr, FileName, MaxNumberFiles)

where PakBusAddr is thePakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of the datalogger that is the source of the files, FileName is the name to use when the file is stored on the local datalogger (with an incrementing numerical identifier appended to the base name), and MaxNumberFiles is the maximum number of files to keep before deleting the oldest file and writing a new file. If the MaxNumberFiles is 0, then the numerical identifier is not used.

| Files Manager can be set on the **Settings Editor | Advanced** tab in theDevice Configuration Utility |

**Note:** Configuration tool used to set up dataloggers and peripherals, and to configure PakBus settings before those devices are deployed in the field and/or added to networks.

setup utility or by using the [SetSetting](setstatussetsetting.md) instruction. Up to four Files Manager settings can be entered.

In the previous example, the datalogger has been set up to store files from PakBus Address 3000 in its USR drive to a file named CR3-XXX (where xxx is an incrementing numeric value), with a maximum of three files specified. It has also been set up to store files from PakBus Address 1086 in its USR drive to a file named CR8-XXX, with a maximum of three files specified. To set these same settings using CRBasic, the syntax would be:

SetSetting ( FilesManager , (3000, USR:CR3-.dat,3) (1086, USR:CR8-.dat,3 )

The Files Manager keeps open any file that it is writing; thus, there may be problems copying or deleting the file. If SetSetting executed again, the existing file is closed and a new one is open. The syntax SetSetting ( FilesManager , 0, , 0 ) will clear the setting.

## Special Use Cases for Files Manager

There are values that can be used in the PakBus address parameter that provide special functionality in the datalogger.

**3209 **: If a PakBus address of 3209 is used, the datalogger will log communication to a file with the syntax of the terminal mode's W command. The Filename is used to specify whether to output Binary (W) or ASCII (w) and the COM port to monitor. The COM port is defined using the numeric characters from the COM port codes in the W command. For example, USR:w1myfile.ext specifies writing communication from ComRS232 to an ASCII file named myfile.txt on the USR drive. The last parameter (normally, number of files) specifies the maximum file size, in bytes.

** 3210 **: A PakBus address of 3210 is used when a file is written to the datalogger using FTP. The first file stored on the datalogger would be MyFile1.txt, the second file would be named MyFile2.txt, etc. The name of the file is stored in the LocalFileName variable defined by the FTPClient instruction.

** 3211 **: A PakBus address of 3211 is used with the [FileWrite](filewrite.md) function. Each time a file is opened for writing ([FileOpen](fileopen.md) is executed, followed by a FileWrite), the file that is opened is given a unique name with an incrementing numerical suffix; for example,

FileOpen ("USR:MyFile.txt","a",-1)

The first file opened for writing would be named MyFile1.txt, the next file would be named MyFile2.txt, etc.

** 3212 **: A PakBus address of 3212 is used to write IPTrace information to a file. The type of information written is determined by the IP Trace Code setting in the datalogger.

** 3213 **: A PakBus address of 3213 is used to manage incoming files via [HTTPGet](httpget.md).

The Filename argument is used to specify the name of the file to be written and the Count argument is used to specify the size of the file in bytes. The file is set up as ring memory where once the file size is reached, new messages will overwrite old messages (this will lead to the logged information being out of order in the file). For example,

(3212, USR:IPTrace.txt, 5000)

This syntax will create a file on the user drive called IPTrace.txt that will grow to approximately 5 KB in size, and then new data will begin overwriting old data.

** 3213**: A PakBus address of 3213 is used to manage files being written to the datalogger as a result of an HTTPGet function.
