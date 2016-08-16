# Midos
Midos is a Win10-like operating system for ComputerCraft computers.  

## .midos File Format
The Midos file format is simple, yet hard to remember as there will be at least 254 commands.  
(254 because if there will be more than 255, I need to create a complex command that uses 2 bytes.)  
(Same goes for 65535, but there probably wont be that many commands.)  

Here is a setup tutorial, there rest will be in text:  
(IN PROGRESS)  

|        | 0_                            | 1_                         | 2_ | 3_ | 4_ | 5_ | 6_ | 7_ | 8_ | 9_ | A_ | B_ | C_ | D_ | E_ | F_ |
|--------|-------------------------------|----------------------------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| **_0** | Push 0                        | Refer to variables         |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_1** | Push 1                        | Refer to variables         |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_2** | Push 2                        | Pop any value              |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_3** | Push 3                        | Refer to code-blocks       |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_4** | Push 4                        | Refer to code-blocks       |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_5** | Push 5                        | Run Code block             |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_6** | Push 6                        | Swap 2 values in the stack |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_7** | Push 7                        | Create a new array         |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_8** | Push 8                        | End array                  |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_9** | Push 9                        | Refer to strings           |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_A** | Pop 2 values, push sum        |                            |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_B** | Pop 2 values, push difference |                            |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_C** | Pop 2 values, push product    |                            |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_D** | Pop 2 values, push quotient   |                            |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_E** | Pop 2 values, push power      |                            |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
| **_F** | Pop value, push square root   |                            |    |    |    |    |    |    |    |    |    |    |    |    |    |    |

### Very important note about pushing to the stack
Items are pushed to arrays if one is being created:  

    17 05 18 05
This program will result in an array containing 5. After the array there'll be another 5.  

### Variables
Variables are very simple to use.  
An application can have a maximum of 2^16 variables (no subtracting 1) which is a lot for ComputerCraft.  
Thing of Midos variables as C pointers that point to an object in the memory.  

Simple prorgam:  

    05 10 00 00
First byte adds `5` to the stack. Now what?  
After any `10` is found, it reads the next 2 bytes (`00 00`) and puts `5` in a variable with key `00 00`.  
You can replace `00 00` with any other 2 bytes.  

To get a variable from the "memory", use `11` and put in your 2 bytes:  

    11 00 00

### Code Blocks
`13` begins a new code block.  
`14` end code block.  

Code blocks are anonymous functions (not anonymous if assigned to a variable).  
`15` runs a code block (has to be at end of stack)  

    13 05 0A 14
Creates a new code block that pushes 5 to the stack and adds it to another number in the stack.  
Code blocks that do not need to know what is in the stack are called Smart Code Blocks (I made that up while writing this).  

    13 05 0A 14 05 16 15
Full program:

    13 05 0A 14          Create the code block (the same one as above)
                05       Push 5
                   16 15 Swap, and the run code block.
Try to guess the correct answer.
<details>
  <summary>Click on the arrow beside this text to see the correct answer.</summary>
  10
</details>

### Strings
Strings are very easy, although different from other languages.  
Midos supports strings containing bytes only from 0 to 255 (ASCII).  
`19` creates a new string.  
`19` requires an item in the stack: The length of the string.
After `19` you must put the bytes that construct the string.

    `02 19 68 69`
Try to guess the correct answer.
<details>
  <summary>Click on the arrow beside this text to see the correct answer.</summary>
  hi
</details>
