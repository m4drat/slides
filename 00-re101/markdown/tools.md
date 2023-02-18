# Tools

---

## 👀 strings

```bash
> strings /bin/hack_me
/lib64/ld-linux-x86-64.so.2
__cxa_finalize
__libc_start_main
__cxa_atexit
flag{this_is_a_fake_flag}
error
abort
nl_langinf
...
```

---

<!-- .slide: data-transition="none-out" data-auto-animate -->

## 💡 objdump

```bash
> objdump -d -M intel /hack_me


/bin/hack_me:     file format elf64-x86-64


Disassembly of section .init:

0000000000001000 <.init>:
    1000:       f3 0f 1e fa             endbr64
    1004:       48 83 ec 08             sub    rsp,0x8
    1008:       48 8b 05 c1 5f 00 00    mov    rax,QWORD PTR [rip+0x5fc1]
    100f:       48 85 c0                test   rax,rax
    1012:       74 02                   je     1016
    1014:       ff d0                   call   rax
    1016:       e8 75 0a 00 00          call   1a90 <func1+0x90>
    101b:       e8 d0 29 00 00          call   39f0 <func2+0x90>
    1020:       48 83 c4 08             add    rsp,0x8
    1024:       c3                      ret
```
<!-- .element: style="height: 480px" -->

---

## 💡 objdump

<!-- .slide: data-transition="none-in" data-auto-animate -->

```bash
> objdump --start-address 0x35b8 --stop-address 0x35d3 -d -M intel /bin/hack_me

/bin/hack_me:     file format elf64-x86-64


Disassembly of section .text:

00000000000035b8 <.text+0x2108>:
    35b8:       48 89 c2                mov    rdx,rax
    35bb:       31 c0                   xor    eax,eax
    35bd:       e8 ae de ff ff          call   1470 <__fprintf_chk@plt>
    35c2:       48 89 ee                mov    rsi,rbp
    35c5:       bf 0a 00 00 00          mov    edi,0xa
    35ca:       e8 d1 dd ff ff          call   13a0 <fputc_unlocked@plt>
    35cf:       48 83 fb 09             cmp    rbx,0x9
```

---

## 🐉 Ghidra

Ghidra is a free and open source reverse engineering tool developed by the National Security Agency of the United States.

| Shortcuts | Description |
| :-------: | :---------: |
| D | Disassemble |
| C | Clear code/data |
| L | Rename function/variable |
| Ctrl + L | Retype variable |
| Ctrl + E | Decompile |
| B | Cycle integer types |
| ' | Cycle string types |

---

## 🔦 readelf

```bash
> readelf -s hack_me

Symbol table '.symtab' contains 48 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
    26: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND strlen@GLIBC_2.2.5
    27: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __stack_chk_fail[...]
    28: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND printf@GLIBC_2.2.5
    35: 0000000000403060    22 OBJECT  GLOBAL DEFAULT   17 g_context
    36: 0000000000404090     0 NOTYPE  GLOBAL DEFAULT   26 _end
    37: 0000000000401160     5 FUNC    GLOBAL HIDDEN    15 _dl_relocate_sta[...]
    38: 0000000000401130    38 FUNC    GLOBAL DEFAULT   15 _start
    41: 000000000040143a   320 FUNC    GLOBAL DEFAULT   15 main
    42: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND setvbuf@GLIBC_2.2.5
    43: 0000000000401224    55 FUNC    GLOBAL DEFAULT   15 InitContext
    44: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __isoc99_scanf@G[...]
    45: 0000000000401314   133 FUNC    GLOBAL DEFAULT   15 CheckBuffer
```
<!-- .element: style="width: 100%; height: 100%" -->

---

## 🔬 strace

Find out what system calls are being made

```bash
> strace /bin/hack_me
execve("/bin/hack_me", ["/bin/hack_me"], 0x7ffe5c8ec010 /* 38 vars */) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fc191518000
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\237\2\0\0\0\0\0"..., 832) = 832
mmap(0x7fc1914ec000, 5281, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0)
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fc1912ce000
exit_group(0)                           = ?
```
<!-- .element: style="width: 1000px; height: 100%" -->

---

## 🔎 ltrace

Find out what library calls are being made

```bash
> ltrace /bin/hack_me
setvbuf(0x7f26c0dde780, 0, 2, 0)                            = 0
memset(0x7ffc0ce14d50, '\0', 128)                           = 0x7ffc0ce14d50
printf("Hello, Mom!")                        = 12
__isoc99_scanf(0x404170, 0x7ffc0ce14d50, 0, 0x7f26c0cd8a37^C <no return ...>
+++ SIGINT (Interrupt) +++
+++ killed by SIGINT +++
```

---

## 🔫 gdb

GDB - executable and core file debugger, allows you to set breakpoints, inspect registers, and more

| Command | Description |
| :-----: | :---------: |
| `b` | Set a breakpoint |
| `r` | Run the program |
| `c` | Continue execution |
| `si` | Step instruction |
| `ni` | Next instruction |
| `fin` | Finish function |
| `x/10xg` | Examine memory |
| `p` | Print a variable |
| `bt` | Print the backtrace |
| `disas` | Disassemble a function |
| `info registers` | Print the registers |
<!-- .element: style="font-size: 22px" -->

---

## 🦕 godbolt

[Compiler explorer](https://godbolt.org/)

<div class="left-col", style="text-align:center">

C

```c
#include <stdio.h>

int scramble(int a, int b, int c) {
    int d = a * b;
    int e = d + c;
    return e;
}

int main() {
    int a = 10;
    int b = 3;
    int result = scramble(a, b, 3);
    printf("%d", result);
}
```

</div>

<div class="right-col", style="text-align:center">

Assembly

```asm
scramble:
        imul    edi, esi
        lea     eax, [rdi+rdx]
        ret
.LC0:
        .string "%d"
main:
        sub     rsp, 8
        mov     esi, 33
        mov     edi, OFFSET FLAT:.LC0
        mov     eax, 0
        call    printf
        mov     eax, 0
        add     rsp, 8
        ret
```

</div>

Note:

Explore options, different compilers and find out what the compiler is doing

---

## Hands-on exercise
