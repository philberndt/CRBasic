# AbortExp

Used to abort the callback attempt and stop any further attempts. If AbortExp is a variable the datalogger will immediately set its AbortExp parameter to True when a callback attempt succeeds. The instruction will not execute when AbortExp is True, and it will abort in the middle of dialing if the expression turns True.

Type: Variable or expression
