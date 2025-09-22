# Tusk Infostealer Lab

## Scenario

A blockchain development company detected unusual activity when an employee was redirected to an unfamiliar website while accessing a DAO management platform. Soon after, multiple cryptocurrency wallets linked to the organization were drained. Investigators suspect a malicious tool was used to steal credentials and exfiltrate funds.

Your task is to analyze the provided intelligence to uncover the attack methods, identify indicators of compromise, and track the threat actor’s infrastructure.

{% file src="../../.gitbook/assets/222-Tusk-Infostealer.zip" %}

{% code title="hash.txt" %}
```
MD5: E5B8B2CF5B244500B22B665C87C11767


Use this hash on online threat intel platforms (e.g., VirusTotal, Hybrid Analysis) to complete the lab analysis.
```
{% endcode %}

## Tools

* [https://opentip.kaspersky.com/](https://opentip.kaspersky.com/)
* [https://www.virustotal.com/gui](https://www.virustotal.com/gui/file/523d4eb71af86090d2d8a6766315a027fdec842041d668971bfbbbd1fe826722)
* [https://securelist.com/](https://securelist.com/)

<details>

<summary>Q1</summary>

In **KB**, what is the size of the malicious file?

</details>

<details>

<summary>Answer</summary>

[https://opentip.kaspersky.com/E5B8B2CF5B244500B22B665C87C11767/results?tab=lookup](https://opentip.kaspersky.com/E5B8B2CF5B244500B22B665C87C11767/results?tab=lookup)

921.36 KB

</details>

<details>

<summary>Q2</summary>

**What word** do the threat actors use in log messages to describe their victims, based on the name of an ancient hunted creature?

</details>

<details>

<summary>Answer</summary>

From virus total community tab, saw a link to securelist blog

[https://securelist.com/tusk-infostealers-campaign/113367/](https://securelist.com/tusk-infostealers-campaign/113367/)

There are one paragraph stated about it

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q3</summary>

The threat actor set up a malicious website to mimic a platform designed for creating and managing decentralized autonomous organizations (DAOs) on the MultiversX blockchain (peerme.io). What is the name of the malicious website the attacker created to simulate this platform?

</details>

<details>

<summary>Answer</summary>

CTRL+F to find it

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q4</summary>

Which cloud storage service did the campaign operators use to host malware samples for both macOS and Windows OS versions?

</details>

<details>

<summary>Answer</summary>

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q5</summary>

The malicious executable contains a configuration file that includes base64-encoded URLs and a password used for archived data decompression, enabling the download of second-stage payloads. What is the password for decompression found in this configuration file?

</details>

<details>

<summary>Answer</summary>

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q6</summary>

What is the name of the function responsible for retrieving the field archive from the configuration file?

</details>

<details>

<summary>Answer</summary>

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q7</summary>

In the third sub-campaign carried out by the operators, the attacker mimicked an AI translator project. What is the name of the legitimate translator, and what is the name of the malicious translator created by the attackers?

</details>

<details>

<summary>Answer</summary>

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q8</summary>

The downloader is tasked with delivering additional malware samples to the victim’s machine, primarily infostealers like StealC and Danabot. What are the IP addresses of the **StealC C2 servers** used in the campaign?

</details>

<details>

<summary>Answer</summary>

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q9</summary>

What is the address of the Ethereum cryptocurrency wallet used in this campaign?

</details>

<details>

<summary>Answer</summary>

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

</details>
