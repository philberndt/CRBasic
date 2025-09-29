# EncryptInitLen (Encrypt Initialization Length)

The length of the initialization vector. Valid entry is an integer between 0 and 16. If a non-zero length is used, anMD5

**Note:** 16 byte checksum of the TCP/IP VTP configuration.

checksum of the initialization EncryptInit is calculated and used as the initialization vector for the encryption.

If the process being encrypted is large and requires more than one packet to be transferred, length should be set to 0 after the first packet. This causes the EncryptInit parameter to be ignored and the encryption algorithm uses the initialization vector saved internally.

Type: Integer
