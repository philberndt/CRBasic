# FileEncrypt (Encrypt File)

The FileEncrypt function is used to encrypt a file stored on the datalogger so that it cannot be read.

## Syntax

FileEncrypt("Device:FileName")

The following is an example of using FileEncrypt to make a CRBasic program stored on the datalogger's CPU unreadable. The Scan count is set at 1, so the file is encrypted and the program stops.

```
BeginProg
Scan (1,Sec,3,1)
FileEncrypt( "CPU:TopSecretFile.CRB" )
NextScan
EndProg
```

**NOTE:** Ideally, file encryption is done by CRBasic's Save and Encrypt functionality. If using a datalogger program to encrypt a file, a "run once" program should be created that encrypts the file and then stops execution. Putting FileEncrypt in a running program will slow down program execution time and possibly cause erratic behavior.

## Remarks

FileEncrypt creates a binary encrypted file that is the same size as the original file. The encrypted file is decoded automatically at compile time if it is used in a CRBasic program. This function allow distribution of CRBasic files without exposing the source code.

FileEncrypt returns true if the file is successfully encrypted. If the file is already encrypted, is not found, or cannot be opened, the function returns false.

The contents of a variable can be encrypted (and subsequently decrypted) using the [Encryption](encryption1.md) function.

**NOTE:** Ideally, file encryption is done by CRBasic's **Save and Encrypt** functionality [(seeFile Menufor more information)](../Info/filemenu.md). If using a datalogger program to encrypt a file, a "run once" program should be created that encrypts the file and then stops execution. Putting FileEncrypt in a running program will slow down program execution time and possibly cause erratic behavior.

## Parameter

# "Device:FileName" (Name of File to be Encrypted)

A string, enclosed in quotes, that contains the name of the file to be encrypted. The Device on which the file is stored is specified as CPU. (The only valid device option for this datalogger.)

Type: String
