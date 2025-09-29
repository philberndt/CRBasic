# TableNo (Table Number)

Specifies the table in the remote datalogger from which a record will be retrieved (GetDataRecord) or accepted (AcceptDataRecords). This is a numeric value that represents the data tables in the order they are declared in the program (for example, the first data table declared in the program is 1, the second data table declared in the program is 2, etc.).

If 32768 (&H8000) is added to the TableNo parameter, the table definitions in both the local and remote dataloggers do not need to be identical. As an example, to accomplish this for table 1, you can enter 32769, or 1+32768, or 1+&H8000. Table 2 would be 32770, etc.

Type: Constant
