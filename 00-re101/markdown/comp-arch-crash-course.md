# Computer architecture crash course 🚀

---

## Existing architectures

* x86 (CISC) <!-- .element: class="fragment" data-fragment-index="1" -->
* x86-64 (CISC) <!-- .element: class="fragment" data-fragment-index="2" -->
* ARM (RISC) <!-- .element: class="fragment" data-fragment-index="3" -->
* MIPS (RISC) <!-- .element: class="fragment" data-fragment-index="4" -->
* ... <!-- .element: class="fragment" data-fragment-index="5" -->

Note:

The primary difference between RISC and CISC architecture is that RISC-based machines execute one instruction per clock cycle. In a CISC processor, each instruction performs so many actions that it takes several clock cycles to complete.

* CISC - Complex Instruction Set Computer
* RISC - Reduced Instruction Set Computer

Baikal - MIPS/ARM processor

---

## Differences

Depending on the architecture, the following things may be different:

* Registers <!-- .element: class="fragment" data-fragment-index="1" -->
* Instruction set <!-- .element: class="fragment" data-fragment-index="2" -->
* Memory layout <!-- .element: class="fragment" data-fragment-index="3" -->
* Calling conventions <!-- .element: class="fragment" data-fragment-index="4" -->
* ... <!-- .element: class="fragment" data-fragment-index="5" -->

---

### Today we will focus on x86-64

* CISC <!-- .element: class="fragment" -->
* A Byte is 8 bits <!-- .element: class="fragment" -->
* Little endian <!-- .element: class="fragment" -->

---

# Registers

---

<!-- .slide: data-transition="none-out" data-auto-animate -->

## Registers

| Register | Purpose |
| :------: | :-----: |
| rax, rbx, rcx, rdx | general registers |
| rbp | base pointer reg (start of stack) |
| rsp | stack pointer reg (current location in stack, growing downwards) |
| rsi | source index reg (source for data copies) |
| rdi | destination index reg (destination for data copies) |
| rip | instruction pointer (current location in code) |
| r8 - r15 | general purpose registers |

---

<!-- .slide: data-transition="none-in" data-auto-animate -->

## Registers

All general purpose registers can be accessed in 8, 16, 32, and 64 bit variants.

<img src="assets/materials/5roqw7y.png" width="600" height="300">

Note:

R8–R15 are the new 64-bit registers.
R8D–R15D are the lowermost 32 bits of each register.
R8W–R15W are the lowermost 16 bits of each register.
R8B–R15B are the lowermost 8 bits of each register.

---

## Registers (FLAGS)

<table>
  <tr>
    <td style="text-align: center">Flag</td>
    <td style="text-align: center">Purpose</td>
  </tr>
  <tr class="fragment">
    <td style="text-align: center">CF (Carry Flag)</td>
    <td style="text-align: center">Carry Flag</td>
  </tr>
  <tr class="fragment">
    <td style="text-align: center">ZF (Zero Flag)</td>
    <td style="text-align: center">Operation result is 0</td>
  </tr>
  <tr class="fragment">
    <td style="text-align: center">SF (Sign Flag)</td>
    <td style="text-align: center">1 if the number is "negative"</td>
  </tr>
  <tr class="fragment">
    <td style="text-align: center">OF (Overflow Flag)</td>
    <td style="text-align: center">The result of the operation overflowed the memory cell</td>
  </tr>
</table>

For more examples, check out this: <!-- .element: style="font-size: 20px" --> [All about the Flags Register](https://redirect.cs.umbc.edu/courses/undergraduate/CMSC211/fall01/burt/tech_help/flags.html) <!-- .element: style="font-size: 20px" -->

---

## Stack

<!-- .slide: data-transition="none-out" data-auto-animate -->

LIFO (Last In First Out) data structure.

Stack is used to save local variables, function arguments, return addresses, saved registers, etc.

---

<!-- .slide: data-transition="none-in" data-auto-animate -->

## Stack

<img src="assets/materials/Lifo_stack.svg" width="660" height="500">

---

# Instruction Set

---

<!-- .slide: data-transition="none-out" data-auto-animate -->

## Instruction Set (Syntax)

We will conform to the Intel syntax.

1. `<OPERATION> <DESTINATION>, <SOURCE>`
2. `<OPERATION> <DESTINATION>, [<BASE> + <INDEX> * <DATA_SIZE>]`

---

<!-- .slide: data-transition="none-in" data-auto-animate -->

## Instruction Set (Syntax)

1. `XOR RAX, RDI`
2. `MOV RAX, QWORD PTR [RBP - 0x8]`
3. `LEA RAX, [RSP + 0x16]`

---

## Instruction Set (Arithmetic)

<div class="left-col", style="text-align:center">

C

```c []
a += 13;
a -= 37;
a++;
a--;
a *= 2;
a /= 3;
```

</div>

<div class="right-col", style="text-align:center">

Assembly

```asm []
add rax, 13
sub rax, 37
inc rax
dec rax
imul rax, 2
idiv rax, 3
```

</div>

---

## Instruction Set (Logic)

<div class="left-col", style="text-align:center">

1. `XOR AX, BX`
2. `NOT EAX`
3. `AND EAX, 0xFFFF`
4. `OR  EAX, 0xFFFF`
5. `SHL RDI, 2`
6. `ROL RSI, 2`

</div>

<div class="right-col", style="text-align:center">

1. `AX = AX ^ BX`
2. `EAX = ~EAX`
3. `EAX = EAX & 0x0000FFFF`
4. `EAX = EAX | 0x0000FFFF`
5. `RDI = RDI << 2`
6. `Cyclic shift left RSI by 2 bits`

</div>

---

## Instruction Set (Control flow)

| Instruction | Description |
| :---------: | :---------: |
| `TEST AX, BX` | `ZF = 1` if `AX == BX` |
| `CMP EAX, 0x1337` | `ZF = 1` if `EAX == 0x1337` |
| `SUB EAX, 0x1337` | `ZF = 1` if `EAX - 0x1337 == 0` |
| `JMP 0x1337` | Jump to __absolute__ address `0x1337` |
| `JGE +0x17` | Jump to __relative__ address `IP + 0x17` if `SF == OF` |
| `JZ +0x1024` | Jump to __relative__ address `IP + 0x1024` if `ZF == 1` |
<!-- .element: style="font-size: 33px" -->

---

## Instruction Set (Memory)

| Instruction | Description |
| :---------: | :---------: |
| `MOV EAX, DWORD PTR [RBP - 0x8]` | `EAX = *(RBP - 0x8)` |
| `MOV DWORD PTR [RBP - 0x8], EAX` | `*(RBP - 0x8) = EAX` |
| `LEA RAX, [RBP - 0x8]` | `RAX = RBP - 0x8` |
| `PUSH RAX` | `*(RSP - 8) = RAX` |
| `POP RAX` | `RAX = *(RSP - 8)` |
<!-- .element: style="font-size: 33px" -->

---

# Functions

---

<!-- .slide: data-transition="none-out" data-auto-animate -->

## Functions

Functions are a way to group code into reusable blocks.

```asm
add:
  add rsi, rdi
  mov rax, rsi
  ret
```

Later we can call a function by using the `call` instruction.

```asm
mov rdi, 13
mov rsi, 37
call add

$ Result: rax = 481
```

Note:

Do any of you know what the `ret` instruction does?

---

<!-- .slide: data-transition="none-in" data-auto-animate -->

## Functions

But what actually happens when we call a function? <!-- .element: class="fragment" -->

And why do we use rdi, rsi, ... to pass arguments? <!-- .element: class="fragment" -->

What if we wan't to call a function from a library? How do we know what arguments to pass? <!-- .element: class="fragment" -->

---

## Function calls: ABI and calling conventions

1. Arguments are passed either in registers or on the stack. <!-- .element: class="fragment" -->

2. Calling convention is a set of rules that define how functions are called. <!-- .element: class="fragment" -->

3. Linux and windows have different calling conventions. <!-- .element: class="fragment" -->

Note:

1. Different calling conventions (thiscall, cdecl, stdcall, fastcall, ...)
2. What if we have 3rd party libraries? That's why we need standard ABI.

---

## Function calls: x86-64 Default calling convention

<span class="fragment" data-fragment-index="1">

First 6 arguments are passed in registers:

`rdi`, `rsi`, `rdx`, `rcx`, `r8`, `r9`

</span>

<span class="fragment" data-fragment-index="2">

Everything else is passed on the stack.

</span>

<span class="fragment" data-fragment-index="3">

Return value is passed in `rax`.

</span>

---

## Function calls: Prologue and epilogue

Prologue: Code that is executed before the function body (setup stack frame, save registers, ...) <!-- .element: class="fragment" -->

Epilogue: Code that is executed after the function body (restore registers, cleanup stack frame, ...) <!-- .element: class="fragment" -->

```asm [2-4|7-8]
do_it:
    push    rbp
    mov     rbp, rsp
    sub     rsp, 32
    ...
    mov     eax, DWORD PTR [rbp-8]
    leave  ; mov rsp, rbp + pop rbp
    ret
```
<!-- .element: class="fragment" -->

---

## Function calls: Stack frame

<div class="left-col", style="text-align:center">

```asm []
do_it:
    push    rbp
    mov     rbp, rsp
    sub     rsp, 32
    ...
    mov     eax, DWORD PTR [rbp-8]
    leave  ; mov rsp, rbp + pop rbp
    ret

call do_it
...
```

<div class="r-stack">
<span class="fragment fade-in-then-out" data-fragment-index="2">

`call do_it` - Save return address on stack

</span>

<span class="fragment fade-in-then-out" data-fragment-index="3">

`push rbp` - Save old stack frame base

</span>

<span class="fragment fade-in-then-out" data-fragment-index="4">

`mov rbp, rsp` - Allocate new stack frame

</span>

<span class="fragment fade-in-then-out" data-fragment-index="5">

`sub rsp, 32` - Allocate space for local variables

</span>

<span class="fragment fade-in-then-out" data-fragment-index="6">

`leave` - Restore old stack frame (`mov rsp, rbp`)

</span>

<span class="fragment fade-in-then-out" data-fragment-index="7">

`leave` - Restore old stack frame (`pop rbp`)

</span>

<span class="fragment fade-in-then-out" data-fragment-index="8">

`ret` - Pop return address and jump to it

</span>
</div>

</div>

<div class="right-col", style="text-align:center;font-size:25px">

<div class="r-stack">
<img class="fragment fade-in-then-out" data-fragment-index="1" src="assets/materials/stack-frame-1.png" style="width: 60%;">
<img class="fragment fade-in-then-out" data-fragment-index="2" src="assets/materials/stack-frame-2.png" style="width: 60%;">
<img class="fragment fade-in-then-out" data-fragment-index="3" src="assets/materials/stack-frame-3.png" style="width: 60%;">
<img class="fragment fade-in-then-out" data-fragment-index="4" src="assets/materials/stack-frame-4.png" style="width: 60%;">
<img class="fragment fade-in-then-out" data-fragment-index="5" src="assets/materials/stack-frame-5.png" style="width: 60%;">
<img class="fragment fade-in-then-out" data-fragment-index="6" src="assets/materials/stack-frame-6.png" style="width: 60%;">
<img class="fragment fade-in-then-out" data-fragment-index="7" src="assets/materials/stack-frame-7.png" style="width: 60%;">
<img class="fragment fade-in-then-out" data-fragment-index="8" src="assets/materials/stack-frame-8.png" style="width: 60%;">
</div>

</div>

Note:

Allocate stack frame
Setup return value
Restore old stack frame

1. `push rbp` - Save old stack frame
2. `mov rbp, rsp` - Allocate new stack frame
3. `sub rsp, 32` - Allocate space for local variables
4. `mov eax, DWORD PTR [rbp-8]` - Setup return value
5. `leave` - Restore old stack frame (`mov rsp, rbp` + `pop rbp`)
6. `ret` - Pop return address and jump to it

---

<!-- .slide: data-transition="none-out" data-auto-animate -->

## System calls

Interface between the kernel and user space. <!-- .element: class="fragment" data-fragment-index="1" -->

Allows user space programs to access kernel functionality: <!-- .element: class="fragment" data-fragment-index="1" -->

1. read/write files <!-- .element: class="fragment" data-fragment-index="2" -->
2. create processes <!-- .element: class="fragment" data-fragment-index="3" -->
3. open network connections <!-- .element: class="fragment" data-fragment-index="4" -->

Note:

Without system calls it would be impossible to have rights management.

Allows user space programs to access kernel functionality (read/write files, create processes, etc). At the same time the kernel can check legitimacy of the request.

---

<!-- .slide: data-transition="none" data-auto-animate -->

## System calls

<img src="assets/materials/Workings-of-a-System-Call.webp">

---

<!-- .slide: data-transition="none" data-auto-animate -->

## System calls

| rax | System call | rdi | rsi | rdx |
| :-: | :---------: | :-: | :-: | :-: |
| 0   | sys_read    |unsigned int fd | char *buf | size_t count |
| 1   | sys_write   |unsigned int fd | const char*buf | size_t count |
| 2   | sys_open    |const char *filename | int flags | int mode |
| 3   | sys_close   |unsigned int fd |
| 4   | sys_stat    |char* filename | struct stat *statbuf |
<!-- .element: style="font-size: 28px" -->

System calls table: <!-- .element: style="font-size: 20px" --> [Syscall's table](https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/) <!-- .element: style="font-size: 20px" -->

---

<!-- .slide: data-transition="none-in" data-auto-animate -->

## System calls

```asm
global _start
section .text

_start:
  mov rax, 1        ; write(
  mov rdi, 1        ;   STDOUT_FILENO,
  mov rsi, msg      ;   "Hello, world!\n",
  mov rdx, msglen   ;   sizeof("Hello, world!\n")
  syscall           ; );

  mov rax, 60       ; exit(
  mov rdi, 0        ;   EXIT_SUCCESS
  syscall           ; );

section .rodata
  msg: db "Hi, Mom!", 10
  msglen: equ $ - msg
```
<!-- .element: class="fragment" data-fragment-index="1" -->

```bash
> nasm -f elf64 -o hello.o hello.s
> ld -o hello hello.o
```
<!-- .element: class="fragment" data-fragment-index="2" -->

---

# Executable file format

---

## Executable file format

The executable file format is a standard way of combining one or more machine code files and associated data into a single file so they can be loaded into memory and executed by a computer.

The most common executable file formats are:

<span class="fragment" data-fragment-index="1">

1. ELF (Executable and Linkable Format) <!-- .element: class="fragment highlight-blue" data-fragment-index="3" -->

</span>

2. PE (Portable Executable) <!-- .element: class="fragment" data-fragment-index="2" -->

Note:

Q: Why are we talking about this?
A: Because as a reverse engineer most likely you will have to start from an executable file.

---

## 🎅 ELF: Overview

Essentially, ELF file consists of a header and different sections. <!-- .element: class="fragment" -->

| Section name | Usage description | Access permissions |
| --- | --- | --- |
| `.text` | Assembly instructions | `r-x` |
| `.rodata` | Read-only data | `r--` |
| `.data` | Data | `rw-` |
| `.bss` | Uninitialized data | `rw-` |
<!-- .element: style="font-size: 32px" class="fragment" -->

Creating tiny elf: <!-- .element: style="font-size: 20px" --> [tiny/teensy.html](http://www.muppetlabs.com/~breadbox/software/tiny/teensy.html) <!-- .element: style="font-size: 20px" -->  
The anatomy of an executable: <!-- .element: style="font-size: 20px" --> [github](https://github.com/mewmew/dissection) <!-- .element: style="font-size: 20px" -->

Note: A summary of the most commonly used sections in ELF files. The .text section contains executable code while the .rodata, .data and .bss sections contains data in various forms.

---

## 🎅 ELF: Structure

![elf_structure.png](https://raw.githubusercontent.com/mewmew/dissection/master/img/elf_structure.png)

Note:

In general, ELF files consist of a file header, zero or more program headers, zero or more section headers and data referred to by the program or section headers.

---

## 🎅 ELF: Dissection

![elf_dissection.png](https://raw.githubusercontent.com/mewmew/dissection/master/img/elf_dissection.png) <!-- .element: style="height:560px" -->

Note:

All ELF files starts with the four byte identifier 0x7F, 'E', 'L', 'F' which marks the beginning of the ELF file header. The ELF file header contains general information about a binary, such as its __object file type (executable, relocatable or shared object)__, __its assembly architecture (x86-64, ARM, …)__, the __virtual address of its entry point__ which indicates the starting point of program execution, and the __file offsets to the program and section headers__.

Each program and section header describes a continuous segment or section of memory respectively. In general, segments are used by the linker to load executables into memory with correct access permissions, while sections are used by the compiler to categorize data and instructions. _Therefore, the program headers are optional for relocatable and shared objects, while the section headers are optional for executables._

The entire contents of a simple "hello world" ELF executable with colour-coded file offsets, sections, segments and program headers. Each file offset is 8 bytes in width and coloured using a darker shade of its corresponding segment, section or program header.

Starting at the middle of the ELF file header, at offset 0x20, is the file offset (red) to the program table (bright red). The program table contains five program headers which specify the size and file offsets of two sections and three segments, namely the .interp (gray) and the .dynamic (purple) sections, and a read-only (blue), a read-write (green) and a read-execute (yellow) segment.

Several sections are contained within the three segments. The read-only segment contains the following sections:

* .interp: the interpreter, i.e. the linker
* .dynamic: array of dynamic entities
* .dynstr: dynamic string table
* .dynsym: dynamic symbol table
* .rela.plt: relocation entities of the PLT
* .rodata: read-only data section

The read-write segment contains the following section:

* .got.plt: Global Offset Table (GOT) of the PLT (henceforth referred to as the GOT as this executable only contains one such table)

And the read-execute segment contains the following sections:

* .plt: Procedure Linkage Table (PLT)
* .text: executable code section

---

## 🎅 ELF: Dynamic linking

```x86asm
text:
  .start:
    mov   rdi, rodata.hello
    call  plt.printf
    mov   rdi, 0
    call  plt.exit
```
<!-- .element: class="fragment" data-fragment-index="1" style="font-size: 20px" -->

Figure 1: The assembly instructions of the .text section.
<!-- .element: class="fragment" data-fragment-index="1" style="font-size: 24px" -->

```x86asm
plt:
  .resolve:
    push  [got_plt.link_map]
    jmp   [got_plt.dl_runtime_resolve]
  .printf:
    jmp   [got_plt.printf]
  .resolve_printf:
    push  dynsym.printf_idx
    jmp   .resolve
  .exit:
    jmp   [got_plt.exit]
  .resolve_exit:
    push  dynsym.exit_idx
    jmp   .resolve
```
<!-- .element: class="fragment" data-fragment-index="2" style="font-size: 20px" -->

Figure 2: The assembly instructions of the .plt section.
<!-- .element: class="fragment" data-fragment-index="2" style="font-size: 24px" -->

Note:

As visualized in figure 1 the first call instruction of the .text section targets the .printf label of the .plt section instead of the actual address of the printf function in the libc library. The Procedure Linkage Table (PLT) provides a level of indirection between call instructions and actual function (procedure) addresses, and contains one entity per external function as outlined in figure 2. The .printf entity of the PLT contains a jump instruction which targets the address stored in the .printf entity of the GOT. Initially this address points to the next instruction, i.e. the instruction denoted by the .resolve_printf label in the PLT. On the first invokation of printf the linker replaces this address with the actual address of the printf function in the libc library, and any subsequent invokation of printf will target the resolved function address directly.

This method of external function resolution is called lazy dynamic linking as it postpones the work and only resolves a function once it is actually invoked at runtime. The lazy approach to dynamic linking may improve performance by limiting the number of symbols that require resolution. At the same time the eager approach may benefit latency sensitive applications which cannot afford the cost of dynamic linking at runtime.

A closer look at the instructions denoted by the .resolve_printf label in figure 2 reveals how the linker knows which function to resolve. Essentially the dl_runtime_resolve function is invoked with two arguments, namely the dynamic symbol index of the printf function and a pointer to a linked list of nodes, each refering to the .dynamic section of a shared object. Upon termination the linked list of our "hello world" process contains a total of four nodes, one for the executable itself and three for its dynamically loaded libraries, namely linux-vdso.so.1, libc.so.6 and ld64.so.1.

To summarise, the execution of a dynamically linked executable can roughly be described as follows. Upon execution the kernel parses the program headers of the ELF file, maps each segment to one or more pages in memory with appropriate access permissions, and transfers the control of execution to the linker ("/lib/ld64.so.1") which was loaded in a similar fashion. The linker is responsible for initiating the addresses of the dl_runtime_resolve function and the aforementioned linked list, both of which are stored in the GOT of the executable. After this setup is complete the linker transfers control to the entry point of the executable, as specified by the ELF file header (in this case the .start label of the .text section). At this point the assembly instructions of the application are executed until termination and external functions are lazily resolved at runtime by the linker through invokations to the dl_runtime_resolve function.

---

## Hooking dynamic functions

```c
#define _GNU_SOURCE

#include <sys/types.h>
#include <dlfcn.h>

char *fgets(char *s, int size, FILE *stream) {
  ...
}
```
<!-- .element: class="fragment" data-fragment-index="1" -->

Compile: <!-- .element: style="font-size: 26px" --> `gcc -fPIC -shared -o libpatcher.so ./patch.c -ldl` <!-- .element: style="font-size: 26px" -->
<!-- .element: class="fragment" data-fragment-index="2" -->

Run: <!-- .element: style="font-size: 26px" --> `LD_PRELOAD=./libpatcher.so ./hello` <!-- .element: style="font-size: 26px" -->
<!-- .element: class="fragment" data-fragment-index="3" -->

---

## Memory layout

```xxd [|4-8|5|8|10-14|11|14|19-23|20|23|24|9,15-16|]
pwndbg> vmmap
LEGEND: STACK | HEAP | CODE | DATA | RWX | RODATA
             Start                End Perm     Size Offset File
    0x555555554000     0x555555555000 r--p     1000      0 argv-strcmp
    0x555555555000     0x555555556000 r-xp     1000   1000 argv-strcmp
    0x555555556000     0x555555557000 r--p     1000   2000 argv-strcmp
    0x555555557000     0x555555558000 r--p     1000   2000 argv-strcmp
    0x555555558000     0x555555559000 rw-p     1000   3000 argv-strcmp
    0x7ffff7d73000     0x7ffff7d76000 rw-p     3000      0 [anon_7ffff7d73]
    0x7ffff7d76000     0x7ffff7d9e000 r--p    28000      0 libc.so.6
    0x7ffff7d9e000     0x7ffff7f33000 r-xp   195000  28000 libc.so.6
    0x7ffff7f33000     0x7ffff7f8b000 r--p    58000 1bd000 libc.so.6
    0x7ffff7f8b000     0x7ffff7f8f000 r--p     4000 214000 libc.so.6
    0x7ffff7f8f000     0x7ffff7f91000 rw-p     2000 218000 libc.so.6
    0x7ffff7f91000     0x7ffff7f9e000 rw-p     d000      0 [anon_7ffff7f91]
    0x7ffff7fbd000     0x7ffff7fbf000 rw-p     2000      0 [anon_7ffff7fbd]
    0x7ffff7fbf000     0x7ffff7fc2000 r--p     3000      0 [vvar]
    0x7ffff7fc2000     0x7ffff7fc3000 r-xp     1000      0 [vdso]
    0x7ffff7fc3000     0x7ffff7fc5000 r--p     2000      0 ld-linux-x86-64.so.2
    0x7ffff7fc5000     0x7ffff7fef000 r-xp    2a000   2000 ld-linux-x86-64.so.2
    0x7ffff7fef000     0x7ffff7ffa000 r--p     b000  2c000 ld-linux-x86-64.so.2
    0x7ffff7ffb000     0x7ffff7ffd000 r--p     2000  37000 ld-linux-x86-64.so.2
    0x7ffff7ffd000     0x7ffff7fff000 rw-p     2000  39000 ld-linux-x86-64.so.2
    0x7ffffffdd000     0x7ffffffff000 rw-p    22000      0 [stack]
```
<!-- .element: style="font-size: 18px; height: 550px" -->

Note: We've discussed the ELF file format and how it is loaded into memory. Now let's take a look at the memory layout of a process.
