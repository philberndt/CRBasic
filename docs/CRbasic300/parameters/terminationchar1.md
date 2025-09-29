# TerminationChar (Termination Character)

Specifies a single character that marks the end of the incoming block of data. The character can be entered as anASCII

**Note:** Abbreviation for American Standard Code for Information Interchange / American National Standards Institute. An encoding scheme in which numbers from 0-127 (ASCII) or 0-255 (ANSI) are used to represent pre-defined alphanumeric characters. Each number is usually stored and transmitted as 8 binary digits (8 bits), resulting in 1 byte of storage per character of text.

character code or as a string. The termination character can be included in, or excluded from, the result string.[For more information, seeASCII Codes and Characters.](../Info/CodesChar.md)

**Exclude TerminationChar **: If the TerminationChar is an ASCII value between 0 and 255, it will terminate the string input upon seeing the character. The character is not included in the result string. To enter the termination character, simply use the [ASCII code](../Info/CodesChar.md).

** Include TerminationChar**: If the TerminationChar is declared as a string, the input string will terminate after a match of the termination string is found. The matching termination string will be included in the resultant string. For printable characters, the string can be entered directly (for instance, for a period character, enter . ). For non-printable characters, use the CHR() instruction along with the [ASCII code](../Info/CodesChar.md). As an example, for a carriage return (CR), enter CHR(13).

Entering a negative number or a null for the TerminationChar means there is no termination character.

Type: Integer, Variable, or Constant
