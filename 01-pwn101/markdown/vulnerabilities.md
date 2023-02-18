# Binary vulnerabilities 🚀

---

## Stack buffer overflow

```c
int main(int argc, char **argv) {  
  char buffer[64];  
  gets(buffer);  
  ...
}
```

---

## Format string

```c
int main(int argc, char **argv) {  
  printf(argv[1]);  
  ...
}
```

---

## Heap bugs

---

## Use-after-free

```c
int main(int argc, char **argv) {  
  char *ptr = malloc(64);  
  free(ptr);  
  ptr[0] = 'A';  
  ...
}
```

---

## Double-free

```c
int main(int argc, char **argv) {  
  char *ptr = malloc(64);  
  free(ptr);  
  ...
  free(ptr);  
}
```
