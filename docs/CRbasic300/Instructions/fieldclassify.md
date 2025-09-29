# FieldClassify (Specify Measurement Classification of a Field)

FieldClassify is specifically used when sending data to CampbellCloud via MQTT. It designates the measurement classification of a given field using a hex string reference.

## Syntax

```
FieldClassify
```

(Class_List)

```
Const SCANRATE = 10
'Declare Variables and Units
Public CS320(4)
Alias CS320(1)=Irradiance
Units Irradiance=W/m^2
Alias CS320(2)=Vout
Units Vout=mV
Alias CS320(3)=Temp
Units Temp=DegC
Alias CS320(4)=Tilt
Units Tilt=Deg

'FieldClassify must be placed within the DataTable, immediately following the output instruction for the field being classified.
```

'Define Data Tables
DataTable(FifteenMin,True,-1)
DataInterval(0,15,Min,10)
Sample (1,Irradiance,IEEE4)
FieldClassify("&H90000101")'example of instruction with 1 rep
Sample (1,Temp,IEEE4)
FieldClassify("&H010a0101")
EndTable

DataTable(Hourly,True,-1)
DataInterval(0,60,Min,10)
Sample (4,CS320(),IEEE4)'example of instruction with reps of 4
FieldClassify("&H90000101,&H1b040201,&H010a0101,&H24010101")
EndTable

'Main Program
BeginProg
'Main Scan
Scan(SCANRATE,Sec,2,0)
SDI12Recorder(CS320(),C1,"0","M4!",1,0,-1)

'Call tables
CallTable FifteenMin
CallTable Hourly

```
NextScan
EndProg
```

## Remarks

FieldClassify is used to specify the measurement classification of a field using a hex string reference. This reference sets the Classification, Sub-Classification, Units, and Aggregate Type, all of which are defined by Campbell Scientific. No customer-specific classifications are allowed. Selecting the '?' option in the CampbellCloud UI gives the user access to a [Measurement Classifications](https://www.campbell-cloud.com/classifications) page, which lists available classification codes.

A string of zeros represents an 'unclassified' status (the default setting). If the classification hex string is not recognized, it is ignored, and the classification defaults to zeros. The FieldClassify instruction must be placed within the DataTable declaration, immediately following the output instruction for the fields being classified.

## Parameters

# Class_List

A quoted string containing the list of classification codes separated by commas. If using arrays and insufficient entries exist in the quoted string, all values not specified will inherit the class of the last one in the list. See [Measurement Classifications](https://www.campbell-cloud.com/classifications) for a list of classification codes. (This list can also be found by selecting **Measurement Classifications** from the **Help** menu in CampbellCloud.

Example:

DataTable(Hourly,True,-1)
Sample(2,TempC,IEEE4)
FieldNames("AirTemp,SoilTemp")
`FieldClassify`("&01010101,&01040101")
EndTable

01010101 is the classification code for Air temperature, degrees C and 01040101 is the classification code for soil temperature, degrees C, as shown in the **Measurements Classification** table:
