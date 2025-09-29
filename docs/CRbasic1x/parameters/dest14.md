# Dest (Destination Variable)

Variable or variable array that holds the output of the encryption or decryption. Note that output to the destination variable can cross array element boundaries. When encrypting, Dest should be declared with a Size greater than the length of EncryptSrc, rounded up to the next multiple of 8 bytes. (For example, if source is 10 bytes, 16 bytes will be placed in destination.)

Type: Variable or Variable Array
