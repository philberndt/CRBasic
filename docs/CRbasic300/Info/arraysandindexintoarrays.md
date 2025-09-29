# Arrays and Indexes into Arrays

Programming languages vary in how they treat references to arrays and indexes into arrays. CRBasic observes the following rules:

**NOTE:** In this instance, an "object" is the channel for a measurement instruction, a variable for an output instruction, an alias assigned to a variable, etc.

| Variable Used | Result                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------- |
| Mult          | Instruction starts with the first variable, Mult(1). Any reps occur on subsequent objects.   |
| Mult( )       | Instruction starts with the first variable, Mult(1). Any reps occur on subsequent objects.   |
| Mult(1)       | Instruction starts with the first variable, Mult(1). Any reps occur on subsequent objects.   |
| Mult(n)       | Instruction starts with n index of the variable array. Any reps occur on subsequent objects. |

The rules used for assigning values to multipliers and offsets used in conjunction with repetitions are different. For additional information, see [Multipliers, Offsets, and Disable Variables with Repetitions](multipliersoffsets.md).

[See alsoMulti-dimensional Arrays.](multidimensionalarrays.md)
