# PingOption

Optional parameter that specifies the IP version of the address to return if the address is a domain name that needs to be resolved to a numeric IP address. The optional parameter may be configured as follows:

| Option Code | Description                       |
| ----------- | --------------------------------- |
| 0 or absent | Ping IPv4 first, if fail try IPv6 |
| 1           | Ping only IPv4                    |
| 2           | Ping only IPv6 (local and global) |
| 3           | Ping IPv6 first, if fail try IPv4 |
