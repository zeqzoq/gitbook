# Perfect Bird | TRX CTF 2025 | Misc

<figure><img src="../.gitbook/assets/flag2.png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/flag3.png" alt="" width="375"><figcaption></figcaption></figure>

Played this CTF together with RE:UN10N members. This CTF is really hard + many RE:UN10N members are busy so mostly the not so strong RE:UN10N members like myself doing it. But its okay because my only goal is to get better. Fighting!!

**Description:**\
As a bird soaring through the sky, you seek the perfect language, and then… you find this

**Flag:** TRX{tHi5\_I5\_th3\_P3rf3ct\_l4nGU4g3!!!!!!}

They gave `chall.db3` file

Reading the file and first time seeing this language.

```
var var 42<Infinity> = 0!

functi main () => {
      const var v = m[a-1]!!!
const var w = ((42 ^ v) % 256) ^ 0x48!!!!
      m[a-1] = w!!!!!!!!!!
      const var a<-3> = 42 % 39!!
      42 += 1!!!!
   if (;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;42) {
      const var v = m[a-1]!!
      const var w = ((42 ^ v) % 256) ^ 0x89!!!
m[a-1] = w!!!!
const var a<-3> = 42 % 39!!
42 += 1!
   }
   if (;;;;;;42) {
    const var v = m[a-1]!!!!!!!!
      const var w = ((42 ^ v) % 256) ^ 0x54!!!
m[a-1] = w!!!!!!!!!!
      const var a<-3> = 42 % 39!
42 += 1!!!!!
   }
   if (;;;;;;;;;;;;;;;;;;;;;;;;42) {
const var v = m[a-1]!!!!!!!!!
      const var w = ((42 ^ v) % 256) ^ 0x24!!!
      m[a-1] = w!!!!
      const var a<-3> = 42 % 39!!!!
42 += 1!!!!
...
const var a<-3> = 42 % 39!!!!!!!
42 += 1!!!
   }
      const const const m<-34434> = [205, 242, 231, 208, 235, 150, 5, 14, 162, 115, 134, 81, 118, 138, 49, 16, 54,142,80, 102, 139, 35, 127, 83, 52, 106, 200, 185, 153, 203, 34, 66, 62, 12, 7, 166, 34, 81, 250]!
   print(m)!
}
```

The file is really long and mostly the same thing, or its looping? But this language is easy to read. So I start with some OSINT to search for this language. Then I found a github link: https://github.com/TodePond/GulfOfMexico and in the github theres also a link to another github where it shows how to run this language: https://github.com/vivaansinghvi07/dreamberd-interpreter/ . If you notice there are two name, Gulf of Mexico and DreamBerd. After some investigating DreamBerd is the old name.

But how do I know this is the language? There are two hints in the challenge description, “bird” and “perfect language”. So I think DreamBerd is the “bird” whereas for “perfect language”, well, the README.md of the github it says “Gulf of Mexico is a perfect programming language.” And if you scroll a bit the language can use multiple !!!!!! and there are const const so its the same as out challenge file .db3.

So start by reading the whole github to make sense of the language. I think this challenge code can be divided into 4 main snippet.

```
var var 42<Infinity> = 0!
```

First one is declaring 42 as a variable. It uses var var so that variable 42 can be re-assigned and edited. And also, this language can have number as variable. <> tag after a variable is their lifespan. The infinity tag indicates that variable 42 will last until the end of the program. Then it assign value 0 to variable 42. the “!” act as a separator that is used to terminate a statement. Basically its the same as “;” in java but in this language, we can be bold!!!!!!!!!!!!!!!!!!!!!. If youre confident that line has no problem then use !!!!!!!!!!!!!!!!!!!!, if not use ?.

```
functi main () => {
```

this is declaring main() function. but why not “function”. Well, we can use any letters from the word function but in order such as func, fun, fn, functi, f.

```
const var v = m[a-1]!!!
const var w = ((42 ^ v) % 256) ^ 0x48!!!!
      m[a-1] = w!!!!!!!!!!
      const var a<-3> = 42 % 39!!
      42 += 1!!!!
   if (;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;42) {
      const var v = m[a-1]!!
      const var w = ((42 ^ v) % 256) ^ 0x89!!!
m[a-1] = w!!!!
const var a<-3> = 42 % 39!!
42 += 1!
   }
...
```

There are no loop in this language. const var v = m\[a-1]!!! Here, const var is used to declare a variable that is mutable (can be edited, but not re-assigned). The expression m\[a-1] accesses an element of the array m. In Gulf of Mexico arrays start at index -1 instead of 0.

const var w = ((42 ^ v) % 256) ^ 0x48!!!! computes a new value w by taking the bitwise XOR (the ^ operator) of 42 and v, applying a modulo 256, and then XORing again with 0x48 (48h) (hex 48).

m\[a-1] = w!!!!!!!!!! then it transfer the value to m\[a-1]

const var a<-3> = 42 % 39!! Specifying a negative lifetime to make a variable exist before its creation, and disappear after its creation. here its just assigning 42 modulus 39 to variable a.

42 += 1!!!! The variable 42 initially is 0 then plus one. I think its like i+1 in loop.

if (;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;42) Semicolons serve as the “not” operator. The repeated semicolons don’t change the logic like exclamation mark. So inside all if block, a variable v is re-declared to grab the current value from the array, a new w is computed using a similar formula, but with a different hexadecimal constant (0x89, 0x54, 0x24 and many more!!!!!!!!!!), the array element is updated with the new w, then lastly A new variable with a lifetime specification is declared, and then the literal 42 is incremented again.

```
    const const const m<-34434> = [205, 242, 231, 208, 235, 150, 5, 14, 162, 115, 134, 81, 118, 138, 49, 16, 54,142,80, 102, 139, 35, 127, 83, 52, 106, 200, 185, 153, 203, 34, 66, 62, 12, 7, 166, 34, 81, 250]!
print(m)!
```

by the look of it you can already see that these numbers is the flag. it store these numbers to m then it just print it. So I think we just need to just run the code? But its not as easy as that. First when trying to run it, you will get error

```bash
$ dreamberd chall.db3
dreamberd.base.InterpretationError: __unnamed_file__, line 34434

       const const const m<-34434> = [205, 242, 231, 208, 235, 150, 5, 14, 162, 115, 134, 81, 118, 138, 49, 16, 54, 142, 80, 102, 139, 35, 127, 83, 52, 106, 200, 185, 153, 203, 34, 66, 62, 12, 7, 166, 34, 81, 250]!
  ^^^^^
Invalid indenting detected (must be a multiple of 3). Tabs count as 2 spaces.
```

This is because all indents must be 3 spaces long. So just add one extra space at const const const m line. Then try to run again.

```bash
$ dreamberd chall.db3
Warning: Public global variable `pivo` access failed.
Code has finished executing. Press ^C once or twice to stop waiting for when-statements and after-statements.
```

It doesnt print anything. I tried to print the m but I failed. Finally I just mimics the code to a python code.

```
m = [205, 242, 231, 208, 235, 150, 5, 14, 162, 115,
     134, 81, 118, 138, 49, 16, 54, 142, 80, 102,
     139, 35, 127, 83, 52, 106, 200, 185, 153, 203,
     34, 66, 62, 12, 7, 166, 34, 81, 250]

# This is variable 42
counter = 0

# Put ALL the hexadecimal constants extracted from each transformation block. Its veryy long.
keys = [0x48,0x89,0x54,0x24,...,0xff,0x84,0xc0,0x75]

# Just simulate the basic XOR. the <> tag after variable can just ignore. Basically there are amny things can ignore like !!!
for key in keys:
    a = counter % 39
    i = a
    v = m[i]
    w = ((counter ^ v) % 256) ^ key
    m[i] = w
    counter += 1

print("m =", m)
```

## [TRX CTF 2025 Misc - Perfect Bird](https://zeqzoq.github.io/ctf/international/team/TRX-CTF-2025-Misc-PerfectBird/) <a href="#page-title" id="page-title"></a>

<i class="fa-list">:list:</i> **Challenges**

* [Misc](https://zeqzoq.github.io/ctf/international/team/TRX-CTF-2025-Misc-PerfectBird/#misc)
  * [Perfect Bird](https://zeqzoq.github.io/ctf/international/team/TRX-CTF-2025-Misc-PerfectBird/#perfect-bird)

![](https://zeqzoq.github.io/assets/images/trx25/flag2.png) ![](https://zeqzoq.github.io/assets/images/trx25/flag3.png)

Played this CTF together with RE:UN10N Malaysia members. This CTF is really hard + many RE:UN10N members are busy so mostly the not so strong RE:UN10N members like myself doing it. But its okay because my only goal is to get better. Fighting!!

***

### Misc[Permalink](https://zeqzoq.github.io/ctf/international/team/TRX-CTF-2025-Misc-PerfectBird/#misc) <a href="#misc" id="misc"></a>

***

#### Perfect Bird[Permalink](https://zeqzoq.github.io/ctf/international/team/TRX-CTF-2025-Misc-PerfectBird/#perfect-bird) <a href="#perfect-bird" id="perfect-bird"></a>

**Description:**\
As a bird soaring through the sky, you seek the perfect language, and then… you find this

**Flag:** TRX{tHi5\_I5\_th3\_P3rf3ct\_l4nGU4g3!!!!!!}

They gave `chall.db3` file

Reading the file and first time seeing this language.

```
var var 42<Infinity> = 0!

functi main () => {
      const var v = m[a-1]!!!
const var w = ((42 ^ v) % 256) ^ 0x48!!!!
      m[a-1] = w!!!!!!!!!!
      const var a<-3> = 42 % 39!!
      42 += 1!!!!
   if (;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;42) {
      const var v = m[a-1]!!
      const var w = ((42 ^ v) % 256) ^ 0x89!!!
m[a-1] = w!!!!
const var a<-3> = 42 % 39!!
42 += 1!
   }
   if (;;;;;;42) {
    const var v = m[a-1]!!!!!!!!
      const var w = ((42 ^ v) % 256) ^ 0x54!!!
m[a-1] = w!!!!!!!!!!
      const var a<-3> = 42 % 39!
42 += 1!!!!!
   }
   if (;;;;;;;;;;;;;;;;;;;;;;;;42) {
const var v = m[a-1]!!!!!!!!!
      const var w = ((42 ^ v) % 256) ^ 0x24!!!
      m[a-1] = w!!!!
      const var a<-3> = 42 % 39!!!!
42 += 1!!!!
...
const var a<-3> = 42 % 39!!!!!!!
42 += 1!!!
   }
      const const const m<-34434> = [205, 242, 231, 208, 235, 150, 5, 14, 162, 115, 134, 81, 118, 138, 49, 16, 54,142,80, 102, 139, 35, 127, 83, 52, 106, 200, 185, 153, 203, 34, 66, 62, 12, 7, 166, 34, 81, 250]!
   print(m)!
}
```

The file is really long and mostly the same thing, or its looping? But this language is easy to read. So I start with some OSINT to search for this language. Then I found a github link: https://github.com/TodePond/GulfOfMexico and in the github theres also a link to another github where it shows how to run this language: https://github.com/vivaansinghvi07/dreamberd-interpreter/ . If you notice there are two name, Gulf of Mexico and DreamBerd. After some investigating DreamBerd is the old name.

But how do I know this is the language? There are two hints in the challenge description, “bird” and “perfect language”. So I think DreamBerd is the “bird” whereas for “perfect language”, well, the README.md of the github it says “Gulf of Mexico is a perfect programming language.” And if you scroll a bit the language can use multiple !!!!!! and there are const const so its the same as out challenge file .db3.

So start by reading the whole github to make sense of the language. I think this challenge code can be divided into 4 main snippet.

```
var var 42<Infinity> = 0!
```

First one is declaring 42 as a variable. It uses var var so that variable 42 can be re-assigned and edited. And also, this language can have number as variable. <> tag after a variable is their lifespan. The infinity tag indicates that variable 42 will last until the end of the program. Then it assign value 0 to variable 42. the “!” act as a separator that is used to terminate a statement. Basically its the same as “;” in java but in this language, we can be bold!!!!!!!!!!!!!!!!!!!!!. If youre confident that line has no problem then use !!!!!!!!!!!!!!!!!!!!, if not use ?.

```
functi main () => {
```

this is declaring main() function. but why not “function”. Well, we can use any letters from the word function but in order such as func, fun, fn, functi, f.

```
const var v = m[a-1]!!!
const var w = ((42 ^ v) % 256) ^ 0x48!!!!
      m[a-1] = w!!!!!!!!!!
      const var a<-3> = 42 % 39!!
      42 += 1!!!!
   if (;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;42) {
      const var v = m[a-1]!!
      const var w = ((42 ^ v) % 256) ^ 0x89!!!
m[a-1] = w!!!!
const var a<-3> = 42 % 39!!
42 += 1!
   }
...
```

There are no loop in this language. const var v = m\[a-1]!!! Here, const var is used to declare a variable that is mutable (can be edited, but not re-assigned). The expression m\[a-1] accesses an element of the array m. In Gulf of Mexico arrays start at index -1 instead of 0.

const var w = ((42 ^ v) % 256) ^ 0x48!!!! computes a new value w by taking the bitwise XOR (the ^ operator) of 42 and v, applying a modulo 256, and then XORing again with 0x48 (48h) (hex 48).

m\[a-1] = w!!!!!!!!!! then it transfer the value to m\[a-1]

const var a<-3> = 42 % 39!! Specifying a negative lifetime to make a variable exist before its creation, and disappear after its creation. here its just assigning 42 modulus 39 to variable a.

42 += 1!!!! The variable 42 initially is 0 then plus one. I think its like i+1 in loop.

if (;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;42) Semicolons serve as the “not” operator. The repeated semicolons don’t change the logic like exclamation mark. So inside all if block, a variable v is re-declared to grab the current value from the array, a new w is computed using a similar formula, but with a different hexadecimal constant (0x89, 0x54, 0x24 and many more!!!!!!!!!!), the array element is updated with the new w, then lastly A new variable with a lifetime specification is declared, and then the literal 42 is incremented again.

```
    const const const m<-34434> = [205, 242, 231, 208, 235, 150, 5, 14, 162, 115, 134, 81, 118, 138, 49, 16, 54,142,80, 102, 139, 35, 127, 83, 52, 106, 200, 185, 153, 203, 34, 66, 62, 12, 7, 166, 34, 81, 250]!
print(m)!
```

by the look of it you can already see that these numbers is the flag. it store these numbers to m then it just print it. So I think we just need to just run the code? But its not as easy as that. First when trying to run it, you will get error

```
─$ dreamberd chall.db3
dreamberd.base.InterpretationError: __unnamed_file__, line 34434

       const const const m<-34434> = [205, 242, 231, 208, 235, 150, 5, 14, 162, 115, 134, 81, 118, 138, 49, 16, 54, 142, 80, 102, 139, 35, 127, 83, 52, 106, 200, 185, 153, 203, 34, 66, 62, 12, 7, 166, 34, 81, 250]!
  ^^^^^
Invalid indenting detected (must be a multiple of 3). Tabs count as 2 spaces.
```

This is because all indents must be 3 spaces long. So just add one extra space at const const const m line. Then try to run again.

```
$ dreamberd chall.db3
Warning: Public global variable `pivo` access failed.
Code has finished executing. Press ^C once or twice to stop waiting for when-statements and after-statements.
```

It doesnt print anything. I tried to print the m but I failed. Finally I just mimics the code to a python code.

```python
m = [205, 242, 231, 208, 235, 150, 5, 14, 162, 115,
     134, 81, 118, 138, 49, 16, 54, 142, 80, 102,
     139, 35, 127, 83, 52, 106, 200, 185, 153, 203,
     34, 66, 62, 12, 7, 166, 34, 81, 250]

# This is variable 42
counter = 0

# Put ALL the hexadecimal constants extracted from each transformation block. Its veryy long.
keys = [0x48,0x89,0x54,0x24,...,0xff,0x84,0xc0,0x75]

# Just simulate the basic XOR. the <> tag after variable can just ignore. Basically there are amny things can ignore like !!!
for key in keys:
    a = counter % 39
    i = a
    v = m[i]
    w = ((counter ^ v) % 256) ^ key
    m[i] = w
    counter += 1

print("m =", m)
```

Running the script got ascii array.

```bash
$ python chall.py
Final m = [84, 82, 88, 123, 116, 72, 105, 53, 95, 73, 53, 95, 116, 104, 51, 95, 80, 51, 114, 102, 51, 99, 116, 95, 108, 52, 110, 71, 85, 52, 103, 51, 33, 33, 33, 33, 33, 33, 125
```

Then use cyberchef or add another function in the python to decrypt it

<figure><img src="../.gitbook/assets/image1.png" alt=""><figcaption></figcaption></figure>
