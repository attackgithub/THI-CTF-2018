# strings-reloaded
Basic reverse engineering challenge.

**Description of the Challenge:**
Aaaah, again a lot of strings. The flag format is `THICTF{<clear-text>}` where `<clear-text>` is the decrypted string of the displayed hash.
Find the correct flag in the binary!

Hint: You will not be able to brute force the hash.

## Setup
--

## Solution
The flag is created during the program execution. Some of the char pointers are used. The output is a sha1 hash and the player need to reverse the binary in order to get the correct flag. The variables 'string4, string61, and string123 are used to build the flag.
```
char* string4 = "UpoUnCvI";
char* string61 = "Udglkwhx";
char* string123 = "XuelCDQy";
```

Only the binary is provied. The C Code will not be provied.

## Flag
THICTF{UpoUnCvIUdglkwhxXuelCDQy}
