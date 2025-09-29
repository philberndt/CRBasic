# DialString

The telephone number and any other codes used to dial the modem. A semi-colon is used to separate multiple commands. Two semi-colons in a row will insert a 1 second delay before continuing to the next characters in the string. If only a phone number is used for the DialString parameter the datalogger will automatically issue an ATDT prior to issuing the number. If an AT command is added to the DialString parameter (e.g., ATV1ATDT) the automatic ATDT is not issued.

Type: String
