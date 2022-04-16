# ** Comman GDB Commands**

The ```help``` command lists all GDB commands.

```help all  /* lists all the commands in GDB */ ```

## **Commands for Execution Control Flow**

**break** : Set a breakpoint.

```
break <func-name>        Set breakpoint at start of function <func-name>
break <line>             Set breakpoint at line number <line>
break <filename:><line>  Set breakpoint at <line> in file <filename>

break main               Set breakpoint at beginning of main
break 13                 Set breakpoint at line 13
break gofish.c:34        Set breakpoint at line 34 in gofish.c
```

