# Comman GDB Commands

The `help` command lists all GDB commands.

```
help all  /* lists all the commands in GDB */
```

## **Commands for Execution Control Flow**

* **break** : Set a breakpoint.

```
break <func-name>        Set breakpoint at start of function <func-name>
break <line>             Set breakpoint at line number <line>
break <filename:><line>  Set breakpoint at <line> in file <filename>

break main               Set breakpoint at beginning of main
break 13                 Set breakpoint at line 13
break gofish.c:34        Set breakpoint at line 34 in gofish.c
```

* **run** : Start running the debugged program from the beginning.

```
run <command line arguments>

run            Run with no command line arguments
run 2 40 100   Run with 3 commnand line arguments: 2, 40, 100
```

* **continue** (cont): Continue execution from breakpoint

```
continue
```
* **step** (s): Execute the next line(s) of the program's C source code,stepping into a functionif a function call is executed on the line(s).

```
step           Execute next line (stepping into a function)
step <count>   Execute next <count> lines of program code

step 10        Execute the next 10 lines (stepping into functions)
```
If the step commands has a function call in its line, then the step command enters into the function. Thus```step <count>``` may result in the program pausing inside a function that was called from the pause point at which the ```step <count>``` command was issued.

* **next** (n): Similar to the ```step``` command , but it treats a function call as a single line. Unlike ```step``` the ```next``` command doesn't enter the function but uses the returned value and moves on to the next line.

```
next         Execute the next line 
next <count> Execute next <count> instructions
```

* **until**: Execute the program until it reaches the specified source code line number.

```
until <line>   Executes until hit line number <line>
```

* **quit**: Exit GDB

```
quit
```

##  **Commands for Examining the Execution Point and Listing Program Code**

* **list**: List program source code.
 
```
list                Lists next few lines of program source code
list <line>         Lists lines around line number <line> of program
list <start> <end>  Lists line numbers <start> through <end>
list <func-name>    Lists lines around beginning of function <func-name>

list 30 100         List source code lines 30 to 100
```
* **where**  (`backtrace, bt`): Show the contents of the stack. The where command is helpful for pinpointing the location of a program crash and for examining state at the interface between function calls and returns, such as argument values passed to functions.

```
where
```

* **frame** `<frame-num>`: Move into the context of stack frame number <frame-num>. As a default, the program is paused in the context of frame 0, the frame at the top of the stack.

```
frame <frame-num>   Sets current stack frame to <frame-num>
info frame          Show state about current stack frame

frame 3             Move into stack frame 3's context (0 is top frame)
```

## **Commands for Setting and Manipulating Breakpoints**

* **break**: Set a breakpoint

```
break <func-name>   Set a breakpoint at start of a function
break <line>        Set a breakpoint at a line number

break main          Set a breakpoint at start of main
break 12            Set a breakpoint at line 12
break file.c:34     Set a breakpoint at line 34 of file.c
```

* **enable, disable , ignore, delete, clear** :  Enable, disable, ignore for some number of times, or delete one or more breakpoints. The `delete` command deletes a breakpoint by its number. In contrast, using the `clear` command deletes a breakpoint at a particular location in the source code.

```
disable <bnums ...>    Disable one or more breakpoints
enable  <bnums ...>    Enable one or more breakpoints
ignore  <bpnum> <num>  Don't pause at breakpoint <bpnum>
                         the next <num> times it's hit
delete  <bpnum>        Delete breakpoint number <bpnum>
delete                 Deletes all breakpoints
clear <line>           Delete breakpoint at line <line>
clear <func-name>      Delete breakpoint at function <func-name>

info break      List breakpoint info (including breakpoint bnums)
disable 3       Disable breakpoint number 3
ignore  2  5    Ignore the next 5 times breakpoint 2 is hit
enable  3       Enable breakpoint number 3
delete  1       Delete breakpoint number 1
clear   124     Delete breakpoint at source code line 124
```

* **condition**:  Set conditions on breakpoints.A conditional breakpoint is one that only transfers control to GDB when a certain condition is true. It can be used to pause at a breakpoint inside a loop only after some number of iterations, or to pause the program at a breakpoint only when the value of a variable has an interesting value for debugging purposes.

```
condition <bpnum> <exp>    Sets breakpoint number <bpnum> to break
                           only when expression <exp> is true

break 28            Set breakpoint at line 28 (in function play)
info break          Lists information about all breakpoints
  Num Type           Disp Enb Address    What
   1   breakpoint    keep y   0x080483a3 in play at gofish.c:28

condition 1 (i > 1000)     Set condition on breakpoint 1
```

## **Commands for Examining and Evaluating Program State and Expressions**

* **print**(p): Display the value of an expression.

```
print <exp>     Display the value of expression <exp>

p i             print the value of i
p i+3           print the value of (i+3)
```

**To print in different formats:**

```
print    <exp>     Print value of the expression as unsigned int
print/x  <exp>     Print value of the expression in hexadecimal
print/t  <exp>     Print value of the expression in binary
print/d  <exp>     Print value of the expression as signed int
print/c  <exp>     Print ASCII value of the expression
print  (int)<exp>  Print value of the expression as unsigned int

print/x 123        Prints  0x7b
print/t 123        Print  1111011
print/d 0x1c       Prints 28
print/c 99         Prints 'c'
print (int)'c'     Prints  99
```


*Sometimes, expressions may require explicit type casting to inform print how to interpret them*

```
print *(int *)0x8ff4bc10   Print int value at address 0x8ff4bc10
```

When using print to display the value of a dereferenced pointer variable, type casting is not necessary, because GDB knows the type of the pointer variable and knows how to dereference its value.

```
`print *ptr`      Print the int value pointed to by ptr
```

To print out a value stored in a hardware register:

```
print $eax      Print the value stored in the eax register
```

* **display**: Automatically display the value of an expression upon reaching a breakpoint.

```
display <exp>   Display value of <exp> at every breakpoint

display i
display array[i]
```

* **x** (examine memory):  Display the contents of a memory location.

```
x <memory address expression>

x  0x5678       Examine the contents of memory location 0x5678
x  ptr          Examine the contents of memory that ptr points to
x  &temp        Can specify the address of a variable
                 (this command is equivalent to: print temp)
```

In general, `x` takes up to three formatting arguments (`x/nfu <memory address>`); the order in which they are listed does not matter:

```
  1. n: the repeat count (a positive integer value)

  2. f: the display format (s: string, i: instruction, x: hex, d: decimal, t: binary, a: address, ??????)

  3. u: the units format (number of bytes) (b: byte, h: 2 bytes, w: 4 bytes, g: 8 bytes)

Here are some examples (assume s1 = "Hello There" is at memory address 0x40062d):
```

```
x/d   ptr       Print value stored at what ptr points to, in decimal
x/a   &ptr      Print value stored at address of ptr, as an address
x/wx  &temp     Print 4-byte value at address of temp, in hexadecimal
x/10dh  0x1234  Print 10 short values starting at address 0x1234, in decimal

x/4c s1         Examine the first 4 chars in s1
    0x40062d   72 'H'  101 'e'  108 'l'  108 'l'

x/s s1         Examine memory location associated with var s1 as a string
    0x40062d   "Hello There"

x/wd s1        Examine the memory location assoc with var s1 as an int
                (because formatting is sticky, need to explicitly set
                units to word (w) after x/s command sets units to byte)
    0x40062d   72

x/8d s1        Examine ASCII values of the first 8 chars of s1
    0x40062d:  72  101 108 108 111 32  84  104
```
* **whatis**: Show the type of an expression.

```
whatis <exp>       Display the data type of an expression

whatis (x + 3.4)   Displays:  type = double
```

* **info**:  lists information about program state and debugger state. There are a large number of info options for obtaining information about the program???s current execution state and about the debugger. A few examples include:

```
help info       Shows all the info options
help status     Lists more info and show commands

info locals     Shows local variables in current stack frame
info args       Shows the argument variable of current stack frame
info break      Shows breakpoints
info frame      Shows information about the current stack frame
info registers    Shows register values
info breakpoints  Shows the status of all breakpoints
```

* **set**:assign/change the value of a program variable, or assign a value to be stored at a specific memory address, or in a specific machine register.

```
set <variable> = <exp>   Sets variable <variable> to expression <exp>

set x = 123 * y   Set var x's value to (123 * y)
```

