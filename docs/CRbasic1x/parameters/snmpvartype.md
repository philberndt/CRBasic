# Type

The data type of the managed object. Type can be Integer, Counter, String, TimeTicks, Opaque, Gauge, or Float. If this parameter is omitted, the object assumes the data type of the variable declared in the CRBasic program. If a variable has not been declared, the default is Integer. If the type is Float, the OID is advertised as a String and conversion takes place automatically (since SNMP does not natively support a type of Float).

Type: String
