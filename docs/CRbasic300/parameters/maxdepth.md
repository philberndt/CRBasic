# MaxDepth (Max Nesting Depth)

The MaxDepth parameter controls, in part, the amount of memory that will be allocated for the parser. This value must be at least one greater than the anticipated maximum nesting depth of XML elements. For example, the CSIXML format, which has a maximum nesting depth of 4 elements, would require a minimum of 5 for this value. If too small of a value is specified, the parser may return a value of -2 while parsing the document.

Type: Constant
