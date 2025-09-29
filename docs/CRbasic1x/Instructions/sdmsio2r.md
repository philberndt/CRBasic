# SDMSIO2R (SDM-SIO2R Serial I/O Module with Relays)

The SDMSIO2R() instruction is used to interface with the internal relays of the SDM-SIO2R, which can switch power or be used for signal control. This instruction does not perform any serial communications. To use the SDM-SIO2R for serial sensor communications (RS-232 or RS-485), refer to the [SerialOpen](serialopen.md),[SerialIn](serialin.md),[SerialFlush](serialflush.md), and [SerialOut](serialout.md) commands.

## Syntax

SDMSIO2R([SDMAddress](../parameters/sdmaddress-sio2r.md),[Mask](../parameters/mask_sdmsio2r.md),[Vp](../parameters/vp.md),[V12](../parameters/v12.md))

**This example program demonstrates using the SDMSIO2R() instruction to control the relays in a SDM-SIO2R**.

```
'Declare constants
```

`Const`SetVp_Open = 0
`Const`SetV12_Open = 0
`Const`SetVp_Closed = 1
`Const`SetV12_Closed = 1
`Const`Mask_Write_Both = 3
`Const`Mask_Read_Both = 7
`Const`address = 0

'Declare Variables
`Public`GetVp`As Long`, GetV12`As Long`, Vp_Relay_State`As String`, V12_Relay_State`As String`

`BeginProg`

`Scan`(7,Sec,0,0)

'Set Both Relays Closed. Should see both LED's ON to indicate relays are closed
`SDMSIO2R`(address, Mask_Write_Both, SetVp_Closed, SetV12_Closed)'write V+ and 12V relays closed

`SDMSIO2R`(address, Mask_Read_Both, GetVp, GetV12)'Read state of V+ & 12V relays.

```
'If Vp is 0 then its open, If Vp is 1 then its closed
```

`If`GetVp = 0`Then`Vp_Relay_State ="Open"`Else`Vp_Relay_State ="Closed"

'

```
If V12 is 0 then its open, If V12 is 1 then its closed
```

`If`GetV12 = 0`Then`V12_Relay_State ="Open"`Else`V12_Relay_State = "Closed"

Delay (1,3,Sec)

'Set Both Relays Open. Should see both LED's OFF to indicate relays are open
`SDMSIO2R`(address, Mask_Write_Both, SetVp_Open, SetV12_Open)' Write V+ and 12V relays to open

`SDMSIO2R`(address, Mask_Read_Both, GetVp, GetV12)''Read state of V+ & 12V relays

'

```
If Vp is 0 then its open, If Vp is 1 then its closed
```

`If`GetVp = 0`Then`Vp_Relay_State ="Open"`Else`Vp_Relay_State= "Closed"

'

```
If V12 is 0 then its open, If V12 is 1 then its closed
```

`If`GetV12 = 0`Then`V12_Relay_State ="Open"`Else`V12_Relay_State ="Closed"

Delay (1,3,Sec)

EndProg

## Remarks

The SDMSIO2R() instruction is used to write or read the state of the internal relay on the SDM-SIO2R. The Vp parameter allows for V+ input to be connected to V+ output, and the V12 parameter allows the 12V input to be connected to 12V output. V+ and 12V can be controlled completely independent of each other. A maximum of 5A of current can flow through each relay. Two LED s on the SDM-SIO2R indicate whether the relay is open or closed and correspond to V+ and 12V. The LED on indicates the relay is closed (power could be present at V+ or 12V output) and LED off indicates the relay is open.

## Parameters

The parameters all accept variables. To perform reads, Vp and V12 must be variables. Since the Mask is also a variable, there is no compile-time checking to verify the parameters in the Vp and V12 parameters can be written.

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# Mask

The Mask parameter determines whether Vp & V12 values are written to SIO2R or read from SIO2R. Mask also determines if reading/writing to both Vp & V12 or one or the other.

| Mask | Action                                                                                              |
| ---- | --------------------------------------------------------------------------------------------------- |
| 0    | No action will be taken.                                                                            |
| 1    | Write the value of the Vp parameter to the V+ relay; 0 is open; non-zero is closed.                 |
| 2    | Write the value of the V12 parameter to 12V terminal ; 0 is off, non-zero is on.                    |
| 3    | Write the values of both Vp & V12 to their respective relays.                                       |
| 5    | Read the state of the V+ relays into the variable Vp; 0 means it was open, 1 means it was closed.   |
| 6    | Read the state of the 12V relays into the variable V12; 0 means it was open, 1 means it was closed. |
| 7    | Read the state of both V+ and 12V relays into their respective variables                            |

Type: Constant or Variable of Type Long

# Vp

The Vp parameter can be used to either write the state of the V+ relay or read the state of the V+ relay (opened or closed).

With the mask parameter set to Write, writing a 0 in the Vp parameter opens the V+ relay. Writing a 1 to the Vp parameter closes the V+ relay. The closed relay connects the V+ input to V+ output and is capable of passing up to 5A of current.

With the mask set to Read, reading a 0 from the Vp parameter means the SIO2R is commanding the V+ relay to be open. Reading a 1 from the Vp parameter means the relay is being commanded to be closed. Note: This only reads the commanded state not the actual state. The relay contacts could permanently fail in closed or opened positions.

Type: Constant or Variable of Type Long

# V12

V12 parameter can be used to either write the state of the 12V relay or read the state of the 12V relay (opened or closed).

With the mask parameter set to Write, writing a 0 in the 12V parameter opens the 12V relay. Writing a 1 to the Vp parameter closes the 12V relay. The closed relay connects the 12V input to 12V output and is capable of passing up to 5A of current.

With the mask set to Read, reading a 0 from the 12V parameter means the SIO2R is commanding the 12V relay to be open. Reading a 1 from the 12V parameter means the relay is being commanded to be closed. Note: This only reads the commanded state not the actual state. The relay contacts could permanently fail in closed or opened positions.

Type: Constant or Variable of Type Long
