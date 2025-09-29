# XMLParse (Read and Parse an XML File)

The XMLParse function is used to read and parse an XML file in the datalogger.

## Syntax

XMLParse(XMLContent,XMLValue,AttrName,AttrNameSpace,ElemName,ElemNameSpace,MaxDepth,MaxNameSpaces)

The followingCR1000Xexample program shows the use of the XMLParse function to parse an XML file that is downloaded from a remote datalogger (a program for other datalogger models would be similar).

```
'Declare variables
Dim fields_counter'counts the number of fields in our xml file
Public file_handle'file pointer used by FileOpen
Public file_read_bytes'number of bytes given by FileRead when reading XML_FILE_NAME
'Const HTTP_CODE = "HTTP/1.1 200 OK" 'used to determine if we receved the file sucessfully over http. Will need to be updated if HTTP/1.1 changes.
Public http_header As String * 100
Const HTTP_SERVER = "192.168.7.154"'ip address or name of web server
Const MAX_BYTES_READ = 1300'max number of bytes to read from file. Compare file_read_bytes and bump this number up if getting close.
Public record_counter'counts the number of records in our xml file
Public remote_model As String * 10
Public remote_os_version As String * 19
Public remote_station_name As String * 15
Public state'Tells us where are we at in theCampbell ScientificXML file
Dim xml_attribute_name As String * 50
Dim xml_attribute_namespace As String * 100
Public xml_content As String * 5000'RETURN TO DIM after debugging test program
Public xml_data_value_1(10)
Public xml_data_value_2(10)
Dim xml_element_namespace As String * 30
Dim xml_element_name As String * 30
Public xml_fields(2) As String * 64'Dim after testing
Const XML_FILE_NAME = "cpu:test_data.xml"'where the xml is stored we retrieved
Public xml_record_number(10)
Public xml_record_time(10) As String * 20
Public xml_response_code'used to see what XMLParse is returning
Public xml_types(2) As String * 6
Dim xml_value As String * 30

'Return values
Const XML_TOO_MANY_NAMESPACES = -3' Too many name space declarations encountered while parsing an element
Const XML_NESTED_TOO_DEEP = -2' Too many nested XML elements
Const XML_SYNTAX_ERROR_OR_FAILED = -1' XML syntax error or XMLParse failed
Const XML_UNRECOGNIZED_ERROR_CONDITION = 0'Unrecognized error condition
Const XML_START = 1'Start of XML element
Const XML_ATTRIBUTE_READ = 2' XML attribute read.
Const XML_END_OF_ELEMENT = 3' End of XML element
Const XML_END_OF_DOCUMENT = 4' END of XML document encountered. The document has been read/parsed successfully.

'XMLParse max settings
Const XML_MAX_DEPTH = 5'Campbell Scientific's XML format has a maximum nexting depth of 4 elements. So give it one more than is needed in memory.
Const XML_MAX_NAMESPACES = 1' SOAP standard usually has two name spaces minimum. The ones we are looking at only have one (the default namespace).

'State codes we can use to tell use where we are at in theCampbell ScientificXML file
Const STATE_BEFORE_ROOT = 1
Const STATE_IN_ROOT = 2
Const STATE_IN_HEAD = 3
Const STATE_IN_ENVIRONMENT = 4
Const STATE_IN_FIELDS = 5
Const STATE_IN_DATA = 6
Const STATE_IN_RECORD = 7
Const STATE_COMPLETE = 8

'Main Program
BeginProg
http_header = ""'make sure we don't send anything in the header to the web server

'initialize some variables that we use in arrays
fields_counter = 1
record_counter = 1

'program only runs once
Scan (1,sec,0,1)
' get XML from another datalogger (or webserver) with the xml we want to parse using the WebServerAPI (Campbell Scientificdatalogger/webserver)
HTTPGet("http://" + HTTP_SERVER + "/?command=DataQuery&uri=dl:Test&format=xml&mode=most-recent&p1=10", XML_FILE_NAME, http_header)

'copy the the content of the xml file to a string variable we can use to parse against (can also read file directly)
file_handle = FileOpen (XML_FILE_NAME, "r", 0)'open file for reading
file_read_bytes = FileRead( file_handle, xml_content, MAX_BYTES_READ)' read the file into our string
FileClose (file_handle)'close the filehandle to the file we just read

'now parse some data from the xml file and put it in our table for use
xml_response_code = XML_START
xml_element_namespace = "Initialize"'has to be included to get XMLParse to do its thing
state = STATE_BEFORE_ROOT

While xml_response_code > XML_UNRECOGNIZED_ERROR_CONDITION AND xml_response_code <> XML_END_OF_DOCUMENT
xml_response_code =XMLParse( xml_content, xml_value, xml_attribute_name, xml_attribute_namespace, _
xml_element_name, xml_element_namespace, XML_MAX_DEPTH, XML_MAX_NAMESPACES)

'check to see if the XMLParse worked
If xml_response_code = XML_SYNTAX_ERROR_OR_FAILED OR xml_response_code = XML_UNRECOGNIZED_ERROR_CONDITION
'return some error message
EndIf

If xml_response_code = XML_START AND xml_element_name = "csixml"
state = STATE_IN_ROOT
EndIf

If xml_response_code = XML_START AND xml_element_name = "head"
state = STATE_IN_HEAD
EndIf

If xml_response_code = XML_START AND xml_element_name = "environment"
state = STATE_IN_ENVIRONMENT
EndIf

If xml_response_code = XML_START AND xml_element_name = "fields"
state = STATE_IN_FIELDS
EndIf

If xml_response_code = XML_START AND xml_element_name = "data"
state = STATE_IN_DATA
EndIf

If xml_response_code = XML_START AND xml_element_name = "r"
state = STATE_IN_RECORD
EndIf

If xml_response_code = XML_END_OF_ELEMENT AND xml_element_name = "csixml" AND state = STATE_IN_ENVIRONMENT
state = STATE_COMPLETE
EndIf

'get remote station name from XML file
If xml_response_code = XML_END_OF_ELEMENT AND xml_element_name = "station-name" AND state = STATE_IN_ENVIRONMENT
remote_station_name = xml_value
EndIf

'get remote model
If xml_response_code = XML_END_OF_ELEMENT AND xml_element_name = "model" AND state = STATE_IN_ENVIRONMENT
remote_model = xml_value
EndIf

'get remote OS version
If xml_response_code = XML_END_OF_ELEMENT AND xml_element_name = "os-version" AND state = STATE_IN_ENVIRONMENT
remote_os_version = xml_value
EndIf

'get all the field names and data types (there should be 3)
If xml_response_code = XML_ATTRIBUTE_READ AND xml_element_name = "field" AND xml_attribute_name = "name" AND state = STATE_IN_FIELDS
xml_fields(fields_counter) = xml_value
EndIf

If xml_response_code = XML_ATTRIBUTE_READ AND xml_element_name = "field" AND xml_attribute_name = "type" AND state = STATE_IN_FIELDS
xml_types(fields_counter) = xml_value
EndIf

If (xml_fields(fields_counter) <> "") AND (xml_types(fields_counter) <> "") AND (fields_counter <> 2) Then
fields_counter = fields_counter + 1
EndIf

'get the data and timestamps
If xml_response_code = XML_ATTRIBUTE_READ AND xml_element_name = "r" AND xml_attribute_name = "time" AND state = STATE_IN_RECORD
xml_record_time(record_counter) = xml_value
EndIf

If xml_response_code = XML_ATTRIBUTE_READ AND xml_element_name = "r" AND xml_attribute_name = "no" AND state = STATE_IN_RECORD
xml_record_number(record_counter) = xml_value
EndIf

If xml_response_code = XML_END_OF_ELEMENT AND xml_element_name = "v1" AND state = STATE_IN_RECORD
xml_data_value_1(record_counter) = xml_value
EndIf

If xml_response_code = XML_END_OF_ELEMENT AND xml_element_name = "v2" AND state = STATE_IN_RECORD
xml_data_value_2(record_counter) = xml_value
EndIf

If (xml_record_time(record_counter) <> "") AND (xml_data_value_1(record_counter) <> 0) _
AND (xml_data_value_2(record_counter) <> 0) AND (xml_record_number <> "") AND (record_counter <> 10) Then
record_counter = record_counter + 1
EndIf

Wend'end of XMLParse

'delete our xml file now that we are done with it
FileManage( XML_FILE_NAME, 8 )

NextScan
EndProg
```

## Remarks

This function returns one of the following:

| Code | Description                                                                                                                                                                                 |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -3   | Too many name space declarations encountered while parsing element.                                                                                                                         |
| -2   | Too many nested XML elements encountered.                                                                                                                                                   |
| -1   | XML syntax error.                                                                                                                                                                           |
| 0    | Unrecognized error condition.                                                                                                                                                               |
| 1    | Start of XML element. The name of the element is given in ElemName and its namespace URI is given in ElemNameSpace.                                                                         |
| 2    | XML attribute read. The name of the attribute is given in AttrName and its namespace URI is given in AttrNameSpace. The value of the attribute is given in XMLValue.                        |
| 3    | End of XML element. The content of the element, if any, is given in XMLValue. The name and the namespace URI of the element is given, respectively, in ElemName and ElemNameSpace.          |
| 4    | End of the XML document encountered. The document has been successfully parsed. Passing "Finished" into the xml_element_namespace will terminate the parsing sequence and return this code. |
| -4   | An attempt to allocate temporary memory to hold strings larger than 64 characters has failed. "Out of memory" will also appear in the compile results.                                      |

## Parameters

# XMLContent (XML Content)

The XMLContent parameter is the XML content to be parsed. It can represent either a file name (using the format DeviceName:FileName) or it can represent a null terminated string that contains the XML content to be parsed.

Type: String variable or file name

# XMLValue (XML Attribute Value)

The XMLValue parameter returns the value of an XML attribute when a response code of 2 is returned or it can return the value of the element content (otherwise known as its CDATA) when a result of 3 is returned.

Type: Variable

# AttrName (Name of XML Attribute)

The AttrName parameter returns the name of an XML attribute when a result of 2 is returned.

Type: Variable

# AttrNameSpace (Namespace URI of XML Attribute)

The AttrNameSpace parameter returns the namespace URI of an XML attribute when a result of 2 is returned.

Type: Variable

# ElemName (XML Element Name)

The ElemName parameter returns the name of an XML element when a result of 2, 3, or 4 is returned.

Type: Variable

# ElemNameSpace (Namespace URI of XML Element)

The ElemNameSpace parameter returns the namespace URI of an XML element when a result of 2, 3, or 4 is returned.

Type: Variable

# MaxDepth (Max Nesting Depth)

The MaxDepth parameter controls, in part, the amount of memory that will be allocated for the parser. This value must be at least one greater than the anticipated maximum nesting depth of XML elements. For example, the CSIXML format, which has a maximum nesting depth of 4 elements, would require a minimum of 5 for this value. If too small of a value is specified, the parser may return a value of -2 while parsing the document.

Type: Constant

# MaxNameSpaces (Maximum Number of Namespaces)

The MaxNameSpaces parameter controls, in part, the amount of memory that will be allocated for the parser. This value must be greater than equal to one or the maximum number of namespaces that are anticipated to be in the document. For instance any document that is exchanged using the SOAP protocol will likely have at least two namespaces. If too small a value is specified, the parser will return a value of -3 while parsing the document.

Type: Constant
