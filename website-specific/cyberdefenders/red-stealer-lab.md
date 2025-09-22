# Red Stealer Lab

## Scenario

You are part of the Threat Intelligence team in the SOC (Security Operations Center). An executable file has been discovered on a colleague's computer, and it's suspected to be linked to a Command and Control (C2) server, indicating a potential malware infection.\
Your task is to investigate this executable by analyzing its hash. The goal is to gather and analyze data beneficial to other SOC members, including the Incident Response team, to respond to this suspicious behavior efficiently.

{% file src="../../.gitbook/assets/184-RedStealer.zip" %}

{% code title="FileHash.txt" %}
```
File Hash (SHA-256): 248FCC901AFF4E4B4C48C91E4D78A939BF681C9A1BC24ADDC3551B32768F907B

Use this hash on online threat intel platforms (e.g., VirusTotal, Hybrid Analysis) to complete the lab analysis.
```
{% endcode %}

## Tools

* [https://www.virustotal.com/gui/](https://www.virustotal.com/gui/)
* [https://bazaar.abuse.ch/browse/](https://bazaar.abuse.ch/browse/)
* [https://threatfox.abuse.ch/browse/](https://threatfox.abuse.ch/browse/)
* [https://app.any.run/submissions](https://app.any.run/submissions)

<details>

<summary>Q1</summary>

Categorizing malware enables a quicker and clearer understanding of its unique behaviors and attack vectors. What category has Microsoft identified for that malware in VirusTotal?

</details>

<details>

<summary>Answer</summary>

Trojan

</details>

<details>

<summary>Q2</summary>

Clearly identifying the name of the malware file improves communication among the SOC team. What is the file name associated with this malware?

</details>

<details>

<summary>Answer</summary>

WEXTRACT

</details>

<details>

<summary>Q3</summary>

Knowing the exact timestamp of when the malware was first observed can help prioritize response actions. Newly detected malware may require urgent containment and eradication compared to older, well-documented threats. What is the UTC timestamp of the malware's first submission to VirusTotal?

</details>

<details>

<summary>Answer</summary>

2023-10-06 04:41

</details>

<details>

<summary>Q4</summary>

Understanding the techniques used by malware helps in strategic security planning. What is the MITRE ATT\&CK technique ID for the malware's data collection from the system before exfiltration?

</details>

<details>

<summary>Answer</summary>

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q5</summary>

Following execution, which social media-related domain names did the malware resolve via DNS queries?

</details>

<details>

<summary>Answer</summary>

Got from any.run

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q6</summary>

Once the malicious IP addresses are identified, network security devices such as firewalls can be configured to block traffic to and from these addresses. Can you provide the IP address and destination port the malware communicates with?

</details>

<details>

<summary>Answer</summary>

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q7</summary>

YARA rules are designed to identify specific malware patterns and behaviors. Using MalwareBazaar, what's the name of the YARA rule created by "`Varp0s`" that detects the identified malware?

</details>

<details>

<summary>Answer</summary>

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q8</summary>

Understanding which malware families are targeting the organization helps in strategic security planning for the future and prioritizing resources based on the threat. Can you provide the different malware alias associated with the malicious IP address according to **ThreatFox**?

</details>

<details>

<summary>Answer</summary>

search ioc:77.91.124.55 at ThreatFox

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Q9</summary>

By identifying the malware's imported DLLs, we can configure security tools to monitor for the loading or unusual usage of these specific DLLs. Can you provide the DLL utilized by the malware for privilege escalation?

</details>

<details>

<summary>Answer</summary>

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

</details>
