# ArgosError (Send Argos Error Message)

The ArgosError instruction sends a "Get and Clear Error Message" command to the transmitter.

## Syntax

ArgosError(ErrorCodes)

Following is a program sample showing the use of ArgosError(). The string Errors will hold a text string indicating the most recent error, if any, in communications between the ST-20 and datalogger.

```
Public Errors as string * 29

BeginProg
Scan (10,Sec,3,0)
ArgosError(Errors)
NextScan
EndProg
```

## Remarks

This instruction sends the &H09 command and then waits for the returned string plus the ACK or NAK character. The transmitter replaces the error message with an empty string after responding to the command.

## Parameter

# ErrorMessage (Returned Error Message)

A variable, declared as a string, that holds the returned error message from the transmitter. Returned error messages include:

- Bad Argument

- Bad Buffer

- Bad Command

- No Failsafe Mode

- Timeout

- Transmit Failure

Type: String Variable
