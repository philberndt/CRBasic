# Class_List

A quoted string containing the list of classification codes separated by commas. If using arrays and insufficient entries exist in the quoted string, all values not specified will inherit the class of the last one in the list. See [Measurement Classifications](https://www.campbell-cloud.com/classifications) for a list of classification codes. (This list can also be found by selecting **Measurement Classifications** from the **Help** menu in CampbellCloud.

Example:

DataTable(Hourly,True,-1)
Sample(2,TempC,IEEE4)
FieldNames("AirTemp,SoilTemp")
`FieldClassify`("&01010101,&01040101")
EndTable

01010101 is the classification code for Air temperature, degrees C and 01040101 is the classification code for soil temperature, degrees C, as shown in the **Measurements Classification** table:
