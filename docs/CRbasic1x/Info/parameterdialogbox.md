# Working with Parameters

The Parameter dialog box will be displayed on the screen when an instruction that has one or more parameters is added to a program or when the cursor is placed on a n existing instruction and the right mouse button is pressed. This dialog box contains a field for each of the parameters in the instruction. Edit these fields as necessary and then click the **Insert** button to paste the instruction into the program.

When the Parameter dialog box is opened, you can move to the next instruction in the program by clicking the **Next** button at the bottom of the dialog box (or click **Prev** to move back to the previous instruction). If a change has been made in the current instruction, you are asked whether to save or discard the change before moving to the next instruction.

Following is an example of the Parameter dialog box for the differential voltage instruction ([VoltDiff](../Instructions/voltdiff.md)).

(Click image to expand/collapse display)

## Parameter Editing Shortcuts

Right-click or press F2 on a parameter that uses a Variable as an input type to display a list of variables that have been defined in the program. A sample list:

The variable list is sorted by variable type and then alphabetically by name. In the previous sample list, the first green A denotes that the variable `AirTemp3ft` is set up as an [Alias](../Instructions/alias.md).[Constants](../Instructions/const1.md) are listed with a blue C,[Dimensioned](../Instructions/dim.md) variables are listed with a red D, and [Public](../Instructions/public.md) variables are listed with a black P. Variables that are Dimensioned within a subroutine or function, and thus local variables, are listed with a purple L.

At any time you can press F10 to display the list of variables, regardless of the input type for the selected parameter. Also, defined variables can be selected from the Variables drop-down list box at the upper right of the Parameter dialog box.

- Right-click or press F2 on a parameter that has a finite number of valid entries to display a list of available options.

- Right-click or press F2 on a parameter

## Help

Press the **Help** button at the bottom right of the Parameter dialog box to display detailed help topic for the instruction being edited. Press F1 when your cursor is within a parameter field to display help only on that parameter. Some fields also have text in the Comments column which provides a short description of the option that has been selected for the parameter.[SeeAccessing the Help Systemfor more information.](accessingthehelpsystem.md)

## Changing Default Parameter Values for an Instruction

Each instruction is programmed with default values for each field. For instance, in the previously displayed Parameter box, the default for the Range is mV5000. If you wanted to edit this so that each time you inserted the `VoltDiff` instruction, the Range value defaulted to mV1000, highlight the instruction in the Instruction Panel, click the **Instruction** menu, select **Edit Instruction Defaults**, and make the change in the resulting dialog box.
