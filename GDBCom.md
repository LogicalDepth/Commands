# ** Comman GDB Commands**

The ```help``` command lists all GDB commands.

```help all  /* lists all the commands in GDB */ ```

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








