---
icon: '1'
description: >-
  Network Mapper (Nmap) is an open-source network analysis and security auditing
  tool written in C, C++, Python, and Lua.
---

# Host Discovery

## Introduction to Nmap

Nmap offers many different types of scans that can be used to obtain various results about our targets. Basically, Nmap can be divided into the following scanning techniques:

* Host discovery
* Port scanning
* Service enumeration and detection
* OS detection
* Scriptable interaction with the target service (Nmap Scripting Engine)

### Syntax

The syntax for Nmap is fairly simple and looks like this:

```shell-session
lalalostcode@htb[/htb]$ nmap <scan types> <options> <target>
```

### Scan Techniques

```shell-session
lalalostcode@htb[/htb]$ nmap --help

<SNIP>
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
<SNIP>
```

* If our target sends a `SYN-ACK` flagged packet back to us, Nmap detects that the port is `open`.
* If the target responds with an `RST` flagged packet, it is an indicator that the port is `closed`.
* If Nmap does not receive a packet back, it will display it as `filtered`. Depending on the firewall configuration, certain packets may be dropped or ignored by the firewall.

## Host Discovery

### Scan Network Range

```shell-session
lalalostcode@htb[/htb]$ sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5

10.129.2.4
10.129.2.10
10.129.2.11
10.129.2.18
10.129.2.19
10.129.2.20
10.129.2.28
```

### Scan IP List

```shell-session
lalalostcode@htb[/htb]$ sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5

10.129.2.18
10.129.2.19
10.129.2.20
```

### Scan Multiple IPs

<pre class="language-shell-session"><code class="lang-shell-session">lalalostcode@htb[/htb]$ sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5

<strong>10.129.2.18
</strong>10.129.2.19
10.129.2.20

#Define the octet
lalalostcode@htb[/htb]$ sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5

10.129.2.18
10.129.2.19
10.129.2.20

</code></pre>

```shell-session
lalalostcode@htb[/htb]$ sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping 

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 00:12 CEST
SENT (0.0107s) ICMP [10.10.14.2 > 10.129.2.18 Echo request (type=8/code=0) id=13607 seq=0] IP [ttl=255 id=23541 iplen=28 ]
RCVD (0.0152s) ICMP [10.129.2.18 > 10.10.14.2 Echo reply (type=0/code=0) id=13607 seq=0] IP [ttl=128 id=40622 iplen=28 ]
Nmap scan report for 10.129.2.18
Host is up (0.086s latency).
MAC Address: DE:AD:00:00:BE:EF
Nmap done: 1 IP address (1 host up) scanned in 0.11 seconds
```

**Default TTL Values untuk Sistem Operasi:**

* **TTL 128:** Umumnya digunakan oleh sistem operasi berbasis **Windows**.
* **TTL 64:** Biasanya digunakan oleh sistem operasi berbasis **Linux/Unix**.
* **TTL 255:** Biasanya digunakan oleh perangkat jaringan seperti router atau switch.

| **Scanning Options** | **Description**                                                      |
| -------------------- | -------------------------------------------------------------------- |
| `-sn`                | Disables port scanning.                                              |
| `-oA tnet`           | Stores the results in all formats starting with the name 'tnet'.     |
| `-iL`                | Performs defined scans against targets in provided 'hosts.lst' list. |
| `--reason`           | Displays the reason for specific result.                             |
| `-PE`                | PING scan using ICMP request                                         |



Other resources:

{% embed url="https://nmap.org/book/man.html" %}

{% file src="../../../../../.gitbook/assets/Network_Enumeration_With_Nmap_Module_Cheat_Sheet.pdf" %}



