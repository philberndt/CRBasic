# Mode

Determines whether the file is being read from or written to, and whether the file format is ASCII or binary. Mode must be enclosed in quotes. Right-click the parameter to display a list of the following options:

| Mode  | Description                                                                                                                                                          |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| "a"   | Append to ASCII file atEOF **Note:** End of file. (write). Set SeekPoint to -1 to append to end of file, or specify a value to begin writing other than end of file. |
| "ab"  | Append to binary file at EOF (write). Set SeekPoint to -1 to append to end of file, or specify a value to begin writing other than end of file.                      |
| "a+"  | Append to ASCII file at EOF (read/write). Set SeekPoint to -1 to append to end of file, or specify a value to begin writing other than end of file.                  |
| "a+b" | Append to binary file at EOF (read/write). Set SeekPoint to -1 to append to end of file, or specify a value to begin writing other than end of file.                 |
| d     | Opens a connection to a specified directory; used to determine if a drive is available.                                                                              |
| "r"   | Open ASCII file for reading at SeekPoint (read).                                                                                                                     |
| "rb"  | Open binary file for reading at SeekPoint (read).                                                                                                                    |
| "r+"  | Open ASCII file for update at SeekPoint (read/write).                                                                                                                |
| "r+b" | Open binary file for update at SeekPoint (read/write).                                                                                                               |
| "w"   | Open/overwrite ASCII file (write). SeekPoint is not valid; leave at 0.                                                                                               |
| "wb"  | Open/overwrite binary file (write). SeekPoint is not valid; leave at 0.                                                                                              |
| "w+"  | Open/overwrite ASCII file (read/write). SeekPoint is not valid; leave at 0.                                                                                          |
| "w+b" | Open/overwrite binary file (read/write). SeekPoint is not valid; leave at 0.                                                                                         |

**NOTE:** If the file is opened with a mode that specifies ASCII, when a Chr(10) (line feed) is encountered, a Chr(13) (carriage return) is inserted before the line feed.

The [MoveBytes](../Instructions/movebytes.md) instruction should be used to move floats into a string variable if TOB1 binary files are being written.

Type: String Variable
