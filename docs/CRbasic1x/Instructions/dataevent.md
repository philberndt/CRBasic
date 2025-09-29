# DataEvent (Data Event)

The DataEvent instruction is used to conditionally start and stop storing data to a DataTable. Trigger events can be specified to determine when data storage begins and when data storage ends. Additionally, a number of records to store before and/or after the event can be specified.

## Syntax

DataEvent(RecBefore,StartTrig,EndTrig,RecAfter)

In the following example, setting Flag high begins a loop through the program where the variable I is incremented each pass through the loop. When I = 7, the data storage event is triggered. The datalogger backs up three records and begins storing data until I = 11, after which, it stores 6 more records to complete the data storage event.

```
'Declare Variables
```

Public Flag as Boolean
Public I
Public MainBat

DataTable(TstEvent,1,1000)'1000 record long table
DataInterval(0,0,Sec,10)'DataInterval with lapses at least 1 reduces processing time, 'even if no interval is specified
DataEvent(3,I = 7,I = 11,6)'Start when I = 7, stop when I = 11
Sample(1,I,FP2)'Store the value of I
Sample(1,MainBat,FP2)'Store the battery voltage
EndTable

BeginProg'Start of program
Do'Infinite do loop
I = 0 'Reset I to 0
Flag = False'Set Flag false
Do While (Not Flag)
Loop'Wait here until Flag(1) is set high
Scan (1,Sec,0,60)'Scan once per second for 1 minute
CallTable TstEvent'Run DataTable
Battery (MainBat)
I = I + 1'Increment I on each scan
NextScan'Start scan again
Loop'Infinite do loop
EndProg'End of program

```

## Remarks


When the DataEvent instruction is in a program, the datalogger sets up a buffer the size of the RecBefore parameter times the number of data values to be output. Any statistical outputs in the data table (for example, average, maximum, or total), will include values from this buffer. If data is stored to a data table from multiple data events, a filemark is stored in the data table between each data storage event.

**NOTE:** Conditionally called data tables generally hold fewer records than time-interval driven data tables. Therefore, to avoid wasting data-storage memory, best practice is to use a fixed number of records for the size parameter of conditional tables, rather than using auto-allocate. If auto-allocate is used for the size of a conditional table, memory allocation will assume that the condition is always met, and a larger portion of memory will be allocated for the conditional table than is necessary.

If [Tablefile](tablefile.md) with option 64 is used with DataEvent, then 3 times the Tablefile Interval (or Numrecs) of memory is pre-allocated for the Tablefile.

**NOTE:** Data Event with Tablefile option 64 is fully implemented inCR1000X OS4and greater.

Other options of Tablefile do not pre-allocate memory. Rather, a Tablefile is created at the baling interval (time or number of records).

**NOTE:** For non-option 64 Tablefiles, the processing time required for writing new files increases as the number of files increases due to file system overhead when creating a new file. In other words, it takes a long time for the datalogger file system to read through numerous files and determine how much room is left for new files. Hence, best practice is to limit the number of tablefiles stored to 1000 or less.

**NOTE:** To reduce processing time, the [DataInterval](datainterval.md) instruction, with at least 1 lapse, should always precede the DataEvent instruction.

[CardOut](cardout.md) is an alternative to Tablefile.


## Parameters



# RecBefore (Records to Store before Triggering Data Storage Event)


The RecBefore parameter is used to define the number of records to be stored in the DataTable prior to when the data storage event was triggered.

Type -- Constant


# StartTrig (Start Data Storage Event)


The StartTrig is aconstant

**Note:** A non-varying fixed number.

, variable, orexpressionto be evaluated for starting the data storage event. Data storage begins when this expression evaluates as True or not equal to 0. StartTrig can be any legal expression, such as TBlk1(2) > 72. In this case, when the variable TBlk1(2) contains a value greater than 72 the StartTrig argument is true and the data storage event starts.

Type -- Constant, Variable, or Expression


# EndTrig (End Data Storage Event)


The EndTrig is the variable, expression, or constant to be evaluated for stopping the data storage event. If a non-zero constant is entered, the number of records stored equals RecBefore + 1 + RecAfter. If 0 is entered, once the data storage event begins it is never stopped.

Type -- Constant, Variable, or Expression


# RecAfter (Records to Store after Triggering Data Storage Event)


The RecAfter parameter is used to define the number of records to be stored in the DataTable after the data storage event is stopped.If the RecAfter parameter is negated (for example, -N where N is the number of records), then a new event occurring during the RecAfter interval will stop the RecAfter count and begin a new DataEvent.

Type Constant
```
