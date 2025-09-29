# Encryption (Encrypt/Decrypt)

The Encryption function is used to encrypt or decrypt the contents of a variable.

## Syntax

_ResultVar _ = **Encryption**(Dest,EncryptSrc,SrcLen,EncryptKey,EncryptInit,EncryptInitLen,EncryptOption)

In the following program example written for aCR1000X, the datalogger encrypts a file that is written to theUSRdrive and then writes the encrypted file to a card.

If using this instruction in other datalogger models, you may need to change some parameters for measurement instructions or the drives used to write files to/from, based on the capabilities of your datalogger.

```
Const MaxFileSize = 10000'Maximum number of bytes that a file will be

Public batt_volt
Public FileCounter As Long
Dim OutStat As Boolean, ProcessFile As Boolean
Dim LastFileName As String * 32'Unencrypted file on the USR drive
Public EncFileName As String * 32'Variable, name of encrypted file on the CRD
Const Key = "SuperSecret"'Preshared key used in the encryption, should use a strong password
Dim FileHandle As Long'Used as a file access handle. Only one file accessed at a time, so is reused
Const MaxContentsLength = Ceiling(MaxFileSize/4) + 4
Dim BinaryContents(MaxContentsLength) As Long'Temporary storage. It is 16 bytes longer than the maximum source length to accommodate padding
Dim EncContents(MaxContentsLength) As Long'Temporary storage. It is 16 bytes longer than the maximum source length to accommodate padding
Public SourceLength As Long
Public InitVector(4) As Long
Public EncryptResult As Long
Public PaddedLength As Long

'DefineDataTables
DataTable (OneMin,1,-1)'Set table size to # of records, or -1 to autoallocate.
TableFile ("USR:DataFile",8,1,1,0,Min,OutStat,LastFileName)
DataInterval (0,60,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
EndTable

'Main Program
BeginProg
'Initialize random number generator using seconds since 1990 + microseconds in datalogger clock
Randomize(Status.Timestamp(1,1) + Status.Timestamp(7,1))
SetSetting ("USRDriveSize",100000)

Scan (1,Sec,0,0)
Battery (batt_volt)

CallTable OneMin
If OutStat Then
ProcessFile = True
EndIf
NextScan

SlowSequence
Scan (1,Sec,3,0)
If ProcessFile Then
FileCounter += 1
'##Read source contents from internal USR drive##
FileHandle = FileOpen (LastFileName,"rb",0)
Erase(EncContents)
SourceLength = FileRead (FileHandle,BinaryContents(),MaxContentsLength)
FileClose (FileHandle)
'##Encrypt the data##
' Initialization vector is a 16 byte random value
InitVector(1) = RND * &h7FFFFFFF
InitVector(2) = RND * &h7FFFFFFF
InitVector(3) = RND * &h7FFFFFFF
InitVector(4) = RND * &h7FFFFFFF

EncryptResult =Encryption(EncContents(),BinaryContents(),SourceLength,Key,InitVector,16,0)
'##If successful, write encrypted data to a file on the card##
If EncryptResult > 0 Then
EncFileName = "CRD:EncData" & FormatLong (FileCounter,"%08X") & ".aes"
FileHandle = FileOpen (EncFileName,"wb",0)
'For some types of source content, you would want to add a data length value onto the front of the file. TOA5 and TOB1 should be fine without.
FileWrite (FileHandle,InitVector,16)'Initialization vector is written at the beginning of the encrypted file
FileWrite (FileHandle,SourceLength,4)'Next four bytes is the length of the source data
PaddedLength = EncryptResult + IIF(EncryptResult MOD 16, 16 - (EncryptResult MOD 16), 0)
FileWrite (FileHandle,EncContents(),PaddedLength)'The encrypted data follows
FileClose (FileHandle)'Closing files is important
EndIf

ProcessFile = False
EndIf'end processing file for encryption
NextScan
EndProg
```

## Remarks

The Encryption function uses the Advanced Encryption Standard (AES) 128 as established by the U.S. National Institute of Standards and Technology. AES uses a symmetric key algorithm; thus, the encryption key used for both encrypting and decrypting the contents of a variable must be the same.

This function returns the number of bytes written to the destination variable. 0 is returned if the function fails or if the destination is not large enough to hold the resulting encrypted or decrypted message.

Two uses of the the Encryption function are:

- Encrypt or decrypt a string, value, or other content stored in a variable before saving it to a file.

- Encrypt or decrypt a message that is shared between two PakBus devices.

The initialization process sets up a context (CTX) for the encryption/decryption process. Subsequent calls to encryption/decryption reference this CTX. Thus, only one encryption process at a time can be run by the datalogger.

## Parameters

# Dest (Destination Variable)

Variable or variable array that holds the output of the encryption or decryption. Note that output to the destination variable can cross array element boundaries. When encrypting, Dest should be declared with a Size greater than the length of EncryptSrc, rounded up to the next multiple of 8 bytes. (For example, if source is 10 bytes, 16 bytes will be placed in destination.)

Type: Variable or Variable Array

# EncryptSrc (Encrypt Source)

The variable or variable array that holds the information to be encrypted or decrypted.

Type: Variable or Variable Array

# SrcLen (Source Length)

Variable that holds the number of bytes from the source to be encrypted or decrypted. A length of 0 indicates that a length of up to and including the first null character should be used.

Type: Variable declared as Long

# EncryptKey (Encryption Key)

Contains the encryption key. The string can be up to 63 bytes in length. If this parameter is null (""), the datalogger's PakBus Encryption Key setting will be used. If neither the EncryptKey parameter nor the PakBus Encryption Key setting contains a value and the Encryption function is in the program, a compile error will be returned.

Type: String Constant

# EncryptInit (Encryption Initialization)

A 16-byte value used as the initialization vector for the encryption or decryption process. At the beginning of encryption/decryption, the initialization vector must be initialized. After that, further calls to the encryption/decryption process will use and modify the initialization as part of the encryption/decryption algorithm.

Type: Variable array

# EncryptInitLen (Encrypt Initialization Length)

The length of the initialization vector. Valid entry is an integer between 0 and 16. If a non-zero length is used, anMD5

**Note:** 16 byte checksum of the TCP/IP VTP configuration.

checksum of the initialization EncryptInit is calculated and used as the initialization vector for the encryption.

If the process being encrypted is large and requires more than one packet to be transferred, length should be set to 0 after the first packet. This causes the EncryptInit parameter to be ignored and the encryption algorithm uses the initialization vector saved internally.

Type: Integer

# EncryptOption (Encrypt Option)

Determines whether EncryptSrc will be encrypted (EncryptOption = 0) or decrypted (EncryptOption = 1). Right-click to display a pick list of the two options.

**NOTE:** The [FileEncrypt](fileencrypt.md) function can be used to encrypt a file stored on the datalogger's file system. FileEncrypt uses a proprietary algorithm and can only be unencrypted by the datalogger. It is typically used to encrypt a program or a portion of a program contained in an Include file that the user would like to hide.
