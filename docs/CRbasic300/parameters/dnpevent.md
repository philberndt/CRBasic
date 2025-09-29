# DNPEvent (DNP Event)

Used to create event data. An expression or variable array may be entered and used to trigger the creation of change events. If a variable or variable array is used, it must be of type Long. If the array is smaller than the source array, the last element of the array is used for all the remaining array elements. A single variable (scalar) is treated like an array of one element. Any time the event trigger variable evaluates as true, a change event will be created for the Source parameter. This allows the user to define the conditions that create change events. The default value is zero and indicates that a change event will be created whenever there is any change to the value of the Source parameter.

Type: Variable or constant
