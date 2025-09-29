# GeoJSON format details

CSI GEOJSON is an adaptation of the GEOJSON open standard data format. This format is less verbose than that CSI-JSON format and allows for multiple observations to be compactly transmitted in a single JSON payload.

In the CSI version of GeoJSON the observationNames are in an array which match fields stored in data tables. The corresponding location in the array is the same location in the values array where the observation value is located.

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

In the above example batteryVolt is in observationNames[0] position so the batteryVolt measurement value will be in {timestamp}:[0] location.

## GeoJSON Format

```
{
"type": "Feature",
"geometry": {
"type": "Point",
"coordinates": [
"<Longitude>",
"<Latitude>",
"<Altitude>"
]
},
"Properties": {
"loggerID": "Device Config setting value which identifies device (EX: CR1000X_400)",
"observationNames": [
"<table field name 1>",
"<table filed name 2>",
"<table field name 3>",
.
.
.
"<table field name k>"
],
"observations": {
"timeStamp 1": [
value 1,
value 2,
value 3,
.
.
.
value k
],
"timeStamp 2": [
value 1,
value 2,
value 3,
.
.
.
value k
],
.
.
.
"timeStamp n": [
value 1,
value 2,
value 3,
.
.
.
value k
],
}
}
}
```
