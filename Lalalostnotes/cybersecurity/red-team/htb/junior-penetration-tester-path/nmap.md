---
description: >-
  Network Mapper (Nmap) is an open-source network analysis and security auditing
  tool written in C, C++, Python, and Lua.
icon: eye
---

# NMAP

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

| **Scanning Options** | **Description**                                                      |
| -------------------- | -------------------------------------------------------------------- |
| `-sn`                | Disables port scanning.                                              |
| `-oA tnet`           | Stores the results in all formats starting with the name 'tnet'.     |
| `-iL`                | Performs defined scans against targets in provided 'hosts.lst' list. |



