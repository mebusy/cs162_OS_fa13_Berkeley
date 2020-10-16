# cs162_OS_fa13_Berkeley
Berkeley CS 162 Operating System, Fall 2013, UC Berkeley

- fa 2013
    - [video fa13](https://www.youtube.com/watch?v=hry_qqXLej8&list=PLRdybCcWDFzCag9A0h1m9QYaujD0xefgM)
    - [web fa2013](https://inst.eecs.berkeley.edu/~cs162/fa13/)

- sp 2020
    - [viedo sp2020](https://www.youtube.com/watch?v=itfEcA3TXq4&list=PLIMsSuI81pxq7c91oQMpmXgmGICbuDA_c)
    - [video sp2020 yet another](https://www.youtube.com/watch?v=dTl9QkH4j8o&list=PL6CdojO56mZ3SeRfpzMBMObSnTziA0gfE)
    - [video lecture 3](https://www.youtube.com/watch?v=Wj-Fvs7mMIQ&list=PL--jIyXjDXf6Q4XA6q8RYnyChYzJ0K0F2&index=3)
    - [web sp2020](https://inst.eecs.berkeley.edu/~cs162/sp20/)


## Homework 0

[RMS's gdb Debugger Tutorial](http://www.unknownroad.com/rtfm/gdbtut/gdbtoc.html)

<details>
<summary>
GDP practice
</summary>

```gdb
> gdb programname
(gdb) break main
(gdb) tbreak test.c:19 // temp break
(gdb) run arg1 "arg2" ...
(gdb) kill  // stop exec
(gdb) next   // step over
(gdb) x/s buf2  // examine variable
0x602080:	""
(gdb) disassemble
Dump of assembler code for function recur:
(gdb) si               // si/stepi , step into  machine instruction level
0x00000000004005d3	9	        return recur(i - 1);
(gdb) stepi
(gdb) info registers
rax            0x2	2
rbx            0x0	0
...

(gdb)info stack
(gdb)tbreak recurse.c:3 if i==0

or

(gdb) tbreak recurse.c:3
(gdb) condition  2 i==0
```
</details>


<details>
<summary>
C: to get the maximum number of process, file descriptors, and so on...
</summary>

```c
#include <sys/resource.h>
int getrlimit(int resource, struct rlimit *rlp);
```

```c
struct rlimit lim;

getrlimit( RLIMIT_STACK , &lim);
printf("stack size: %ld\n", lim.rlim_cur );

getrlimit( RLIMIT_NPROC , &lim);
printf("process limit: %ld\n", lim.rlim_cur);

getrlimit( RLIMIT_NOFILE , &lim);
printf("max file descriptors: %ld\n",  lim.rlim_cur);
```

</details>

- ELF (Executable and Linkable Format), the object format used by Linux
    - `gcc -C `  -> assemble code
    - `gcc -c `  -> obj code
    - `objdump -D `  disassemble all 
    - `objdump -t `  show symbol table 

- [objdump symbol table](http://manpages.ubuntu.com/manpages/focal/en/man1/objdump.1.html)

<details>
<summary>
Symbol Table Details
</summary>

```bash
SYMBOL TABLE:
00000000 l    df *ABS*	00000000 map.c
00000000 l    d  .text	00000000 .text
00000000 l    d  .data	00000000 .data
00000000 l    d  .bss	00000000 .bss
00000000 l    d  .note.GNU-stack	00000000 .note.GNU-stack
00000000 l    d  .eh_frame	00000000 .eh_frame
00000000 l    d  .comment	00000000 .comment
00000004       O *COM*	00000004 foo
00000000 g     O .data	00000004 stuff
00000000 g     F .text	00000052 main
00000000         *UND*	00000000 malloc
00000000         *UND*	00000000 recur
```

- COLUMN ONE: the symbol's value
- COLUMN TWO: a set of characters and spaces indicating the flag bits that are set on the symbol. There are seven groupings which are listed below:
    - group one: (l,g,,!) local, global, neither, both.
    - group two: (w,) weak or strong symbol.
    - group three: (C,) symbol denotes a constructor or an ordinary symbol.
    - group four: (W,) symbol is warning or normal symbol.
    - group five: (I,) indirect reference to another symbol or normal symbol.
    - group six: (d,D,) debugging symbol, dynamic symbol or normal symbol.
    - group seven: (F,f,O,) symbol is the name of function, file, object or normal symbol.
- COLUMN THREE: the section in which the symbol lives, ABS means not associated with a certain section
- COLUMN FOUR: the symbol's size or alignment.
- COLUMN FIVE: the symbol's name.

</details>

## Lectures

1. [Lecture 2 Four Fundamental OS Concepts](lecture/2%20Four%20Fundamental%20Concepts%20of%20Operating%20Systems.pdf)
    - Four Fundamental OS Concepts
        1. **Thread**
            - Threads are **virtual cores**.
        2. **Address space**
        3. **Process**
        4. **Dual mode operation / Protection**
    - Q: Do all threads get equal amount of time ?
        - Yes, no, maybe. It dependds a lot on what you're trying to do. It's a scheduling problem.
    - OS -> load program -> relocate
    - Q: Where is kernel code/data in process ?
        - unix: kernel space is mapped in high, but inaccessible to user processes.
    - Q: Does kill the parent process killed the child ?
        - No. The process family are tree structured. If a parent dies before the child oftentimes the child gets inherited by a the **init** process (with pid 1 ).
    - Q: zombie process
        - when a process dies, it will leave a data struct called zombie to wait its parenet process to finish relative information garthing.
        - if the parent didn't invoide `wait()`, `waitpid()` to finish its work,  the zombie data will stay forever.

2. [Lexture 3]()

3. [Lexture 4]()
    - **socket** is an abstraction of network I/O
    - socket setup over TCP/IP
        - server socket: Listens for new connection, and produces new sockets for each unique connection.
        - that is, there is 3 sockets involved.
4. [Lecture 5 Concurrency](lecture/5_Concurrency.pdf)
    - all interrupts are asynchronous



