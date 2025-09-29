# Signature (Program Code Signature)

The Signature function returns the signature for program code in a datalogger program.

## Syntax

Signature

```
'code for which to calculate signature
```

_variable_ = Signature

The following program shows the use of the Signature function to return a unique signature for two portions of the program code.

```
Public Sig(2), RefTemp, TCTemp, Counter AS Long

BeginProg
Scan (1,Sec,3,0)
Signature
PanelTemp (RefTemp,15000)
TCDiff (TCTemp,1,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

Sig(1) =Signature
NextScan

SlowSequence
Signature
Do
Counter=Counter+1
If Counter = 99999 Then Counter = 0
Sig(2)=Signature
Loop
EndProg
```

## Remarks

The Signature function returns a number between 0 and 65535 that is a Campbell Scientific pseudo random signature of the program code and then reinitializes the signature seed to &HAAAA (43690). Signature must occur within [BeginProg/EndProg](beginprogendprog.md) statements, or within a subroutine. The portion of code for which to return a signature is bracketed by two signature declarations. This function is not used until it is called in the program; therefore, the first use will always return 43690. More than one Signature function can be used in the program. This function allows a way to detect whether or not portions of the program code have changed. Blank lines and comments are not included in the Signature.
