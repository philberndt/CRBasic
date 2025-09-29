# Gzip (Create Compressed File)

The Gzip function creates a compressed file with a .gz suffix. If multiple files are compressed the resulting file has a tar.gz suffix.

## Syntax

Gzip(Filename,ZippedFilename)

or

_Result =_ Gzip( Filename, ZippedFilename )

In this example program, two Gzip instructions are used to demonstrate zipping a single file and multiple files. In this example, the files are stored on a SD card. The Result variable stores either the number of files succssefully zipped or an error code.

```
Public RefTemp, Batt_volt
Public FileZip as Boolean
Public Result(2) as Long
Public ZipDone as Boolean

BeginProg
Scan (1,Sec,0,0)
If FileZip Then'Set true to start zip
ZipDone = False'lets user know if Zip is done
'Zip multiple programs
Result(1) =Gzip("CRD:Program1.cr1x,CRD:Program2.cr1x,CRD:Program3.cr1x","CRD:ZippedFiles")'creates ZippedFiles.tar.gz on a SD card
'Zip a data file on a SD Card
Result(2) =GZip("CRD:Data1.dat","CRD:Data1.dat")'Creates Data1.dat.gz on a SD card
FileZip = False'reset zip trigger
ZipDone = True'let user know when the zip is complete
EndIf
PanelTemp (RefTemp,15000)
Battery (Batt_Volt)
NextScan
EndProg
```

## Remarks

The Gzip instruction is commonly placed in a conditional statement and triggered with a variable flag. Zipping a file may take several seconds to minutes, depending on file size. Zipping a file has the potential to significantly reduce its size. The amount of reduction realized depends mainly on the number and proximity of redundant blocks of information in the file. Note that compression has little effect on an encrypted program (FileEncrypt in the CRBasic Editor), since the encryption process does not produce a large number of repeatable byte patterns. Additionally, GZip has little effect on files that already employ compression such as JPEG or MPEG-4. For other file types, a reduction in file size means fewer bytes are transferred when sending a file. This reduction can have a significant impact on transfer times over slow or high latency links or cost when utilizing "pay by the byte" data plans. Those using low-baud-rate terrestrial radio, satellite, or restricted cellular data plans should consider zipping programs, data, and operating systems before sending. TheCR1000Xalso supports Gzip extract. Click [here](../Info/gzipextract.md) for more information.

The Gzip function returns the number of files successfully compressed or an error code. Error codes are: -1 if there is not enough room to store the file or -10 if the file cannot be opened (for example, the file is missing or is named differently than specified).

**NOTE:** It may take several minutes to complete compression of large files or multiple small files. A flag may be set in the program to indicate when compression is done.

## Parameters

# Filename

Specifies the name of the file, or files, to be zipped/compressed. FileName must be a constant and enclosed in quotes. It is entered in the format of "Device:FileName" where Device is CRD: (memory card), USR: (user-defined drive),orUSB:(SC115). If more then one file is to be compressed, this is a comma-separated list. For example, "CRD:FileName1,CRD:Filename2,CRD:Filename3".

Type: Constant enclosed in quotes

# ZippedFilename

Specifies the name of the resulting zipped file. If compression is successful, .gz will be appended to the file name. If more than one file is zipped, .tar is appended first and then .gz. For example, ZippedFiles.tar.gz.

Type: Constant enclosed in quotes
