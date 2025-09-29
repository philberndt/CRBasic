# FieldOrigin (Specify Origin of a Field)

FieldOrigin is used to specify the measurement origin of a field using a string reference. FieldOrigin is specifically used when sending data to CampbellCloud via MQTT.

FieldOrigin(Origin_Description)

The following 2 examples demonstrate using FieldOrigin in a data table.

Table example 1:

```
DataTable(15Min,True,-1)
DataInterval(0,15,Min,10)
```

Sample(3,Count,IEEE4)
FieldNames("MyCount1,MyCount2,MyCount3")
FieldClassify("1,&H201","123")
'Three separate field origin descriptions, separated by commas
FieldOrigin("Diff1,Diff2,SE1")

```
EndTable
```

Table example 2:

```
DataTable(15Min,True,-1)
Sample(1,Test,IEEE4)
FieldNames("MyTest")
FieldClassify("&H8675309","123")
'One field origin description with three levels of identification separated by colons(port:device:channel)
FieldOrigin("CPI12:Volt116:Diff1")
EndTable
```

Follwing is a full example program:

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

'FieldClassify and FieldOrigin must be placed within the DataTable, immediately following the output instruction for the field being classified.
```

'Define Data Tables
DataTable(15Min,True,-1)
DataInterval(0,15,Min,10)
Sample (1,Irradiance,IEEE4)
FieldClassify("&H90000101"):FieldOrigin("C1:SDI12:1")'example of sample with 1 rep
Sample (1,Temp,IEEE4)
FieldClassify("&H010a0101"):FieldOrigin("C1:SDI12:3")'example of sample with 1 rep
EndTable

DataTable(Hourly,True,-1)
DataInterval(0,60,Min,10)
Sample (4,CS320(),IEEE4)'example of sample with 4 reps
FieldClassify("&H90000101,&H1b040201,&H010a0101,&H24010101"):FieldOrigin("C1:SDI12:1,C1:SDI12:2,C1:SDI12:3,C1:SDI12:4")
EndTable

'Main Program
BeginProg
'Main Scan
Scan(SCANRATE,Sec,2,0)
SDI12Recorder(CS320(),C1,"0","M4!",1,0,-1)

'Call tables
CallTable 15Min
CallTable Hourly

```
NextScan
EndProg
```

## Remarks

The FieldOrigin instruction accepts a quoted string with elements separated by commas. If using arrays and insufficient entries exist in the quoted string, all values not specified will inherit the origin specified for the last array element in the list. The FieldOrigin instruction must be placed within the DataTable declaration, immediately following the output instruction for the fields being described.

## Parameters

# Origin_Description

A quoted string containing a comma-separated list of field origin descriptors. Colons can be used to indicate hierarchical levels within the same field origin. When using arrays, if the quoted string lacks sufficient entries, any unspecified values will inherit the origin of the last specified entry.
