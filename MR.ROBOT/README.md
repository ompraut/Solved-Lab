# Mr. Robot CTF Writeup

## Machine Information

* Platform: VulnHub
* Machine: Mr. Robot
* Difficulty: Beginner to Intermediate

## Tools Used

* Nmap
* Gobuster
* Wordlists
* Reverse Shell
* Nmap
* Linux Terminal

## Enumeration

### Host Discovery

Identified the target machine and performed port and service enumeration.

```bash
nmap -sC -sV <target-ip>
```

### Open Services

* HTTP

## Web Enumeration

Visited the target website and examined the robots.txt file.

Discovered:

* Key 1
* Username list
* Password wordlist

## Directory Enumeration

Performed directory brute forcing and discovered the WordPress login page.

```bash
gobuster dir -u http://<target-ip> -w <wordlist>
```

Discovered:

```text
/wp-login
```

## Authentication

Sorted and analyzed the discovered username and password files.

Successfully identified valid credentials and logged into the WordPress administration panel.

## Initial Access

Navigated to:

```text
Appearance → Editor
```

Edited the 404 template and inserted a PHP reverse shell.

Configured the reverse shell with the attacker's IP address and listening port.

Triggered the shell and obtained remote access.

## Post Exploitation

Navigated to:

```bash
/home/robot
```

Discovered:

* key-2
* MD5 password hash

Cracked the password hash and obtained credentials for the user:

```text
robot
```

## Privilege Escalation

Logged in as robot and performed local enumeration.

Identified that Nmap could be used interactively.

Checked GTFOBins and used the Nmap interactive shell technique to escape to a shell.

```bash
nmap --interactive
```

```bash
!sh
```

Successfully obtained root privileges.

## Root Access

Navigated to:

```bash
cd /root
```

Retrieved the final key.

## Flags Collected

* Key 1
* Key 2
* Key 3 (Root)

## Lessons Learned

* robots.txt enumeration
* WordPress exploitation
* Reverse shell deployment
* Password hash cracking
* Linux privilege escalation using Nmap
* GTFOBins methodology
