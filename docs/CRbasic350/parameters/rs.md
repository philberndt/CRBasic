# Rs (Solar Radiation)

The variable in which the solar radiation measurement is stored that will be used for the ETsz calculation. The solar radiation measurement must be scaled to give total MegaJoules per meter squared (MJ m-2) received during the period between calling the table with the ETsz instruction in it (this is normally the scan interval). If the sensor is scaled to output flux; for example Wm-2, this would need to be multiplied by the scan interval in seconds and a multiplier applied to convert to MJ; for example, 10-6to convert from W.

Type: Variable
