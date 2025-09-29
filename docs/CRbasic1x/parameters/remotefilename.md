# RemoteFileName (Remote File Names)

This parameter specifies the remote file name or a comma-separated list of file names for sending, retrieving, or deleting data, or for listing directory contents. For renaming, it indicates the new file name(s). Include the directory and file name (e.g., /Data/file01.dat ). Multiple files should be listed as file01.dat,/Data/file02.dat . Use forward slashes (/) for directories, suitable for all operating systems. If no directory is specified, the root directory is assumed. The specified directory must pre-exist on the server.

To add a timestamp to the remote file name, include this exact string of characters YYYY-MM-DD_HH-MM-SS in the RemoteFileName parameter (For example tabledata_YYYY-MM-DD_HH-MM-SS.xml ). When the file is written, a timestamp will be put into the remote file name in place of YYYY-MM-DD_HH-MM-SS .

If [streaming data](../Instructions/ftpclient.md#Streaming), the timestamp of the first record streamed is used in the remote file name. If data streaming is not used, the current datalogger clock time is used.

Other methods to include a timestamp in the remote file name involve using string concatenation to insert the return value of [tablename.timestamp](../Instructions/tablenametimestamp.md)(5, 1) into the file name. See [Example Program 1](../Instructions/ftpclient.md#Example1) for an example of including a timestamp in the remote file name using this method.

When streaming data, the remote file name is automatically appended with an incrementing file number and a .dat file extension unless 1000 is added to the format option. For example, if the FileOption is 8 (TOA5, Header, Timestamp, Record#) and 1000 is added (1008), the data logger will not automatically append the incrementing number or .dat extension to the uploaded file.

Also, if the RemoteFileName parameter contains the exact string YYYY-MM-DD_HH-MM-SS for the addition of a timestamp, as described above, the incrementing file number and .dat extension will not automatically be appended.

If the automatically appended incrementing file number and .dat extension are desired along with having a timestamp in the filename, use a method other than YYYY-MM-DD_HH-MM-SS , like the concatenation method in Example Program 1.

Type: Constant string (enclosed in quotes) or variable string
