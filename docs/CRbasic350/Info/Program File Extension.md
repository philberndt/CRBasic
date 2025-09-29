# Program File Extension

| The CRBasic Editor displays instruction lists and help files based on the type of datalogger file opened. The compiler used when a program is compiled is also based on the datalogger type. Many dataloggers support a specific datalogger extension (e.g., \*.CR1X or \*.CR6). However, if a program file has a generic extension (e.g., .DLD or .CRB), the datalogger type must be selected in the '[Set Datalogger Type](setdldextension.md)' dialog box (Tools | Set Datalogger Type). Valid program file extensions and their compatibility with different datalogger types are shown in the table below: |

| File Extension | CR8x0 CR1000 CR3000 | CR1000Xe \* CR1000X      | CR6                          | CR300                       | CR350 | GRANITE 6/9/10 |
| -------------- | ------------------- | ------------------------ | ---------------------------- | --------------------------- | ----- | -------------- |
| .CRx           | Yes                 | Yes                      | Yes                          | Yes                         | No    | No             |
| .DLD           | Yes                 | Yes                      | Yes                          | Yes                         | Yes   | Yes            |
| .CRB           | No                  | \*Yes (OS 4 and greater) | \* Yes \*(OS 10 and greater) | \* Yes \*(OS 9 and greater) | Yes   | Yes            |
