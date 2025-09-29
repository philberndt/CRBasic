# MaxNameSpaces (Maximum Number of Namespaces)

The MaxNameSpaces parameter controls, in part, the amount of memory that will be allocated for the parser. This value must be greater than equal to one or the maximum number of namespaces that are anticipated to be in the document. For instance any document that is exchanged using the SOAP protocol will likely have at least two namespaces. If too small a value is specified, the parser will return a value of -3 while parsing the document.

Type: Constant
