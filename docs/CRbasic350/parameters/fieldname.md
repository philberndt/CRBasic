# "Fieldname" (Field Name)

Used to specify the name of the variable(s) in the destination datalogger for (SendVariables) or retrieve from (GetVariables). If Swath is greater than 1, FieldName must be an array. FieldName must be entered as a string (enclosed in quotes). If the variable in the source datalogger has been assigned an Alias, the alias must be used for Fieldname unless the value requested is from the Public table. In this case, either the original name or the Alias name can be used (the exception is CR200 series dataloggers; they require that the Alias be used). If the requested Fieldname is from an output table of a CRBasic datalogger, and the output is something other than a sample, the output type suffix must be added to the variable name (for example, Temp_Avg).[For more information, seeCRBasic Program Structure.](../Info/crbasicprogramstructure.md)

Type: String or Variable that evaluates as a string
