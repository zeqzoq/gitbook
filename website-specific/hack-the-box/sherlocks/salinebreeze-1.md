# SalineBreeze-1

Sherlock Scenario and attachment

**Scenario**: Your manager has just informed you that, due to recent budget cuts, you'll need to take on additional responsibilities in threat analysis. As a junior threat intelligence analyst at a cybersecurity firm, you're now tasked with investigating a cyber espionage campaign linked to a group known as Salt Typhoon. Apparently, defending against sophisticated Nation-State cyber threats is now a "do more with less" kind of game. Your Task: Conduct comprehensive research on Salt Typhoon, focusing on their tactics, techniques, and procedures. Utilize the MITRE ATT\&CK framework to map out their activities and provide actionable insights. Your findings could play a pivotal role in fortifying our defenses against this adversary. Dive deep into the data and show that even with a shoestring budget, you can outsmart the cyber baddies.

## Tools used

* [https://attack.mitre.org/](https://attack.mitre.org/)
* [https://mitre-attack.github.io/attack-navigator/](https://mitre-attack.github.io/attack-navigator/)



### Task 1, 2 & 3

**Starting with the MITRE ATT\&CK page, which country is thought be behind Salt Typhoon?**

**According to that page, Salt Typhoon has been active since at least when? (Year)**

**What kind of infrastructure does Salt Typhoon target?**

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

China

2019

Network

</details>

### Task 4

**Salt Typhoon has been associated with multiple custom built malware, what is the name of the malware associated with the ID S1206?**

There are only one software

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Answer</summary>

JumbledPath

</details>

### Task 5, 6 & 7

**What operating system does this malware target?**

**What programming language is the malware written in?**

**What kind of infrastructure does Salt Typhoon target?**

Clicking into JumblePath page.

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

Linux

Go

Cisco

</details>

### Task 8

**The malware can perform 'Indicator Removal' by erasing logs. What is the MITRE ATT\&CK ID for this?**

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Answer</summary>

T1070.002

</details>

### Task 9

**On December 20th, 2024, Picus Security released a blog on Salt Typhoon detailing some of the CVEs associated with the threat actor. What was the CVE for the vulnerability related to the Sophos Firewall?**

There are no "Picus Security" in anywhere including the reference in both of the MITRE page. I google dorked here to get it.

`site:picussecurity.com "Salt Typhoon""Sophos Firewall"`&#x20;

<figure><img src="../../../.gitbook/assets/image (9).png" alt="" width="530"><figcaption></figcaption></figure>

All of them mention about what CVE is for Sophos Firewall

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

CVE-2022-3236

</details>

### Task 10 & 11

**The blog demonstrates how the group modifies the registry to obtain persistence with a backdoor known as Crowdoor. Which registry key do they target?**

**What is the MITRE ATT\&CK ID of the previous technique?**

Although there are three website that mention about CVE in task 9, but there is only one that mention about registry

{% embed url="https://www.picussecurity.com/resource/how-breach-and-attack-simulation-helps-you-to-operationalize-mitre-attck" %}

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

HKCU\Software\Microsoft\Windows\CurrentVersion\Run

T1112

</details>

### Task 12

**On November 25th, 2024, TrendMicro published a blog post detailing the threat actor. What name does this blog primarily use to refer to the group?**

Tweak the google dorking systax a bit

`site:trendmicro.com "Salt Typhoon"`

The first one should be it

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Earth Estries

</details>

### Task 13

**The blog post identifies additional malware attributed to the threat actor. Which malware do they describe as a 'multi-modular backdoor...using a custom protocol protected by Transport Layer Security'?**

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

GHOSTSPIDER

</details>

### Task 14

**Most of the domains the malware communicates with have a .com top-level domain. One uses a .dev TLD. What is the full domain name for the .dev TLD?**

There are one in Campaign Beta

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Answer</summary>

telcom.grishamarkovgf8936.workers.dev

</details>

### Task 15

**What is the filename for the first GET request to the C\&C server used by the malware?**

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Answer</summary>

index.php

</details>

### Task 16, 17 & 18

**On September 30th, 2021, a blog post was released on Securelist by Kaspersky. What was the threat actor's name at that time?**

**What is the name of the malware that this article focuses on?**

More google dorking

`site:securelist.com "Salt Typhoon" after:2021-09-29 before:2021-10-01`

{% embed url="https://securelist.com/ghostemperor-from-proxylogon-to-kernel-mode/104407/" %}

<details>

<summary>Answer</summary>

GhostEmperor

Demodex

Rootkit

</details>

### Task 19

**The first stage consists of a malicious PowerShell dropper. What type of encryption is used to obfuscate the code?**

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Answer</summary>

AES

</details>

Task 20

The malware uses Input/Output Control codes to perform various tasks related to hiding malicious artifacts. What is the IOCTL code used by the malware to hide its service from the list within the services.exe process address space?

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

0x220300

</details>

