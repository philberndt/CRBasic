# Output Format

Specifies the format for the published data.

| Number | Description                           |
| ------ | ------------------------------------- |
| 1      | CSIJSON                               |
| 2      | Campbell ScientificVersion of GeoJSON |

**NOTE:** Note: If publishing to CampbellCloud, use output format 2, GeoJSON.

#### CSIJSON Output Format

```
{
"head" : {
"transaction" : 0,
"signature" : 49005,
"environment" : {
"station_name" : "321",
"table_name" : "panelTempC",
"model" : "CR1000X",
"serial_no" : "321",
"os_version" : "CR1000X.02.00.2019.02.27.1326",
"prog_name" : "CPU:mqttPub24.CR1X"
},
"fields" : [ {
"name" : "crTemp",
"type" : "xsd:float",
"process" : "Smp",
"settable" : false
} ]
},
"data" : [ {
"time" : "2019-02-28T18:45:00",
"vals" : [ 24.73 ]
}]
```

}

```
Type: Constant


#### GeoJSON Output Format


```

{
"type": "Feature",
"geometry": {
"type": "Point",
"coordinates": [ "12345.000000", "23456.000000", "345.000000" ]
},
"Properties": {
"loggerID": "CR1000X_400",
"observationNames": ["batteryVolt", "batteryVolt_Avg", "crTemp"],
"observations": {
"2019-02-28T19:00:00.0Z": [13.67, 13.67, 24.31]
}
}
}

```
For details, see [GeoJSON format details](../Instructions/mqtt_geojson_format.md).
```
