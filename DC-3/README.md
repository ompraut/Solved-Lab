# DC3 Writeup

## Machine Information

* Platform: VulnHub
* Machine: DC3
* Difficulty: Beginner

## Tools Used

* Nmap
* Burp Suite
* Searchsploit
* Linux Terminal

## Enumeration

### Host Discovery

Discovered the target machine on the network using Nmap.

```bash
nmap -sn <network>
```

### Port and Service Enumeration

Performed a detailed scan to identify open ports, services, and versions.

```bash
nmap -sV -sC <target-ip>
```

### Open Ports

* 80/tcp - HTTP

  <img width="638" height="765" alt="image" src="https://github.com/user-attachments/assets/6cd1684b-ccae-41a6-ab91-b0cb4dc8ca32" />


The web server was hosting a Joomla Content Management System (CMS).

## Joomla Enumeration

Used Nmap scripts to gather additional information.

```bash
nmap -p80 --script http-joomla-brute <target-ip>
```

```bash
nmap --script http-enum <target-ip>
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8ea962d8-07ed-4bd6-afd6-44a32dc72a10" />


During enumeration, the Joomla administrator login page was discovered.

## Initial Access

Attempted authentication testing against the administrator portal.

After gaining access to the Joomla administration panel, the template editor was available.

A PHP reverse shell was inserted into an editable template file.

The reverse shell was configured with the attacker's IP address and listening port.

## Reverse Shell

Started a listener and triggered the reverse shell.

A shell connection was successfully established.

## Shell Stabilization

Spawned a proper interactive shell:

```bash
python -c 'import pty; pty.spawn("/bin/sh")'
```

## Privilege Escalation Enumeration

Enumerated SUID binaries:

```bash
find / -type f -perm -u=s 2>/dev/null
```

Collected operating system information:

```bash
lsb_release -a
```

The identified operating system version was searched in Searchsploit for known privilege escalation vulnerabilities.

## Local Privilege Escalation

Located a suitable exploit using Searchsploit.

Transferred the exploit to the target machine.

Extracted the archive:

```bash
tar -xvf <filename>
```

Compiled the exploit:

```bash
./compile.sh
```

Executed the exploit:

```bash
./doubleput
```

## Root Access

Successfully obtained root privileges.

<img width="649" height="717" alt="image" src="https://github.com/user-attachments/assets/e052386f-9dbf-4510-b572-f1cb1d492155" />


Navigated to the root directory and retrieved the final flag.

```bash
cd /root
ls
```


## Lessons Learned

* Joomla CMS enumeration
* Template-based code execution
* Reverse shell deployment
* Linux privilege escalation
* Exploit compilation and execution

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6df30496-3e0d-4348-a89a-8e2b53c42e74" />

<img width="638" height="746" alt="image" src="https://github.com/user-attachments/assets/1492e5ed-530c-4942-bff3-9989cad18ffa" />


