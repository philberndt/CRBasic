# Attach (File Attachment)

A string expression containing a local file name, comma separated list of local file names, data table name, or data table field name. Local files are generally created by TableFile or aCampbell Scientificcamera. Multiple local file names can be specified as a comma separated list (for example,CPU:file1.dat,CPU:image.jpg ). Make sure to include the file directory along with the file name.The only device choice for this datalogger is CPU.

If streaming data directly from a data table or table field to an email attachment, this parameter should be a constant specifying the table name (for example, TableOne ) or table field name (for example, TableOne.BattV_Min ) that is used as the data source. Or, to stream all data tables, specify the optional data table parameters required for streaming and specify the data source as an empty string ( ").

If an attachment is not required, specify this parameter as an empty string ( ) and do not specify the optional data table parameters.

Type: Variable formatted as a string
