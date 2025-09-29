# OID

The object identifier, represented using dot notation; e.g., 1.3.6.1.4.1.111.4.1.1. If the variable is an array, elements of that array are accessed using the syntax root.element (where root is the OID and element is the element number of the array; e.g., 1.3.6.1.4.1.111.4.1.1.2 specifies element number 2 of the object ID 1.3.6.1.4.1.111.4.1.1 variable).

In addition to the user defined OIDs, the following OIDs are supported in the datalogger:

- 1.3.6.1.2.1.1.1 sysDescr

- 1.3.6.1.2.1.1.2 sysObjectID

- 1.3.6.1.2.1.1.3 sysUpTime

- 1.3.6.1.2.1.1.4 sysContact

- 1.3.6.1.2.1.1.5 sysName

- 1.3.6.1.2.1.1.6 sysLocation

- 1.3.6.1.2.1.1.7 sysServices

When [ESSVariables](../Instructions/essvariables.md) and SNMPVariable are used in the same program, these user-defined OIDs must begin with an identifier of 1.3.6.1.4.1.1207 or greater (the last OID in the datalogger OS structure for ESSVariables is 1.3.6.1.4.1.1206).

Type: Constant
