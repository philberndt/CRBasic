# "Authen" (Email Authentication Type)

A string specifying the type of authentication to be used when logging in to the email server. The datalogger supports APOP, Plain, andTLS

**Note:** Transport Layer Security. An Internet communication security protocol.

authentication. APOP is recommended if it is supported by the mail server. APOP uses theMD5

**Note:** 16 byte checksum of the TCP/IP VTP configuration.

hash function to encrypt the user name and password. Gmail requires TLS.

Type: String, "APOP", "Plain", or "TLS"
