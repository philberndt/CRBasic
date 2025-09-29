# TimerNo (Timer Number)

A number assigned to the timer (for example, 0, 1, 2, ). Memory is allocated for timers based on the value entered + 1 (for example, using a TimerNo of 100 will allocate memory for 101 timers even if there is only one Timer in the program). Use a low number to conserve memory.

If this value is a variable, memory for 17 (0 - 16) timers is allocated. If more than 17 timers are needed, and TimerNo also needs to be a variable, use a "dummy" Timer instruction, which does not execute at run-time, with a TimerNo of the maximum number of timers needed in the program.

Type: Integer or Variable
