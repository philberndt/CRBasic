# RangeCode (Range Specifier Code)

Specifies the Range Specifier Code in the Qualifier Field of the DNP3 Application Layer. This parameter is used to define whether all data of the specified object and variation will be requested, or only certain index points. A RangeCode of 6 signifies that no range field is used and implies that all values are requested.

| RangeCode | Description                             |
| --------- | --------------------------------------- |
| 0         | 1-Octet start-stop indexes              |
| 1         | 2-Octet start-stop indexes              |
| 2         | 4-Octet start-stop indexes              |
| 6         | No range field used; implies all values |
