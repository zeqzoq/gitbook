# swampCTF

Web – Serialise

The Jackson configuration enables polymorphic deserialization with default typing. However, it only “allows” three types based on the whitelist:

![](<../.gitbook/assets/0 (7).png>)

Deserialization is the process of converting data (like JSON) into objects that a program can work with. In this case, the program takes JSON input and turns it into Java objects (like Person, Address, and Job).

Setting resumeURI": "file:///flag.txt can get the flag. other variable can be random. Either use burp suite or anything else, for me use curl

![](<../.gitbook/assets/1 (7).png>)

Flag: swampCTF{f1l3\_r34d\_4nd\_d3s3r14l1z3\_pwn4g3\_x7q9z2r5v8}

## Pwn- Beginner Pwn 2

Locally, run cyclic to the binary to find the offset

![](<../.gitbook/assets/2 (7).png>)

The offset should be 18. Find the win function where the flag is.

![](<../.gitbook/assets/3 (7).png>)

Craft the payload and test locally

![](<../.gitbook/assets/4 (7).png>)

Then pass to server

![](<../.gitbook/assets/5 (2).png>)

Flag: swampCTF{1t5\_t1m3\_t0\_r3turn!!}

## Forensic – Homework Help

Got a vhd file. I open it with autopsy, not mount it.

Thought we need to do something but there is the flag in plain sight

![](<../.gitbook/assets/6 (1).png>)

Flag: swampCTF{n0thing\_i5\_3v3r\_d3l3t3d}



## Forensic - Preferential Treatment

Follow tcp stream and find cpassword

![](<../.gitbook/assets/12 (1).png>)

The challenge mention about older windows server 2008. So to decrypt this password im using gpp-decrypt

![](<../.gitbook/assets/13 (1).png>)

Flag: swampCTF{4v3r463\_w1nd0w5\_53cur17y}

## Forensic - Planetary Storage

Don’t know what these file is

![](<../.gitbook/assets/14 (1).png>)

But just using cat can found payload

![](<../.gitbook/assets/15 (1).png>)

Decrpypt base64

![](<../.gitbook/assets/16 (1).png>)

Then decrypt again

![](<../.gitbook/assets/17 (1).png>)

Flag: swampCTF{1pf5-b453d-d474b453}

## Web - Editor

![](<../.gitbook/assets/18 (1).png>)

add

\<embed src="flag.txt" width="800px" height="2100px" />

![](<../.gitbook/assets/19 (1).png>)

Flag: swampCTF{c55\_qu3r135\_n07\_j5}
