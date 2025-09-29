# NBytes (Number of Bytes)

The number of bytes that should be stored in Dest after the BeginWord has been received. If NBytes is equal to or less than 0, then bytes are read between the BeginWord and the EndWord, exclusive of the BeginWord and EndWord. If the BeginWord is not used (BeginWord = 0) then this parameter is the number of bytes to store prior to the EndWord.

Type: Constant
