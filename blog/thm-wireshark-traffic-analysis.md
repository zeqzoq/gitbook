---
description: >-
  I wrote this as my notes as I focuses on blue teaming in ctf competitions and
  my career.
---

# THM Wireshark Traffic Analysis

## Nmap Scan

### TCP Connect Scans

* Relies on the three-way handshake (needs to finish the handshake process).
* Usually conducted with `nmap -sT` command.
* Used by non-privileged users (only option for a non-root user).
* Usually has a windows size larger than 1024 bytes as the request expects some data due to the nature of the protocol.

| Open TCP Port | 	Open TCP Port | Closed TCP Port |
| ------------- | -------------- | --------------- |
| SYN -->       | SYN -->        | SYN -->         |
| <-- SYN, ACK  | <-- SYN, ACK   | <-- RST, ACK    |
| ACK -->       | ACK -->        |                 |
|               | RST, ACK -->   |                 |

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption><p>Open TCP Port (Connect)</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1).png" alt=""><figcaption><p>Closed TCP port (Connect)</p></figcaption></figure>

`tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size > 1024` to filter only TCP Connect, success and failed.

### Syn Scan

* Doesn't rely on the three-way handshake (no need to finish the handshake process).
* Usually conducted with `nmap -sS` command.
* Used by privileged users.
* Usually have a size less than or equal to 1024 bytes as the request is not finished and it doesn't expect to receive data.

| Open TCP Port | Close TCP Port |
| ------------- | -------------- |
| SYN -->       | SYN -->        |
| <-- SYN, ACK  | <-- RST, ACK   |
| RST -->       |                |

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>Open TCP port (SYN)</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption><p>Closed TCP port (SYN)</p></figcaption></figure>

### UDP Scans

* Doesn't require a handshake process
* No prompt for open ports
* ICMP error message for close ports
* Usually conducted with `nmap -sU` command.

|  Open UDP Port | Closed UDP Port                                                          |
| -------------- | ------------------------------------------------------------------------ |
| UDP packet --> | UDP packet -->                                                           |
|                | ICMP Type 3, Code 3 message. (Destination unreachable, port unreachable) |

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption><p>Closed (port no 69) and open (port no 68) UDP ports</p></figcaption></figure>

`icmp.type==3 and icmp.code==3` to filter "Destination unreachable" UDP

## ARP Poisoning & Man In The Middle!

A suspicious situation means having two different ARP responses (conflict) for a particular IP address.

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

for example, picture below, there is a conflict; the MAC address that ends with "b4" crafted an ARP request with the "192.168.1.25" IP address, then claimed to have the "192.168.1.1" IP address.

<figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

There is also ARP Flooding. It came from the same MAC address.

<figure><img src="../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

|  Notes                                                                                                                                                                                                                                                                   | Wireshark filter                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Global search                                                                                                                                                                                                                                                            | `arp`                                                                                                                                                                                                                                                                                                                                                |
| <p></p><p>"ARP" options for grabbing the low-hanging fruits:</p><ul><li>Opcode 1: ARP requests.</li><li>Opcode 2: ARP responses.</li><li>Hunt: Arp scanning</li><li>Hunt: Possible ARP poisoning detection</li><li>Hunt: Possible ARP flooding from detection:</li></ul> | <p></p><ul><li><code>arp.opcode == 1</code></li><li><code>arp.opcode == 2</code></li><li><code>arp.dst.hw_mac==00:00:00:00:00:00</code></li><li><code>arp.duplicate-address-detected or arp.duplicate-address-frame</code></li><li><code>((arp) &#x26;&#x26; (arp.opcode == 1)) &#x26;&#x26; (arp.src.hw_mac == target-mac-address)</code></li></ul> |

Proof of MITM attack: destination MAC address at the same end device

<figure><img src="../.gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>

## Identifying Hosts: DHCP, NetBIOS and Kerberos

Protocols that can be used in Host and User identification:

* Dynamic Host Configuration Protocol (DHCP) traffic
* NetBIOS (NBNS) traffic&#x20;
* Kerberos traffic

### DHCP Analysis

| Notes                                                                                                                                                                                                                                         | Wireshark Filter                                                                                                                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p></p><p>"DHCP Request" options for grabbing the low-hanging fruits:</p><ul><li>Option 12: Hostname.</li><li>Option 50: Requested IP address.</li><li>Option 51: Requested IP lease time.</li><li>Option 61: Client's MAC address.</li></ul> | <p></p><ul><li><code>dhcp.option.hostname contains "keyword"</code></li></ul>                                                                                                                                                           |
| <p></p><p>"DHCP ACK" options for grabbing the low-hanging fruits:</p><ul><li>Option 15: Domain name.</li><li>Option 51: Assigned IP lease time.</li></ul>                                                                                     | <p></p><ul><li><code>dhcp.option.domain_name contains "keyword"</code></li></ul>                                                                                                                                                        |
| <p></p><p>"DHCP NAK" options for grabbing the low-hanging fruits:</p><ul><li>Option 56: Message (rejection details/reason).</li></ul>                                                                                                         | As the message could be unique according to the case/situation, It is suggested to read the message instead of filtering it. Thus, the analyst could create a more reliable hypothesis/result by understanding the event circumstances. |

### NetBIOS (NBNS) Analysis

| Notes                                                                                                                                                                                            | Wireshark Filter                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------ |
| Global search.                                                                                                                                                                                   | <p></p><ul><li><code>nbns</code></li></ul>                         |
| <p></p><p>"NBNS" options for grabbing the low-hanging fruits:</p><ul><li>Queries: Query details.</li><li>Query details could contain "name, Time to live (TTL) and IP address details"</li></ul> | <p></p><ul><li><code>nbns.name contains "keyword"</code></li></ul> |

### Kerberos Analysis

| Notes                                                                                                                                                                                                                                                                                                                                                                  | Wireshark Filter                                                                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Global Search                                                                                                                                                                                                                                                                                                                                                          | <p></p><ul><li><code>kerberos</code></li></ul>                                                                                                                     |
| <p>User account search:</p><ul><li>CNameString: The username.</li></ul><p><strong>Note:</strong> Some packets could provide hostname information in this field. To avoid this confusion, filter the "$" value. The values end with "$" are hostnames, and the ones without it are user names.</p>                                                                      | <p></p><ul><li><code>kerberos.CNameString contains "keyword"</code> </li><li><code>kerberos.CNameString and !(kerberos.CNameString contains "$" )</code></li></ul> |
| <p>"Kerberos" options for grabbing the low-hanging fruits:</p><ul><li>pvno: Protocol version.</li><li>realm: Domain name for the generated ticket.</li><li>sname: Service and domain name for the generated ticket.</li><li>addresses: Client IP address and NetBIOS name.<br></li></ul><p>Note: the "addresses" information is only available in request packets.</p> | <p></p><ul><li><code>kerberos.pvno == 5</code></li><li><code>kerberos.realm contains ".org"</code> </li><li><code>kerberos.SNameString == "krbtg"</code></li></ul> |

## Tunnelling Traffic: ICMP and DNS

### ICMP Analysis

summary: ICMP contain data

| Name                                                                                                                                                                                          | Wireshark Filter                                             |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Global search                                                                                                                                                                                 | <p></p><ul><li><code>icmp</code></li></ul>                   |
| <p></p><p>"ICMP" options for grabbing the low-hanging fruits:</p><ul><li>Packet length.</li><li>ICMP destination addresses.<br></li><li>Encapsulated protocol signs in ICMP payload</li></ul> | <p></p><ul><li><code>data.len > 64 and icmp</code></li></ul> |

### DNS Analysis

| Notes                                                                                                                                                                                                                                                                                                                                                                  | Wireshark Filter                                                                                                 |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Global Search                                                                                                                                                                                                                                                                                                                                                          | <p></p><ul><li><code>dns</code></li></ul>                                                                        |
| <p></p><p>"DNS" options for grabbing the low-hanging fruits:</p><ul><li>Query length.</li><li>Anomalous and non-regular names in DNS addresses.</li><li>Long DNS addresses with encoded subdomain addresses.</li><li>Known patterns like dnscat and dns2tcp.</li><li>Statistical analysis like the anomalous volume of DNS requests for a particular target.</li></ul> | <p></p><ul><li><code>dns contains "dnscat"</code></li><li><code>dns.qry.name.len > 15 and !mdns</code></li></ul> |
|                                                                                                                                                                                                                                                                                                                                                                        |                                                                                                                  |

## Cleartext Protocol Analysis: FTP

### FTP Analysis

| Notes                                                                                                                                                                                                                                                       | Wireshark Filter                                                                                                                                                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>"FTP" options for grabbing the low-hanging fruits:</p><ul><li>x1x series: Information request responses.</li><li>x2x series: Connection messages.</li><li>x3x series: Authentication messages.</li></ul><p>Note: "200" means command successful.</p>     |                                                                                                                                                                                                                                                     |
| <p></p><p>"x1x" series options for grabbing the low-hanging fruits:</p><ul><li>211: System status.</li><li>212: Directory status.</li><li>213: File status</li></ul>                                                                                        | <p></p><ul><li><code>ftp.response.code == 211</code> </li></ul>                                                                                                                                                                                     |
| <p></p><p>"x2x" series options for grabbing the low-hanging fruits:</p><ul><li>220: Service ready.</li><li>227: Entering passive mode.</li><li>228: Long passive mode.</li><li>229: Extended passive mode.</li></ul>                                        | <p></p><ul><li><code>ftp.response.code == 227</code></li></ul>                                                                                                                                                                                      |
| <p></p><p>"x3x" series options for grabbing the low-hanging fruits:</p><ul><li>230: User login.</li><li>231: User logout.</li><li>331: Valid username.</li><li>430: Invalid username or password</li><li>530: No login, invalid password.</li></ul>         | <p></p><ul><li><code>ftp.response.code == 230</code></li></ul>                                                                                                                                                                                      |
| <p></p><p>"FTP" commands for grabbing the low-hanging fruits:</p><ul><li>USER: Username.</li><li>PASS: Password.</li><li>CWD: Current work directory.</li><li>LIST: List.</li></ul>                                                                         | <p></p><ul><li><code>ftp.request.command == "USER"</code></li><li><code>ftp.request.command == "PASS"</code></li><li><code>ftp.request.arg == "password"</code></li></ul>                                                                           |
| <p></p><p>Advanced usages examples for grabbing low-hanging fruits:</p><ul><li>Bruteforce signal: List failed login attempts.</li><li>Bruteforce signal: List target username.</li><li>Password spray signal: List targets for a static password.</li></ul> | <p></p><ul><li><code>ftp.response.code == 530</code></li><li><code>(ftp.response.code == 530) and (ftp.response.arg contains "username")</code></li><li><code>(ftp.request.command == "PASS" ) and (ftp.request.arg == "password")</code></li></ul> |

## Cleartext Protocol Analysis: HTTP

### HTTP Analysis

Following attacks could be detected with the help of HTTP analysis:

* Phishing pages
* Web attacks
* Data exfiltration
* Command and control traffic (C2)

| Notes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Wireshark Filter                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Global search</p><p>Note: HTTP2 is a revision of the HTTP protocol for better performance and security. It supports binary data transfer and request&#x26;response multiplexing.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | <p></p><ul><li><code>http</code></li><li><code>http2</code></li></ul>                                                                                                                                                                                                                                      |
| <p></p><p>"HTTP Request Methods" for grabbing the low-hanging fruits:</p><ul><li>GET</li><li>POST</li><li>Request: Listing all requests</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <p></p><ul><li><code>http.request.method == "GET"</code></li><li><code>http.request.method == "POST"</code></li><li><code>http.request</code></li></ul>                                                                                                                                                    |
| <p></p><p>"HTTP Response Status Codes" for grabbing the low-hanging fruits:</p><ul><li>200 OK: Request successful.</li><li>301 Moved Permanently: Resource is moved to a new URL/path (permanently).</li><li>302 Moved Temporarily: Resource is moved to a new URL/path (temporarily).</li><li>400 Bad Request: Server didn't understand the request.</li><li>401 Unauthorised: URL needs authorisation (login, etc.).</li><li>403 Forbidden: No access to the requested URL. </li><li>404 Not Found: Server can't find the requested URL.</li><li>405 Method Not Allowed: Used method is not suitable or blocked.</li><li>408 Request Timeout:  Request look longer than server wait time.</li><li>500 Internal Server Error: Request not completed, unexpected error.</li><li>503 Service Unavailable: Request not completed server or service is down.</li></ul> | <p></p><ul><li><code>http.response.code == 200</code></li><li><code>http.response.code == 401</code></li><li><code>http.response.code == 403</code></li><li><code>http.response.code == 404</code></li><li><code>http.response.code == 405</code></li><li><code>http.response.code == 503</code></li></ul> |
| <p>"HTTP Parameters" for grabbing the low-hanging fruits:</p><ul><li>User agent: Browser and operating system identification to a web server application.</li><li>Request URI: Points the requested resource from the server.</li><li>Full *URI: Complete URI information.</li></ul><p>*URI: Uniform Resource Identifier.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <p></p><ul><li><code>http.user_agent contains "nmap"</code></li><li><code>http.request.uri contains "admin"</code></li><li><code>http.request.full_uri contains "admin"</code></li></ul>                                                                                                                   |
| <p></p><p>"HTTP Parameters" for grabbing the low-hanging fruits:</p><ul><li>Server: Server service name.</li><li>Host: Hostname of the server</li><li>Connection: Connection status.</li><li>Line-based text data: Cleartext data provided by the server.</li><li>HTML Form URL Encoded: Web form information.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <p></p><ul><li><code>http.server contains "apache"</code></li><li><code>http.host contains "keyword"</code></li><li><code>http.host == "keyword"</code></li><li><code>http.connection == "Keep-Alive"</code></li><li><code>data-text-lines contains "keyword"</code></li></ul>                             |
| <p></p><p>Research outcomes for grabbing the low-hanging fruits:</p><ul><li>Different user agent information from the same host in a short time notice.</li><li>Non-standard and custom user agent info.</li><li>Subtle spelling differences. ("Mozilla" is not the same as  "Mozlilla" or "Mozlila")</li><li>Audit tools info like Nmap, Nikto, Wfuzz and sqlmap in the user agent field.</li><li>Payload data in the user agent field.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                  | <p></p><ul><li><code>(http.user_agent contains "sqlmap") or (http.user_agent contains "Nmap") or (http.user_agent contains "Wfuzz") or (http.user_agent contains "Nikto")</code></li></ul>                                                                                                                 |

## Encrypted Protocol Analysis: Decrypting HTTPS

### Decrypting HTTPS Traffic

| Notes                                                                                                                                                                                                                                                                                                                                                                | Wireshark Filter                                                                                                                                                                                |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>"HTTPS Parameters" for grabbing the low-hanging fruits:</p><ul><li>Request: Listing all requests<br></li><li>TLS: Global TLS search</li><li>TLS Client Request</li><li>TLS Server response</li><li>Local Simple Service Discovery Protocol (SSDP)</li></ul><p>Note: SSDP is a network protocol that provides advertisement and discovery of network services.</p> | <p></p><ul><li><code>http.request</code></li><li><code>tls</code></li><li><code>tls.handshake.type == 1</code></li><li><code>tls.handshake.type == 2</code></li><li><code>ssdp</code></li></ul> |

Similar to the TCP three-way handshake process, the TLS protocol has its handshake process. The first two steps contain "Client Hello" and "Server Hello" messages. The given filters show the initial hello packets in a capture file. These filters are helpful to spot which IP addresses are involved in the TLS handshake.

* Client Hello: `(http.request or tls.handshake.type == 1) and !(ssdp)`&#x20;
* Server Hello: `(http.request or tls.handshake.type == 2) and !(ssdp)`
