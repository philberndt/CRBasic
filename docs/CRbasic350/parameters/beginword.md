# BeginWord (Beginning of Record)

A two byte word (1 to 65535) that indicates the beginning of a record. If the value entered is less than 256, the datalogger will look only for a single byte. If 0 is entered, a BeginWord is not used and the data stored in Dest will be NBytes prior to the EndWord. If &H80000000 is entered, a Null character will be used for BeginWord.

Hexadecimal values can be entered for BeginWord and EndWord by preceding the characters with &H.

Type: Constant integer
