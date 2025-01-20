---
description: >-
  Think you've got what it takes to outsmart the Hack Smarter Security team?
  They claim to be unbeatable, and now it's your chance to prove them wrong.
  Dive into their web server, find the hidden flags.
---

# Silver Plate

{% hint style="info" %}
You can access the challenges on [Tryhackme](https://tryhackme.com/r/room/silverplatter)
{% endhint %}

Table of contents

* Enumeration
* SSH (20)
* HTTP (80)
* HTTPS (8080)
* Post Exploitation

## Enumeration

### Nmaps Scan

{% tabs %}
{% tab title="Rustscan" %}
```bash
rustscan silverplatter.thm -- -A  
```
{% endtab %}

{% tab title="Nmap" %}
```bash
nmap -p- silverplatter.thm -Pn  -T5 -v 
```
{% endtab %}

{% tab title="Nmap & Ports" %}
```bash
nmap -p 20,80,8080 -A silverplatter.thm -Pn -v 
```
{% endtab %}
{% endtabs %}

The `-Pn` flag tells `nmap` to **skip host discovery** and assume that all hosts are online.\
By default, `nmap` performs a "ping" to check if a host is alive before scanning. With `-Pn`, it skips this step.

<details>

<summary><strong>When to Use</strong>:</summary>

* If the target is configured to block ICMP requests (ping).
* To force `nmap` to scan all targets regardless of their response to ping.

</details>

The `-A` flag in `nmap` is used for **Advanced Scanning**

<details>

<summary>When to Use:</summary>



Use the `-A` flag in the following scenarios:

**1. When You Need Detailed Information About a Target**

* You want to identify the operating system, service versions, and potentially additional metadata about the target.
* Ideal for penetration testing or vulnerability assessments.

**2. When Performing Reconnaissance on a Network**

* Provides a comprehensive view of the devices and services present in a network.
* Useful in both internal and external reconnaissance during ethical hacking.

**3. When Running Default NSE Scripts**

* The `-A` flag includes default Nmap scripts, which can identify:
  * Vulnerabilities.
  * Misconfigurations.
  * Service banners.

**4. When Tracing the Network Path**

* If you're interested in understanding the network route to the target (e.g., for troubleshooting or mapping the network), `-A` automatically includes **traceroute**.

**5. When Speed and Stealth Are Not Priorities**

* The `-A` flag increases scan time significantly.
* It is more intrusive and can trigger alarms in Intrusion Detection Systems (IDS).

</details>

```bash
PORT     STATE SERVICE    REASON         VERSION
22/tcp   open  ssh        syn-ack ttl 61 OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 1b:1c:87:8a:fe:34:16:c9:f7:82:37:2b:10:8f:8b:f1 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ0ia1tcuNvK0lfuy3Ep2dsElFfxouO3VghX5Rltu77M33pFvTeCn9t5A8NReq3felAqPi+p+/0eRRfYuaeHRT4=
|   256 26:6d:17:ed:83:9e:4f:2d:f6:cd:53:17:c8:80:3d:09 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKecigNtiy6tW5ojXM3xQkbtTOwK+vqvMoJZnIxVowju
80/tcp   open  http       syn-ack ttl 61 nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
8080/tcp open  http-proxy syn-ack ttl 60
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 404 Not Found
|     Connection: close
|     Content-Length: 74
|     Content-Type: text/html
|     Date: Wed, 15 Jan 2025 14:50:16 GMT
|     <html><head><title>Error</title></head><body>404 - Not Found</body></html>
​
```

Some ports are open:

| PORTS | PROTOCOL     | NAME         |
| ----- | ------------ | ------------ |
| 22    | TCP/UDP/SCTP | ssh/scp/sftp |
| 80    | HTTP         | ​            |
| 8008  | TCP          | http proxy   |

### SSH (22)

* Password auth is enabled can't pass the root&#x20;

```bash
└─$ ssh root@silverplatter.thm
The authenticity of host 'silverplatter.thm (10.10.28.15)' can't be established.
ED25519 key fingerprint is SHA256:WFcHcO+oxUb2E/NaonaHAgqSK3bp9FP8hsg5z2pkhuE.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'silverplatter.thm' (ED25519) to the list of known hosts.
root@silverplatter.thm's password: 
```

### HTTP (80)

Directory fuzz using dirsearch

```bash
Target: http://silverplatter.thm/

[00:22:42] Starting: 
[00:24:19] 301 -  178B  - /assets  ->  http://silverplatter.thm/assets/
[00:24:19] 403 -  564B  - /assets/
[00:25:25] 301 -  178B  - /images  ->  http://silverplatter.thm/images/
[00:25:25] 403 -  564B  - /images/
[00:25:42] 200 -   17KB - /LICENSE.txt
[00:26:31] 200 -  771B  - /README.txt

## Nothing interesting
```

Vhost

```bash
## Nothing interesting
```

Website Features and Notes

<figure><img src="../../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

To-do list

{% stepper %}
{% step %}
Enumerate what is **Silverpeas** is
{% endstep %}

{% step %}
Investigate username `"scriptkiddy"`
{% endstep %}
{% endstepper %}

### HTTP (8080)

**What is Silverpeas ?**

Comment​[Silverpeas](https://www.silverpeas.org/)⁠ is a Collaborative and Social-Networking Portal built to facilitate and to leverage the collaboration, the knowledge-sharing and the feedback of persons, teams and organizations.Comment

* Accesible at `http://silverplatter.thm:8080/silverpeas`

<figure><img src="../../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

How to Exploit ?

There are some resources from google that may help about how to exploit it:

* [https://rhinosecuritylabs.com/research/silverpeas-file-read-cves/](https://rhinosecuritylabs.com/research/silverpeas-file-read-cves/)
* [https://github.com/advisories/GHSA-4w54-wwc9-x62c](https://github.com/advisories/GHSA-4w54-wwc9-x62c)
* [https://nvd.nist.gov/vuln/detail/CVE-2024-36042](https://nvd.nist.gov/vuln/detail/CVE-2024-36042)

{% hint style="info" %}
Hint from Description Challenges
{% endhint %}

<figure><img src="../../../../.gitbook/assets/image (12).png" alt=""><figcaption><p>Source the Description of the Challenges</p></figcaption></figure>

Notice that authentication cannot be bypassed using the rockyou.txt wordlist. The author provided a hint with the keyword **"cool"**. We investigated further and discovered another helpful tool called [CewL](https://github.com/digininja/CeWL), random password generator.

```bash
┌──(lalalostkali㉿ChillKaGuy)-[~/Downloads]
└─$ cewl http://silverplatter.thm > passsilverpeas.txt
```

Caido Time!

Follow these steps:

1. Intercept the login form
2. Send it through automate
3. Fuzz with previous file from cewl

<figure><img src="../../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

4. Hit run!

<figure><img src="../../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

4. Successfull authentication

```bash
scr1ptkiddy:adipiscing
```

{% hint style="warning" %}
Pay attention to the previous resource to understand how to exploit it. We can modify the ID parameters for this purpose.
{% endhint %}

<figure><img src="../../../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

We got the ssh credentials!

```bash
──(lalalostkali㉿ChillKaGuy)-[~/Downloads]
└─$ ssh tim@silverplatter.thm  
tim@silverplatter.thm's password: 
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-91-generic x86_64)
tim@silver-platter:~$ whoami
tim
tim@silver-platter:~$ ls
user.txt
tim@silver-platter:~$ cat user.txt 
```

<details>

<summary>First flag</summary>

```
THM{c4ca4238a0b923820dcc509a6f75849b}
```

</details>

Finally we go the user flag!

## Post-Exploitation

```bash
tim@silver-platter:~$ id
uid=1001(tim) gid=1001(tim) groups=1001(tim),4(adm)
```

<details>

<summary>Quick Explanations</summary>

* **`uid` (User ID)**: The unique identifier for the user account.

- **`gid` (Group ID)**: The primary group associated with the user.

* **`groups`**: Lists all groups the user is a member of, including the primary group.

- **Group names (e.g., `adm`)**: Group adm is used for system monitoring tasks. Members of this group can read many log files in /var/log, and can use xconsole.

</details>

So, we know that we can read the `/var/log` file

```bash
tim@silver-platter:~$ cd /var/log
tim@silver-platter:/var/log$ ls
alternatives.log    apt		auth.log.2			   bootstrap.log  cloud-init.log	 dmesg	     dmesg.2.gz  dpkg.log	faillog    kern.log	  kern.log.3.gz  nginx	  syslog.1     ubuntu-advantage.log
alternatives.log.1  auth.log	auth.log.2.gz			   btmp		  cloud-init-output.log  dmesg.0     dmesg.3.gz  dpkg.log.1	installer  kern.log.1	  landscape	 private  syslog.2.gz  unattended-upgrades
amazon		    auth.log.1	aws114_ssm_agent_installation.log  btmp.1	  dist-upgrade		 dmesg.1.gz  dmesg.4.gz  dpkg.log.2.gz	journal    kern.log.2.gz  lastlog	 syslog   syslog.3.gz  wtmp
tim@silver-platter:/var/log$ grep -ir "password"
```

We got the mysterious password, should we pass the root?

```bash
tim@silver-platter:/var/log$ su root
Password: 
su: Authentication failure
tim@silver-platter:/var/log$ su tyler
Password: 
tyler@silver-platter:/var/log$ 
tyler@silver-platter:/var/log$ sudo su
[sudo] password for tyler: 
Sorry, try again.
[sudo] password for tyler: 
Sorry, try again.
[sudo] password for tyler: 
root@silver-platter:/var/log# ls -la
total 2152
root@silver-platter:~# whoami
root
root@silver-platter:~# ls
root.txt  snap  start_docker_containers.sh
root@silver-platter:~# cat root.txt 
```

<details>

<summary>Finally we got the last flag!</summary>

```
THM{098f6bcd4621d373cade4e832627b4f6}
```

</details>

HAPPY HACKING :tada:

