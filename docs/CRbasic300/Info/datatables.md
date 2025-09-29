# About Data Tables

A Data Table consists of the [DataTable](../Instructions/datatable.md) instruction, Output Trigger Condition(s), Output Processing Instructions, and the [EndTable](../Instructions/endtable.md) instruction. The DataTable instruction has three parameters:Name,TrigVar, andSize. The TrigVar can be aConstant

**Note:** A non-varying fixed number.

, Variable, orexpression, and is used, along with the other (optional) output trigger conditions specified by the [DataInterval()](../Instructions/datainterval.md) and [DataEvent()](../Instructions/dataevent.md) declarations, to trigger the Output Processing Instructions to store processed data to memory.

Output Processing instructions are executed whenever the DataTable is called, regardless of the TrigVar state (true or false). Output Processing instructions do their final processing and store the results to memory; i.e., create a new record of data, when the TrigVar is true and the other (optional) conditions are also met. When the TrigVar is false, the Output Processing instructions perform their intermediate processing but not their final processing, and a new record will not be created.

Commonly, if the output records are solely interval based, the TrigVar is set to True always, and DataInterval() is the sole specifier of the output trigger condition. Otherwise, the TrigVar parameter is used to specify an output trigger condition that cannot be specified, at least entirely, by DataInterval.

**NOTE:** For information on retrieving data stored in data tables to use in your program, see [Data Table Access](datatableaccess.md).
