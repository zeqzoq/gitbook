# calc (tbc)

calc \[150 points]

Have you ever use Microsoft calculator?

nc chall.pwnable.tw 10100

```bash
┌──(zeqzoq㉿zeqzoq)-[/mnt/c/Users/hzqzz/Downloads/tmp]
└─$ ./calc
=== Welcome to SECPROG calculator ===
1+1
2
test
Merry Christmas!
```

```bash
┌──(zeqzoq㉿zeqzoq)-[/mnt/c/Users/hzqzz/Downloads/tmp]
└─$ file calc
calc: ELF 32-bit LSB executable, Intel 80386, version 1 (GNU/Linux), statically linked, for GNU/Linux 2.6.24, BuildID[sha1]=26cd6e85abb708b115d4526bcce2ea6db8a80c64, not stripped
```

```bash
┌──(zeqzoq㉿zeqzoq)-[/mnt/c/Users/hzqzz/Downloads/tmp]
└─$ checksec calc
[*] '/mnt/c/Users/hzqzz/Downloads/tmp/calc'
    Arch:       i386-32-little
    RELRO:      Partial RELRO
    Stack:      Canary found
    NX:         NX enabled
    PIE:        No PIE (0x8048000)
    Stripped:   No
```

taking a look at each function

{% code title="main()" %}
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  ssignal(14, timeout);
  alarm(60);
  puts("=== Welcome to SECPROG calculator ===");
  fflush(stdout);
  calc();
  return puts("Merry Christmas!");
}
```
{% endcode %}

* **Purpose**
  * Set up a 60 s timeout (signal 14/`SIGALRM` → `timeout` handler), print a welcome banner, then hand off to `calc()`.
  * After `calc()` exits, it prints “Merry Christmas!” and returns.
* **Key points**
  * `ssignal(14, timeout)` + `alarm(60)`: ensures the program will call `timeout()` (not shown) if the user takes too long.
  * `fflush(stdout)` after the banner ensures prompt appears immediately.
  * All real work happens inside `calc()`.

{% code title="calc()" %}
```c
unsigned int calc()
{
  int v1[101]; // [esp+18h] [ebp-5A0h] BYREF
  char s[1024]; // [esp+1ACh] [ebp-40Ch] BYREF
  unsigned int v3; // [esp+5ACh] [ebp-Ch]

  v3 = __readgsdword(0x14u);
  while ( 1 )
  {
    bzero(s, 0x400u);
    if ( !get_expr(s, 1024) )
      break;
    init_pool(v1);
    if ( parse_expr(s, v1) )
    {
      printf("%d\n", v1[v1[0]]);
      fflush(stdout);
    }
  }
  return __readgsdword(0x14u) ^ v3;
}
```
{% endcode %}

* **Purpose**
  * Loop reading arithmetic expressions, evaluate them, and print the result until EOF or blank line.
* **How it works**
  1. **Stack protection**: saves a TLS-based canary in `v3`.
  2. **Input loop**
     * Clears the input buffer `s`.
     * Calls `get_expr(s,1024)`. If it returns 0 (no more input), breaks out.
     * Zeros the “operand pool” array `v1[0..100]` via `init_pool()`.
     *   If parsing+evaluation succeed (`parse_expr` returns 1), prints the top-of-stack result:

         ```c
         cCopyEditv1[0]     // holds the current stack size
         v1[v1[0]] // the top element
         ```
  3. **Return value**: XORs the new canary against the old, returning it—standard stack-cookie check.

```c
int __cdecl get_expr(int a1, int a2)
{
  int v2; // eax
  char v4; // [esp+1Bh] [ebp-Dh] BYREF
  int v5; // [esp+1Ch] [ebp-Ch]

  v5 = 0;
  while ( v5 < a2 && read(0, &v4, 1) != -1 && v4 != 10 )
  {
    if ( v4 == 43 || v4 == 45 || v4 == 42 || v4 == 47 || v4 == 37 || v4 > 47 && v4 <= 57 )
    {
      v2 = v5++;
      *(_BYTE *)(a1 + v2) = v4;
    }
  }
  *(_BYTE *)(v5 + a1) = 0;
  return v5;
}
```

* **Purpose**
  * Read up to `maxlen` bytes from stdin until newline (or EOF), filtering out everything except digits `1–9` and the operators `+ - * / %`.
  * Null-terminate the resulting string.
* **Returns**
  * Number of bytes written (`len`). If `0`, `calc()` will stop.

```c
_DWORD *__cdecl init_pool(_DWORD *a1)
{
  _DWORD *result; // eax
  int i; // [esp+Ch] [ebp-4h]

  result = a1;
  *a1 = 0;
  for ( i = 0; i <= 99; ++i )
  {
    result = a1;
    a1[i + 1] = 0;
  }
  return result;
}
```

**Purpose**

* Zero out the “operand stack” of size 101 (at `pool[0]..pool[100]`).
  * `pool[0]` stores the current stack size (initially 0).
  * `pool[1..]` hold the pushed integer values.

```c
int __cdecl parse_expr(int a1, _DWORD *a2)
{
  int v3; // eax
  int v4; // [esp+20h] [ebp-88h]
  int i; // [esp+24h] [ebp-84h]
  int v6; // [esp+28h] [ebp-80h]
  int v7; // [esp+2Ch] [ebp-7Ch]
  char *s1; // [esp+30h] [ebp-78h]
  int v9; // [esp+34h] [ebp-74h]
  char s[100]; // [esp+38h] [ebp-70h] BYREF
  unsigned int v11; // [esp+9Ch] [ebp-Ch]

  v11 = __readgsdword(0x14u);
  v4 = a1;
  v6 = 0;
  bzero(s, 0x64u);
  for ( i = 0; ; ++i )
  {
    if ( (unsigned int)(*(char *)(i + a1) - 48) > 9 )
    {
      v7 = i + a1 - v4;
      s1 = (char *)malloc(v7 + 1);
      memcpy(s1, v4, v7);
      s1[v7] = 0;
      if ( !strcmp(s1, "0") )
      {
        puts("prevent division by zero");
        fflush(stdout);
        return 0;
      }
      v9 = atoi(s1);
      if ( v9 > 0 )
      {
        v3 = (*a2)++;
        a2[v3 + 1] = v9;
      }
      if ( *(_BYTE *)(i + a1) && (unsigned int)(*(char *)(i + 1 + a1) - 48) > 9 )
      {
        puts("expression error!");
        fflush(stdout);
        return 0;
      }
      v4 = i + 1 + a1;
      if ( s[v6] )
      {
        switch ( *(_BYTE *)(i + a1) )
        {
          case '%':
          case '*':
          case '/':
            if ( s[v6] != 43 && s[v6] != 45 )
              goto LABEL_14;
            s[++v6] = *(_BYTE *)(i + a1);
            break;
          case '+':
          case '-':
LABEL_14:
            eval(a2, s[v6]);
            s[v6] = *(_BYTE *)(i + a1);
            break;
          default:
            eval(a2, s[v6--]);
            break;
        }
      }
      else
      {
        s[v6] = *(_BYTE *)(i + a1);
      }
      if ( !*(_BYTE *)(i + a1) )
        break;
    }
  }
  while ( v6 >= 0 )
    eval(a2, s[v6--]);
  return 1;
}
```

* **Purpose**
  * **Tokenizes** the input string on non-digit characters → extracts integer tokens and operators.
  * **Pushes** each integer (> 0) onto the operand stack. Rejects literal `0` to avoid division-by-zero.
  * Maintains an **operator stack** (`op_stack[]`) and, using simple precedence rules, decides when to call `eval()` on the top operator versus when to push the new one.
  * After the loop, it **evaluates** any remaining operators.
* **Returns**
  * `1` on success (expression parsed & evaluated),
  * `0` on error (“prevent division by zero” or “expression error!”).

```c
_DWORD *__cdecl eval(_DWORD *a1, char a2)
{
  _DWORD *result; // eax

  if ( a2 == 43 )
  {
    a1[*a1 - 1] += a1[*a1];
  }
  else if ( a2 > 43 )
  {
    if ( a2 == 45 )
    {
      a1[*a1 - 1] -= a1[*a1];
    }
    else if ( a2 == 47 )
    {
      a1[*a1 - 1] /= (int)a1[*a1];
    }
  }
  else if ( a2 == 42 )
  {
    a1[*a1 - 1] *= a1[*a1];
  }
  result = a1;
  --*a1;
  return result;
}
```

* **Purpose**
  * Pop the top two integers from the operand stack, apply the operator `op`, push the result back onto the stack (by overwriting the next-to-top slot), and decrement the stack size.
* **Supported ops**:
  * `+`, `-`, `*`, `/`.
  * (Modulo `%` is recognized by the parser but not implemented here.)

In summary, this is essentially a minimal **infix-to-postfix** evaluator, with basic error checks and a 60 s alarm guard against hanging.

1. **Read** a filtered line of input (`get_expr`).
2. **Zero** your stacks (`init_pool`).
3. **Parse & evaluate** with the two-stack method (`parse_expr` + `eval`).
4. **Print** the final result from the top of the operand stack.
