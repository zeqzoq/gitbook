# UFO-1

Sherlock Info, Scenario and attachment

**Info:** This Sherlock is all about researching the history and capabilities of the Sandworm Team using data from the Mitre Att\&ck page for the group.

**Scenario**: Being in the ICS Industry, your security team always needs to be up to date and should be aware of the threats targeting organizations in your industry. You just started as a Threat intelligence intern, with a bit of SOC experience. Your manager has given you a task to test your skills in research and how well can you utilize Mitre Att\&ck to your advantage. Do your research on Sandworm Team, also known as BlackEnergy Group and APT44. Utilize Mitre ATT\&CK to understand how to map adversary behavior and tactics in actionable form. Smash the assessment and impress your manager as Threat intelligence is your passion.

## Tools used

* [https://attack.mitre.org/](https://attack.mitre.org/)

### Task 1

**According to the sources cited by Mitre, in what year did the Sandworm Team begin operations?**

[https://attack.mitre.org/groups/G0034/](https://attack.mitre.org/groups/G0034/) in the first paragraph

<details>

<summary>Answer</summary>

2009

</details>

### Task 2

**Mitre notes two credential access techniques used by the BlackEnergy group to access several hosts in the compromised network during a 2016 campaign against the Ukrainian electric power grid. One is LSASS Memory access (T1003.001). What is the Attack ID for the other?**

Blackenergy group is the other name for Sandworm Team stated at the right side

<figure><img src="../../../.gitbook/assets/image (22).png" alt="" width="454"><figcaption></figcaption></figure>

Instead of searching the group's attack navigator, we should loo into the 2016 Ukrainian campaign because the group it self has many credential access techniques but the task said got only two.

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Under credential access technique should be only two

<details>

<summary>Answer</summary>

T1110

</details>

### Task 3

**During the 2016 campaign, the adversary was observed using a VBS script during their operations. What is the name of the VBS file?**

CTRL+F in the campaign page and got the filename

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

ufn.vbs

</details>

### Task 4

The APT conducted a major campaign in 2022. The server application was abused to maintain persistence. What is the Mitre Att\&ck ID for the persistence technique was used by the group to allow them remote access?

Look into the 2022 Ukraine Electric Power Attack attack navigator. There are just one that logically can get remote access

<details>

<summary>Answer</summary>

T1505.003

</details>

### Task 5

**What is the name of the malware / tool used in question 4?**

Going back into mitre attack page, find web shell part and the name is there

<details>

<summary>Answer</summary>

Neo-REGEORG

</details>

### Task 6

**Which SCADA application binary was abused by the group to achieve code execution on SCADA Systems in the same campaign in 2022?**

CTRL+F to find SCADA

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

and there is one mentioning to execute commands

<details>

<summary>Answer</summary>

scilc.exe

</details>

### Task 7

**Identify the full command line associated with the execution of the tool from question 6 to perform actions against substations in the SCADA environment.**

Find another row that mention the same file just below it

<details>

<summary>Answer</summary>

C:\sc\prog\exec\scilc.exe -do pack\scil\s1.txt

</details>

### Task 8

**What malware/tool was used to carry out data destruction in a compromised environment during the same campaign?**

There are technique call data destruction. just read about it

<details>

<summary>Answer</summary>

CaddyWiper

</details>

### Task 9

**The malware/tool identified in question 8 also had additional capabilities. What is the Mitre Att\&ck ID of the specific technique it could perform in Execution tactic?**

Clicking into CaddyWiper MITRE page, view the attack navigator and its under execution

<details>

<summary>Answer</summary>

T1106

</details>

### Task 10

**The Sandworm Team is known to use different tools in their campaigns. They are associated with an auto-spreading malware that acted as a ransomware while having worm-like features. What is the name of this malware?**

There are many tools used by Sandworm Team, and I dont really know what each of them use for. Based on the writeup, there are technique for self spreading ransomware which are Data Encrypted for Impact + Lateral Tool Transfer

<details>

<summary>Answer</summary>

NotPetya

</details>

### Task 11

**What was the Microsoft security bulletin ID for the vulnerability that the malware from question 10 used to spread around the world?**

Reading through the techniques used by NotPetya, there are one that mention about exploiting a vulnerability

<details>

<summary>Answer</summary>

MS17-010

</details>

### Task 12

**What is the name of the malware/tool used by the group to target modems?**

As always, CTRL+F does not yield any result for Modem except one in the reference. I decided to take a look at the reference. The reference stated about "AcidRain" and I take a look at the list of tools used by the group and there is one called AcidRain

<details>

<summary>Answer</summary>

AcidRain

</details>

### Task 13

**Threat Actors also use non-standard ports across their infrastructure for Operational-Security purposes. On which port did the Sandworm team reportedly establish their SSH server for listening?**

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

6789

</details>

### Task 14

**The Sandworm Team has been assisted by another APT group on various operations. Which specific group is known to have collaborated with them?**

This one is at the description of the group

<details>

<summary>Answer</summary>

APT28

</details>
