# SeekPoint

Specifies the byte position to begin reading from or writing to when the file is opened. The value is in bytes, and the read or write begins after the specified SeekPoint. For instance, if 100 is entered, the read or write begins at byte 101. If 0 is entered and a file is being written, existing data will be overwritten. If one of the four "a" options is being used to write data, enter -1 to append to the end of the file or enter a value to begin at a specific byte. SeekPoint has no affect with the "w" options, which always begin at byte 0, overwriting the existing data.

If one of the four "a" options is being used to write data, enter -1 to append to the end of the file or enter a value to begin at a specific byte. SeekPoint has no affect with the "w" options, which always begin at byte 0, overwriting the existing data.

Multiple reads or writes (prior to a FileClose for the FileHandle) begin where the previous file operation left off. When a FileClose instruction is executed for the FileHandle, the FileHandle is deleted.

Type: Variable
