# Include File

The **Include File** setting is a datalogger setting that specifies the name of a file to be included at the end of a CRBasic program when it is run, or it can be run as the default program. The setting is available in GRANITE-series, CR6, CR3000, CR1000X, CR800-series, CR300-series, and CR1000 dataloggers. It is set up from the **Settings Editor** tab inDevice Configuration Utility

**Note:** Configuration tool used to set up dataloggers and peripherals, and to configure PakBus settings before those devices are deployed in the field and/or added to networks.

.

The rules used by the datalogger when a new program is compiled or when the datalogger is powered up are:

1. When the datalogger first starts, it will execute any commands found in the powerup.ini file. (The powerup.ini file is stored on a CRD or USB drive; the datalogger checks the CRD first and if a file is found, it is used. Refer to the datalogger manual for more information on the powerup.ini file.) This may include a command to set the **Run Now** or **Run On Power-up** attributes for a program.
2. If the datalogger is starting from power-up, any file that is marked as **Run On Power-up** will be the program that is run. If there is a file specified by the **Include File** setting, it will be incorporated into the program that is run.
3. If there is no program file marked **Run On Power-up**(or if the program cannot be compiled), the datalogger will run the program specified by the ** Include File**setting. Note that the datalogger will run a program without a`BeginProg()`instruction if it includes a`SlowSequence()`. This feature allows the specified **Include File** to be used as code included in another program or as a program run on its own.
4. If the **Include File** program cannot be run or if no program is specified for this setting, the datalogger will attempt to run the program named default.CRBif one exists.
5. If no default.CRBfile can be found or compiled, the datalogger will not run any program.

The **Include File** can be useful when certain settings are required for communication (such as an open Serial or TCPIP port), where sending a new program might cause communication to be disabled if the program send fails.

**NOTE:** The **Include File** setting should not be confused with the`Include()`instruction. The`Include()`instruction is used within a program to incorporate additional code into a program, but it does not affect other programs that may be run in the datalogger.

**NOTE:** The datalogger attempts to incorporate the **Include File** into any program that is run in the datalogger. Thus, care should be taken to avoid duplicate variable names, and in most instances, the code should be placed within a`SlowSequence()`. If it is placed within a`SlowSequence()`, there will be no`BeginProg()/EndProg()`instructions to interfere with another program, but it will also run if executed on its own.

**NOTE:** When sending an operating system to a datalogger viaCampbell Scientificsoftware **Connect** window, the running program is stopped, the operating system is loaded into memory, and when the entire operating system is received it is compiled. If an **Include File** or a default.CRBfile resides on a datalogger drive, when the running program is stopped the **Include File** or default.CRBprogram will begin running. It will stop running when the operating system begins compiling and will resume running after compilation is complete. An **Include File** or default.CRBfile should not contain instructions that use a substantial amount of memory (e.g., a large program), since this reduction in memory could result in insufficient memory for storing the operating system prior to compilation. Consideration should also be given to ensure that other instructions in the program do not have unintended effects.
