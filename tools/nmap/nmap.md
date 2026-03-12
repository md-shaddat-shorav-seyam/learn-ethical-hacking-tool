Since you're interested in **ethical hacking, networking, and security** (you were exploring router firmware and HackTheBox earlier), learning **Nmap deeply** will be extremely useful for you. Nmap is one of the **most important tools for penetration testing and reconnaissance**.

I'll give you a **complete start-to-advanced guide with real-world use cases**. 🚀

---

# Nmap: Complete Start-to-End Guide (with Real-World Examples)

## 1. What is Nmap

**Nmap (Network Mapper)** is an open-source tool used for:

* Network discovery
* Port scanning
* Service detection
* OS detection
* Vulnerability discovery
* Security auditing

Used by:

* Ethical hackers
* Penetration testers
* Network administrators
* Security researchers

Official site:

* Nmap

---

# 2. Installation

### Linux (Kali / Parrot / Ubuntu)

```bash
sudo apt update
sudo apt install nmap
```

Check installation:

```bash
nmap --version
```

Example output

```
Nmap version 7.94
```

---

# 3. Basic Syntax

```
nmap [scan type] [options] target
```

Example

```
nmap 192.168.1.1
```

Components:

| Part      | Meaning                          |
| --------- | -------------------------------- |
| scan type | TCP / SYN / UDP                  |
| options   | OS detection / service detection |
| target    | IP / domain / network            |

---

# 4. First Scan (Basic Port Scan)

```
nmap 192.168.1.10
```

Output example:

```
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
443/tcp open https
```

Meaning:

| Field   | Meaning                  |
| ------- | ------------------------ |
| PORT    | Port number              |
| STATE   | open / closed / filtered |
| SERVICE | service running          |

---

# 5. Scan Multiple Targets

### Multiple IPs

```
nmap 192.168.1.1 192.168.1.5
```

### Subnet scan

```
nmap 192.168.1.0/24
```

Scan entire network.

---

# 6. Scan Specific Ports

```
nmap -p 80 192.168.1.1
```

Multiple ports

```
nmap -p 22,80,443 192.168.1.1
```

Range

```
nmap -p 1-1000 192.168.1.1
```

---

# 7. Scan All Ports

```
nmap -p- 192.168.1.1
```

This scans:

```
1 → 65535
```

Important in penetration testing.

---

# 8. SYN Scan (Stealth Scan)

```
nmap -sS 192.168.1.1
```

Features:

* Fast
* Stealthy
* Default in root mode

Works by sending **TCP SYN packets**.

---

# 9. TCP Connect Scan

```
nmap -sT 192.168.1.1
```

Used when:

* No root privileges.

---

# 10. UDP Scan

```
nmap -sU 192.168.1.1
```

Finds services like:

| Port | Service |
| ---- | ------- |
| 53   | DNS     |
| 161  | SNMP    |
| 69   | TFTP    |

UDP scanning is **slower**.

---

# 11. Service Version Detection

```
nmap -sV 192.168.1.1
```

Example output:

```
21/tcp open ftp vsftpd 3.0.3
22/tcp open ssh OpenSSH 7.6
80/tcp open http Apache 2.4.29
```

Important for **vulnerability discovery**.

---

# 12. OS Detection

```
nmap -O 192.168.1.1
```

Example:

```
OS details: Linux 4.15 - 5.4
```

---

# 13. Aggressive Scan

```
nmap -A 192.168.1.1
```

Includes:

* OS detection
* Version detection
* Script scanning
* Traceroute

---

# 14. Fast Scan

```
nmap -F 192.168.1.1
```

Scans top **100 ports only**.

---

# 15. Scan Speed Control

```
nmap -T0
nmap -T1
nmap -T2
nmap -T3
nmap -T4
nmap -T5
```

| Mode | Meaning         |
| ---- | --------------- |
| T0   | extremely slow  |
| T1   | sneaky          |
| T3   | normal          |
| T4   | fast            |
| T5   | very aggressive |

Example

```
nmap -T4 192.168.1.1
```

---

# 16. Detect Firewall

```
nmap -sA 192.168.1.1
```

ACK scan checks firewall rules.

---

# 17. NSE Scripts (Powerful Feature)

Nmap has **NSE (Nmap Scripting Engine)**.

Example:

```
nmap --script vuln 192.168.1.1
```

Scans for vulnerabilities.

Example categories:

| Script  | Purpose         |
| ------- | --------------- |
| vuln    | vulnerabilities |
| exploit | exploitation    |
| auth    | authentication  |
| brute   | brute force     |

Example

```
nmap --script ftp-anon 192.168.1.10
```

---

# 18. Output Results

### Normal output

```
nmap -oN result.txt 192.168.1.1
```

### XML output

```
nmap -oX result.xml 192.168.1.1
```

### All formats

```
nmap -oA scan 192.168.1.1
```

Creates:

```
scan.nmap
scan.xml
scan.gnmap
```

---

# 19. Real World Penetration Testing Workflow

### Step 1 — Host Discovery

```
nmap -sn 192.168.1.0/24
```

Find live hosts.

---

### Step 2 — Port Scan

```
nmap -p- 192.168.1.10
```

Find open ports.

---

### Step 3 — Service Detection

```
nmap -sV -p 22,80,443 192.168.1.10
```

---

### Step 4 — Vulnerability Scan

```
nmap --script vuln 192.168.1.10
```

---

### Step 5 — Exploitation

Use tools like:

* Metasploit Framework
* Burp Suite

---

# 20. Real World Example (HackTheBox / CTF)

Example scan:

```
nmap -sC -sV -oA scan 10.10.10.5
```

Output:

```
21/tcp open ftp vsftpd 2.3.4
22/tcp open ssh OpenSSH 7.4
80/tcp open http Apache 2.4
```

Analysis:

| Port | Attack possibility |
| ---- | ------------------ |
| FTP  | anonymous login    |
| SSH  | brute force        |
| HTTP | web vulnerability  |

Next steps:

```
ftp 10.10.10.5
```

Check anonymous login.

---

# 21. Advanced Scan

Full professional scan:

```
nmap -p- -sS -sV -sC -O -T4 192.168.1.10
```

This performs:

* Full port scan
* SYN scan
* service detection
* default scripts
* OS detection

---

# 22. Automation Example

Simple bash script:

```bash
#!/bin/bash

target=$1

echo "Starting scan on $target"

nmap -p- $target -oN ports.txt
nmap -sV $target -oN services.txt
nmap --script vuln $target -oN vuln.txt
```

Run:

```
./scan.sh 192.168.1.10
```

---

# 23. Common Professional Commands

Most used by pentesters:

```
nmap -sC -sV target
```

```
nmap -p- target
```

```
nmap -A target
```

```
nmap --script vuln target
```

---

# 24. Legal Warning ⚠️

Never scan networks without permission.

Legal targets:

* HackTheBox
* TryHackMe
* Your own lab
* Bug bounty targets

---

# 25. Practice Labs (Best for Learning)

Practice on:

* Hack The Box
* TryHackMe
* Metasploitable

---

# If you want, I can also show you:

* **Top 30 Nmap commands used by professional hackers**
* **How hackers scan networks without being detected**
* **How to combine Nmap with Python automation**
* **Nmap + vulnerability exploitation workflow**

These will push your **ethical hacking skills to professional level.** 🔥
