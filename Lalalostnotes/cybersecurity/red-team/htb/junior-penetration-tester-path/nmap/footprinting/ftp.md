# FTP

### FTP

* Oldest protocol the internet
* Establish connection between client  control channel in the port TCP 21 and data channel in TCP 20
* [https://web.archive.org/web/20230326204635/https://www.smartfile.com/blog/the-ultimate-ftp-commands-list/](https://web.archive.org/web/20230326204635/https://www.smartfile.com/blog/the-ultimate-ftp-commands-list/)

### TFTP

* `Trivial File Transfer Protocol` (`TFTP`) is simpler than FTP and performs file transfers between client and server processes
*

Let us take a look at a few commands of `TFTP`:

| **Commands** | **Description**                                                                                                                        |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| `connect`    | Sets the remote host, and optionally the port, for file transfers.                                                                     |
| `get`        | Transfers a file or set of files from the remote host to the local host.                                                               |
| `put`        | Transfers a file or set of files from the local host onto the remote host.                                                             |
| `quit`       | Exits tftp.                                                                                                                            |
| `status`     | Shows the current status of tftp, including the current transfer mode (ascii or binary), connection status, time-out value, and so on. |
| `verbose`    | Turns verbose mode, which displays additional information during file transfer, on or off.                                             |

### Default Configuration

* One of the most used FTP servers on Linux-based distributions is [vsFTPd](https://security.appspot.com/vsftpd.html)

### **Install vsFTPd**

```shell-session
lalalostcode@htb[/htb]$ sudo apt install vsftpd 
```

### **vsFTPd Config File**

```shell-session
lalalostcode@htb[/htb]$ cat /etc/vsftpd.conf | grep -v "#"
```

| **Setting**                                                   | **Description**                                                                                          |
| ------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `listen=NO`                                                   | Run from inetd or as a standalone daemon?                                                                |
| `listen_ipv6=YES`                                             | Listen on IPv6 ?                                                                                         |
| `anonymous_enable=NO`                                         | Enable Anonymous access?                                                                                 |
| `local_enable=YES`                                            | Allow local users to login?                                                                              |
| `dirmessage_enable=YES`                                       | Display active directory messages when users go into certain directories?                                |
| `use_localtime=YES`                                           | Use local time?                                                                                          |
| `xferlog_enable=YES`                                          | Activate logging of uploads/downloads?                                                                   |
| `connect_from_port_20=YES`                                    | Connect from port 20?                                                                                    |
| `secure_chroot_dir=/var/run/vsftpd/empty`                     | Name of an empty directory                                                                               |
| `pam_service_name=vsftpd`                                     | This string is the name of the PAM service vsftpd will use.                                              |
| `rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem`          | The last three options specify the location of the RSA certificate to use for SSL encrypted connections. |
| `rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key` |                                                                                                          |
| `ssl_enable=NO`                                               |                                                                                                          |

* [http://vsftpd.beasts.org/vsftpd\_conf.html](http://vsftpd.beasts.org/vsftpd_conf.html)

### Dangerous Settings

| **Setting**                    | **Description**                                                                    |
| ------------------------------ | ---------------------------------------------------------------------------------- |
| `anonymous_enable=YES`         | Allowing anonymous login?                                                          |
| `anon_upload_enable=YES`       | Allowing anonymous to upload files?                                                |
| `anon_mkdir_write_enable=YES`  | Allowing anonymous to create new directories?                                      |
| `no_anon_password=YES`         | Do not ask anonymous for password?                                                 |
| `anon_root=/home/username/ftp` | Directory for anonymous.                                                           |
| `write_enable=YES`             | Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE? |

**Anonymous Login**

```shell-session
lalalostcode@htb[/htb]$ ftp 10.129.14.136

Connected to 10.129.14.136.
220 "Welcome to the HTB Academy vsFTP service."
Name (10.129.14.136:cry0l1t3): anonymous

230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

ftp> ls

200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-rw-r--    1 1002     1002      8138592 Sep 14 16:54 Calender.pptx
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Clients
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Documents
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Employees
-rw-rw-r--    1 1002     1002           41 Sep 14 16:45 Important Notes.txt
226 Directory send OK.
```

**vsFTPd Status**

```shell-session
ftp> status

Connected to 10.129.14.136.
No proxy connection.
Connecting using address family: any.
Mode: stream; Type: binary; Form: non-print; Structure: file
Verbose: on; Bell: off; Prompting: on; Globbing: on
Store unique: off; Receive unique: off
Case: off; CR stripping: on
Quote control characters: on
Ntrans: off
Nmap: off
Hash mark printing: off; Use of PORT cmds: on
Tick counter printing: off

```

| **Setting**               | **Description**                                                        |
| ------------------------- | ---------------------------------------------------------------------- |
| `dirmessage_enable=YES`   | Show a message when they first enter a new directory?                  |
| `chown_uploads=YES`       | Change ownership of anonymously uploaded files?                        |
| `chown_username=username` | User who is given ownership of anonymously uploaded files.             |
| `local_enable=YES`        | Enable local users to login?                                           |
| `chroot_local_user=YES`   | Place local users into their home directory?                           |
| `chroot_list_enable=YES`  | Use a list of local users that will be placed in their home directory? |

| **Setting**             | **Description**                                                                  |
| ----------------------- | -------------------------------------------------------------------------------- |
| `hide_ids=YES`          | All user and group information in directory listings will be displayed as "ftp". |
| `ls_recurse_enable=YES` | Allows the use of recurse listings.                                              |

**Hiding IDs - YES**

```shell-session
ftp> ls

---> TYPE A
200 Switching to ASCII mode.
ftp: setsockopt (ignored): Permission denied
---> PORT 10,10,14,4,223,101
200 PORT command successful. Consider using PASV.
---> LIST
150 Here comes the directory listing.
-rw-rw-r--    1 ftp     ftp      8138592 Sep 14 16:54 Calender.pptx
drwxrwxr-x    2 ftp     ftp         4096 Sep 14 17:03 Clients
drwxrwxr-x    2 ftp     ftp         4096 Sep 14 16:50 Documents
drwxrwxr-x    2 ftp     ftp         4096 Sep 14 16:50 Employees
-rw-rw-r--    1 ftp     ftp           41 Sep 14 16:45 Important Notes.txt
-rw-------    1 ftp     ftp            0 Sep 15 14:57 testupload.txt
```

### Footprinting the Service

**Nmap FTP Scripts**

```shell-session
lalalostcode@htb[/htb]$ sudo nmap --script-updatedb

Starting Nmap 7.80 ( https://nmap.org ) at 2021-09-19 13:49 CEST
NSE: Updating rule database.
NSE: Script Database updated successfully.
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.28 seconds

lalalostcode@htb[/htb]$ find / -type f -name ftp* 2>/dev/null | grep scripts

/usr/share/nmap/scripts/ftp-syst.nse
/usr/share/nmap/scripts/ftp-vsftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-vuln-cve2010-4221.nse
/usr/share/nmap/scripts/ftp-proftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-bounce.nse
/usr/share/nmap/scripts/ftp-libopie.nse
/usr/share/nmap/scripts/ftp-anon.nse
/usr/share/nmap/scripts/ftp-brute.nse


```

**Nmap**

```shell-session
lalalostcode@htb[/htb]$ sudo nmap -sV -p21 -sC -A 10.129.14.136

Starting Nmap 7.80 ( https://nmap.org ) at 2021-09-16 18:12 CEST
Nmap scan report for 10.129.14.136
Host is up (0.00013s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.0.8 or later
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rwxrwxrwx    1 ftp      ftp       8138592 Sep 16 17:24 Calendar.pptx [NSE: writeable]
| drwxrwxrwx    4 ftp      ftp          4096 Sep 16 17:57 Clients [NSE: writeable]
| drwxrwxrwx    2 ftp      ftp          4096 Sep 16 18:05 Documents [NSE: writeable]
| drwxrwxrwx    2 ftp      ftp          4096 Sep 16 17:24 Employees [NSE: writeable]
| -rwxrwxrwx    1 ftp      ftp            41 Sep 16 17:24 Important Notes.txt [NSE: writeable]
|_-rwxrwxrwx    1 ftp      ftp             0 Sep 15 14:57 testupload.txt [NSE: writeable]
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.14.4
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
```

**Nmap Script Trace**

```shell-session
lalalostcode@htb[/htb]$ sudo nmap -sV -p21 -sC -A 10.129.14.136 --script-trace

Starting Nmap 7.80 ( https://nmap.org ) at 2021-09-19 13:54 CEST                                                                                                                                                   
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 8 [10.129.14.136:21]                                   
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 16 [10.129.14.136:21]             
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 24 [10.129.14.136:21]
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 32 [10.129.14.136:21]
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #1 [10.129.14.136:21] (timeout: 7000ms) EID 42
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #2 [10.129.14.136:21] (timeout: 9000ms) EID 50
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #3 [10.129.14.136:21] (timeout: 7000ms) EID 58
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #4 [10.129.14.136:21] (timeout: 11000ms) EID 66
NSE: TCP 10.10.14.4:54226 > 10.129.14.136:21 | CONNECT
NSE: TCP 10.10.14.4:54228 > 10.129.14.136:21 | CONNECT
NSE: TCP 10.10.14.4:54230 > 10.129.14.136:21 | CONNECT
NSE: TCP 10.10.14.4:54232 > 10.129.14.136:21 | CONNECT
NSOCK INFO [11.4660s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 50 [10.129.14.136:21] (41 bytes): 220 Welcome to HTB-Academy FTP service...
NSOCK INFO [11.4660s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 58 [10.129.14.136:21] (41 bytes): 220 Welcome to HTB-Academy FTP service...
NSE: TCP 10.10.14.4:54228 < 10.129.14.136:21 | 220 Welcome to HTB-Academy FTP service.
```
