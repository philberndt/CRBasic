# MaxRecords (Maximum Records to Retrieve)

Optional parameter.

If MaxRecords is set to a value less than 0, GetDataRecord will collect up to MaxRecords of uncollected data, beginning with the oldest missing record. If MaxRecords is greater than 0, the instruction will collect up to MaxRecords of uncollected data starting with the most recent record (and then going back and filling in the gaps or "holes"). The default value is 1, which collects only the most recent record. 1 is assumed if the parameter is not used.

Type: Constant
