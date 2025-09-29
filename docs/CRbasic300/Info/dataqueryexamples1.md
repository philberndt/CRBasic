# DataQuery Examples

In the following examples,`WSN30Sec`is the table name and`CWS900_Ts`is the fieldname.

Five most recent records from a data table (WSN30sec), CWS900_Ts field (most-recent):

http://192.168.4.14/?command = dataquery&uri = dl:WSN30sec.CWS900_Ts&format = html&mode = most-recent&p1 = 5

All records since a specific date (since-time):

http://192.168.4.14/?command = dataquery&uri = dl:WSN30sec.CWS900_Ts&format = html&mode = since-time&p1 = 2010-07-26

All records between two dates/times (date-range):

http://192.168.4.14/?command = dataquery&uri = dl:WSN30sec.CWS900_Ts&format = html&mode = date-range&p1 = 2010-07-27T12:00:00&p2 = 2010-07-27T14:00:00

All records since a specific time (since-time):

http://192.168.4.14/?command = dataquery&uri = dl:WSN30sec.CWS900_Ts&format = html&mode = since-time&p1 = 2010-07-27T12:00:00

All records since a specific record (since-record):

http://192.168.4.14/?command = dataquery&uri = dl:WSN30sec.CWS900_Ts&format = html&mode = since-record&p1 = 14400

All records since one hour ago (backfill):

http://192.168.4.14/?command = dataquery&uri = dl:WSN30sec.CWS900_Ts&format = html&mode = backfill&p1 = 3600
