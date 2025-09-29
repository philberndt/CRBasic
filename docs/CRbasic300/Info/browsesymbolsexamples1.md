# BrowseSymbols Examples

Return all tables in datalogger:

http://192.168.4.14/?command = browsesymbols&uri = dl:&format = html

or

http://192.168.4.14/?command = browsesymbols&format = html

(if the URI is not specified, it is assumed to be the top-level device, which in this instance is the datalogger)

Return all fields in the Public table:

http://192.168.4.14/?command = browsesymbols&uri = dl:public&format = html

Return the indexes in the Flag array, which is part of the Public table:

http://192.168.4.14/?command = browsesymbols&uri = dl:public.flag&format = html
