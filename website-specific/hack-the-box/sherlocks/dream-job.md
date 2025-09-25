# Dream Job

## Sherlock Info, Scenario and attachment

**Info:** In this Sherlock, players will be introduced to the MITRE ATT\&CK framework, which is a comprehensive tool used to research and understand advanced persistent threat (APT) groups. Specifically, players will focus on the APT group known as Lazarus Group. As they progress, players will get to explore various tactics, techniques, and procedures (TTPs) associated with Lazarus Group.

**Scenario**: You are a junior threat intelligence analyst at a Cybersecurity firm. You have been tasked with investigating a Cyber espionage campaign known as Operation Dream Job. The goal is to gather crucial information about this operation.

{% file src="../../../.gitbook/assets/DreamJob1.zip" %}

The zip file contain IOCs.txt

{% code title="IOCs.txt" fullWidth="false" %}
```
1. 7bb93be636b332d0a142ff11aedb5bf0ff56deabba3aa02520c85bd99258406f
2. adce894e3ce69c9822da57196707c7a15acee11319ccc963b84d83c23c3ea802
3. 0160375e19e606d06f672be6e43f70fa70093d2a30031affd2929a5c446d07c1 
```
{% endcode %}

## Tools used

* [https://attack.mitre.org/](https://attack.mitre.org/)
* [https://mitre-attack.github.io/attack-navigator/](https://mitre-attack.github.io/attack-navigator/)
* [https://www.virustotal.com/gui/home/search](https://www.virustotal.com/gui/home/search)

### Task 1

**Who conducted Operation Dream Job?**

The answer can be found at the Sherlock Info. Or can visit Operation Dream Job on [MITRE ATT\&CK](https://attack.mitre.org/campaigns/C0022/)

<figure><img src="../../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Answer</summary>

Lazarus Group

</details>

### Task 2

**When was this operation first observed?**

On the same page, it shows the First Seen date. This question does not ask for the Lazarus group, "The group has been active since at least 2009" taken from MITRE ATT\&CK, but it ask about the operation itself.

<details>

<summary>Answer</summary>

September 2019

</details>

### Task 3

**There are 2 campaigns associated with Operation Dream Job. One is `Operation North Star`, what is the other?**

The answer can be found at the same place as Task 2

<details>

<summary>Answer</summary>

Operation Interception

</details>

### Task 4

**During Operation Dream Job, there were the two system binaries used for proxy execution. One was `Regsvr32`, what was the other?**

On the same page, looking at the Techniques Used section

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Answer</summary>

Rundll32

</details>

### Task 5 & 6

What lateral movement technique did the adversary use? What is the technique ID for the previous answer?

As a newbies in Threal Intelligence, I dont really know which technique belongs to which technique. Im sure its to just memorize it but i new to it. There are the feature in MITRE ATT\&CK called attack navigator. I preferred to just view over [there](https://mitre-attack.github.io/attack-navigator/#layerURL=https%3A%2F%2Fattack.mitre.org%2Fcampaigns%2FC0022%2FC0022-enterprise-layer.json) where it categorized the techniques and I have better understanding. Scroll to the right and view the technique under lateral movement

<details>

<summary>Answer</summary>

Task 5: Internal Spearphishing

Task 6: T1534

</details>

### Task 7

**What Remote Access Trojan did the Lazarus Group use in Operation Dream Job?**

Going back to Operation Dream Job page, under software table. While we dont really know what the 3 tools may be doing, all we can do is to read about it. So click each of them, read, and eventually found which one is the Remote Access Trojan.

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

DRATzarus

</details>

### Task 8

**What technique did the malware use for execution?**

Going to attack navigator for DRATzarus, look under execution

<details>

<summary>Answer</summary>

Native API

</details>

### Task 9

**What technique did the malware use to avoid detection in a sandbox?**

There are many technique for evasion, but need to find specifically for sandbox

<details>

<summary>Answer</summary>

Time Based evasion

</details>

### Task 10

**To answer the remaining questions, utilize VirusTotal and refer to the IOCs.txt file. What is the name associated with the first hash provided in the IOC file?**

Just copy paste the first hash into VT

<details>

<summary>Answer</summary>

IEXPLORE.EXE

</details>

### Task 11

**When was the file associated with the second hash in the IOC first created?**

Looking at the Details History

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2020-05-12 19:26:17

</details>

### Task 12

**What is the name of the parent execution file associated with the second hash in the IOC?**

Looking under Relations tab

<details>

<summary>Answer</summary>

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

BAE\_HPC\_SE.iso

</details>

### **Task 13**

Examine the third hash provided. What is the file name likely used in the campaign that aligns with the adversary's known tactics?

Under Details tab there sure have multiple names we can see but the one aligns with the adversary's known tactics (Spear Phishing) is one

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Answer</summary>

Salary\_Lockheed\_Martin\_job\_opportunities\_confidential.doc

</details>

### Task 14

**Which URL was contacted on 2022-08-03 by the file associated with the third hash in the IOC file?**

Under relations tab

<figure><img src="../../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Answer</summary>

hxxps\[://]markettrendingcenter\[.]com/lk\_job\_oppor\[.]docx

</details>
