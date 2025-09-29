# Compound Operators

Compound operators are a short hand way to set a variable equal to an expression that includes itself. For example, X += 2 is the same as saying X is equal to itself plus two, or X = X +2.

X /= Y is the same as saying X is equal to itself divided by Y, or X = X/Y.

### Example Program:

```
Public X, Y = 1'declare variables

BeginProg
Scan (10,Sec,3,0)'scan interval of 10 seconds
X+=1'X is equal itself plus one
Y*=2'Y is equal itself times two
NextScan
EndProg
```
