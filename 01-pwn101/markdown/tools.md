# Tools

---

## pwntools

A python library that makes binary exploitation easier.

```python
# Spawn a process
ps = process('./vm')

# Spawn a process with gdb
ps = gdb.debug(['./vm'], gdbscript='b *0x401EA8')

# Load executable and libc
exe = ELF('./vm')        #  ; exe.disasm(addr, size)
libc = ELF('./libc.so')  #  ; libc.search(needle, writable = False)

# Find offsets
got_free = exe.got['free']
free_hook = libc.symbols['__free_hook']
```

---

## 🔫 pwndbg

| Command | Description |
| :-----: | :---------: |
| vmmap | Show memory map |
| retaddr | Show return address |
| canary | Show canary |
| telescope | Show memory |
| leakfind | Find pointer leak chains |

---

## Hands-on exercise
