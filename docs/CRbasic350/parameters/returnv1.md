# ReturnV1

Optional parameter that determines whether or not to return the V1 voltage as shown in the following image. If ReturnV1 is non-zero, V1 will be returned. In this case, the Destination parameter must be a two-element (or greater) array. The first element will hold V2/V1 (the normal output for this instruction). The second element will hold V1.

| Value | State            |
| ----- | ---------------- |
| 0     | Do not return V1 |
| ≠0    | Return V1        |
