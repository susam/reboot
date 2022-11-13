DOS Reboot Program
==================

The program `REBOOT.COM` was developed on MS-DOS Version 6.22 using
the DOS program named `DEBUG.EXE`. It can be used to reboot MS-DOS.


Assemble
--------

Here is the complete `DEBUG.EXE` session that shows how this program
was written:

```
C:\>DEBUG
-A
1165:0100 JMP FFFF:0000
1165:0105
-N REBOOT.COM
-R CX
CX 0000
:5
-W
Writing 00005 bytes
-Q

C:\>
```

Note that the `N` (name) command specifies the name of the file where
we write the binary machine code to. Also, note that the `W` (write)
command expects the registers BX and CX to contain the number of bytes
to be written to the file. When `DEBUG.EXE` starts, it already
initializes BX to 0 automatically, so we only set the register CX to 5
with the `R CX` command above.

The debugger session inputs are archived in the file named
`REBOOT.TXT`, so the binary file named `REBOOT.COM` can also be
created by running the following DOS command:

```
DEBUG < REBOOT.TXT
```

The binary executable file can be created on a Unix or Linux system
using the `printf` command as follows:

```
echo EA 00 00 FF FF | xxd -r -p > REBOOT.COM
```


Unassemble
----------

Here is a disassembly of `REBOOT.COM` to confirm that it has been
written correctly:

```
C:\>DEBUG
-N REBOOT.COM
-L
-U 100 104
117C:0100 EA0000FFFF    JMP     FFFF:0000
```


Run
---

To run this program on MS-DOS, simply enter the following command at
the command prompt:

```
REBOOT
```


How Does It Work?
-----------------

When the computer boots, the x86 microprocessor starts in real mode
and executes the instruction at FFFF:0000. This is an address in the
BIOS ROM that contains a far jump instruction to go to another
address, typically F000:E05B.

```
C:\>DEBUG
-U FFFF:0000 4
FFFF:0000 EA5BE000F0    JMP     F000:E05B
```

The address F000:E05B contains the BIOS start-up program which
performs a power-on self-test (POST), initializes the peripheral
devices, loads the boot sector code, and executes it. These operations
complete the booting sequence.

The important point worth noting here is that the very first
instruction the microprocessor executes after booting is the
instruction at FFFF:0000. The `REBOOT.COM` program available in this
project too jumps to this address thereby triggering the boot
sequence.


License
-------

This is free and open source software. You can use, copy, modify,
merge, publish, distribute, sublicense, and/or sell copies of it,
under the terms of the MIT License. See [LICENSE.md][L] for details.

This software is provided "AS IS", WITHOUT WARRANTY OF ANY KIND,
express or implied. See [LICENSE.md][L] for details.

[L]: LICENSE.md
