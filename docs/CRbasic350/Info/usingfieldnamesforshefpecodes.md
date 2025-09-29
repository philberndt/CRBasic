# Using FieldNames for SHEF PE Codes

The FieldNames instruction can be used to add text to the beginning of a record for SHEF PE codes. The [FieldNames](../Instructions/fieldnames.md) instruction has two parts, the FieldName and an optional Description. The two parts are separated by a colon (for example,`FieldNames (NewName:This is the description)`). When the data is copied to the transmitter, whatever is used for the Description text is added as a prefix to the data.

**NOTE:** The datalogger allows only one FieldNames instruction for each output processing instruction.
