# EncryptKey (Encryption Key)

Contains the encryption key. The string can be up to 63 bytes in length. If this parameter is null (""), the datalogger's PakBus Encryption Key setting will be used. If neither the EncryptKey parameter nor the PakBus Encryption Key setting contains a value and the Encryption function is in the program, a compile error will be returned.

Type: String Constant
