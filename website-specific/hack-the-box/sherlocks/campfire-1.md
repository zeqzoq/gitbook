# Campfire-1

## Scenario

Alonzo Spotted Weird files on his computer and informed the newly assembled SOC Team. Assessing the situation it is believed a Kerberoasting attack may have occurred in the network. It is your job to confirm the findings by analyzing the provided evidence. You are provided with: 1- Security Logs from the Domain Controller 2- PowerShell-Operational Logs from the affected workstation 3- Prefetch Files from the affected workstation

### Task 1

**Analyzing Domain Controller Security Logs, can you confirm the date & time when the kerberoasting activity occurred?**

zero knowledge on kerberoasting tbh.&#x20;

<figure><img src="../../../.gitbook/assets/image (46).png" alt="" width="524"><figcaption></figcaption></figure>

Then filtering event id 4769 as the AI said its the most crucial event for Kerberoating detection.

<figure><img src="../../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

2024-05-21 11:05:54

Its wrong. I thought that multiple alert in the same seconds indicate some sort of bruteforcing but Im wrong. Taking a look at the hint itself, it stated that we are looking for service name is NOT (krbtgt or ends with $ (For e.g DC01$ ) ). The ticket type should be 0x17 which is for RC4 type encryption. The failure code should be 0x0. Why? I do not know too. Im very new in attacking windows machine. lets ask my best friend. ChatGPT

* **Service name is NOT `krbtgt`**
  * Kerberoasting targets **service tickets (TGS)** for accounts with **SPNs**, not the **TGT**.
  * `krbtgt` is the KDC account used to sign/encrypt **TGTs** (logged in **4768**, not 4769). Requests “to” `krbtgt` aren’t the roastable service tickets you care about, so you exclude it.
* **Service name does NOT end with `$` (e.g., `DC01$`)**
  * Names ending in `$` are **computer accounts** or **(g)MSAs**.
  * Their passwords are **long, random, and auto-rotated** by AD (computers \~30 days; gMSAs even stronger), making the RC4 hash essentially **impractical to crack** and generating lots of benign noise.
  * Attackers typically go after **user-backed service accounts** with SPNs (human-set passwords), not machine/gMSA accounts.
* **Ticket Encryption Type = `0x17` (RC4-HMAC)**
  * `0x17` (decimal 23) is **RC4-HMAC**. When you see `0x17`, the KDC issued a ticket using the account’s **NT hash** as the key—exactly what Kerberoasting tools extract for **offline cracking**.
  * AES (`0x11`/`0x12`) is far harder to crack; many detections focus on `0x17` because it’s the **most crackable** and the typical goal of an attacker (they may force or negotiate RC4 if the service account allows it).
* **Failure Code = `0x0` (success)**
  * You only get a roastable blob when the **TGS request succeeds**. Any other failure code means no usable ticket was issued. Filtering to `0x0` removes noise from failed auth attempts.

#### Putting it together (how it maps to Kerberoasting)

* Kerberoasting = request **TGS** for a **service account with an SPN** → receive a **service ticket** encrypted with the **service account’s key** → extract the **RC4 hash** from the ticket → **crack offline**.
* Therefore, you look for **4769** events where:
  * It’s not the KDC/TGT path (`krbtgt` excluded),
  * It’s not a machine/gMSA account (`$` excluded),
  * The **encryption type is RC4 (`0x17`)** (the crackable case),
  * And the request **succeeded (`0x0`)**.

#### Extra tips (useful refinements)

* Also look at **Service Name (SPN)** patterns (e.g., `MSSQLSvc/`, `HTTP/`, `CIFS/`) and **unusual client IPs** or **rare requesters**.
* If your environment enforces **AES-only** for service accounts, flag any **RC4** issuance as **high-signal**.
* Consider time-of-day and volume anomalies (e.g., one user requesting many distinct SPNs quickly).

That’s the “why” behind each filter: maximize signal for **crackable, successful TGS requests** against **human-password service accounts**, and minimize noise.

Now back to me. While i search for the answer, I may found the obvious one. Which is MSSQLService. I do hope this is the answer...

<figure><img src="../../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

2024-05-21 11:18:09

and its incorrect again. then found administrator. I mean.. who doesnt want to crack an admin password?

<figure><img src="../../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

and wrong again.. my bad its not 0x17.

Theres nothing I found. Honestly Im pretty confident about 2024-05-21 11:18:09 being the right answer. Then looking at the writeup... it correct but its in UTC. im UTC+8. Event Viewer used local time to dipplay the time. Welp.. atleast learned something new.

2024-05-21 03:18:09

### Task 2

**What is the Service Name that was targeted?**

From the same event we know the service is MSSQLService

### Task 3

**It is really important to identify the Workstation from which this activity occurred. What is the IP Address of the workstation?**

Again, fromt he same event at Client Address: 172.17.79.129

### Task 4

**Now that we have identified the workstation, a triage including PowerShell logs and Prefetch files are provided to you for some deeper insights so we can understand how this activity occurred on the endpoint. What is the name of the file used to Enumerate Active directory objects and possibly find Kerberoastable accounts in the network?**

Now the challenge said to use the other files. Beside SECURITY-DC.evtx we got Powershell-Operational.evtx and prefetch files. When enumerating anything, I looked into what command line executed. So i opened Powershell-Operational.evtx. Honestly there are many scripts to look at. But there is only one file thats being executed which makes this Sherlock "Very Easy".

powerview.ps1

### Task 5

**When was this script executed?**

2024-05-21 11:16:32 but I learned from mistakes

2024-05-21 03:16:32

### Task 6

**What is the full path of the tool used to perform the actual kerberoasting attack?**

For this I dont really found any eecuted tools using powershell. So i think the attacker might be double-clicking the tools to run it. So onwards to the prefetch files. But to read the files we need some parser. Because we cant read using cat or strings. Looking at zimmerman's tools becase thats where one of the trusted forensics tools I used. Luckily, there is one for prefetch file called PECmd. [https://ericzimmerman.github.io/#!index.md](https://ericzimmerman.github.io/#!index.md)

```powershell
.\PECmd.exe -d prefetch --json prefetch.json --csv prefetch.csv
[snip]
---------- Processed prefetch\WWAHOST.EXE-2CFA09D4.pf in 0.32614490 seconds ----------
Processed 207 out of 212 files in 17.2942 seconds

Failed files
  prefetch\FILESYNCCONFIG.EXE-1C1104B5.pf ==> (Invalid signature! Should be 'SCCA')
  prefetch\MICROSOFT.SHAREPOINT.EXE-EECBA9B3.pf ==> (Invalid signature! Should be 'SCCA')
  prefetch\SVCHOST.EXE-6A4A44E7.pf ==> (Invalid signature! Should be 'SCCA')
  prefetch\SVCHOST.EXE-77C41F85.pf ==> (Invalid signature! Should be 'SCCA')
  prefetch\SVCHOST.EXE-B6F285B2.pf ==> (Invalid signature! Should be 'SCCA')

prefetch.json does not exist. Creating...
Saving json output to prefetch.json\20250925133556_PECmd_Output.json
Path to prefetch.csv does not exist. Creating...
CSV output will be saved to prefetch.csv\20250925133556_PECmd_Output.csv
CSV time line output will be saved to prefetch.csv\20250925133556_PECmd_Output_Timeline.csv
```

When viewing the forensic files, i has too many things to look at. Thats why we use parser, filtering and so on to view things easier. There are one tools from Zimmerman called Timeline Explorer use to parse CSV file.

Using the tools, taking a look at 2024-05-21 there is one exe file that not belongs to Windows

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

\VOLUME{01d951602330db46-52233816}\USERS\ALONZO.SPIRE\DOWNLOADS\RUBEUS.EXE

change to HTB answer format

C:\Users\Alonzo.Spire\Downloads\Rubeus.exe

### Task 7

**When was the tool executed to dump credentials?**

2024-05-21 03:18:08
