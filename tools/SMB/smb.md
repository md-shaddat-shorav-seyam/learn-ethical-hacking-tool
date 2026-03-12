## SMB (Server Message Block) – Complete Guide (Start → Advanced → Real-World Use Cases)

![Image](https://images.openai.com/static-rsc-3/nFF31ig6hgZg_gvu3FX057Ab9kDft3hKMukre56rd6gDPHpyPVCSgJ2jlWr__QAvfan21A6i6POelyTHLoj4_eHQYOoaXFPfclTfUVgrhn0?purpose=fullsize\&v=1)

![Image](https://docs.oracle.com/cd/E23824_01/html/821-1449/figures/SolarisCIFSDiagram.png)

![Image](https://www.techtarget.com/rms/onlineImages/networking-smb_mobile.jpg)

![Image](https://visualitynq.com/app/uploads/SMB1-copy-clear-letters.jpg)

**SMB (Server Message Block)** is a **network file sharing protocol** used for communication between computers in a network.

It allows systems to:

* Share **files**
* Share **printers**
* Access **network drives**
* Communicate with **remote services**

It is heavily used in **Windows networks**.

---

# 1. What SMB Stands For

**SMB = Server Message Block**

Developed by **IBM** and later extended by **Microsoft**.

Modern version is called **CIFS (Common Internet File System)**.

---

# 2. What SMB Is Used For

Main purposes:

1. **File sharing**
2. **Printer sharing**
3. **Remote file access**
4. **Network drive mounting**
5. **Authentication in Windows domains**

Example:

A company server shares a folder:

```
\\fileserver\documents
```

Employees access it from their computers.

---

# 3. SMB Architecture

SMB follows a **Client-Server Model**.

### SMB Server

Provides shared resources.

Examples:

* Windows Server
* Linux Samba server
* NAS devices

### SMB Client

Requests access.

Examples:

* Windows PC
* Linux machine
* MacOS

---

# 4. SMB Versions

| Version | Year | Features               |
| ------- | ---- | ---------------------- |
| SMB1    | 1984 | Original protocol      |
| SMB2    | 2006 | Faster, fewer commands |
| SMB3    | 2012 | Encryption, security   |

Important:

**SMB1 is vulnerable and should be disabled.**

Example vulnerability:

* **WannaCry ransomware**

---

# 5. SMB Default Ports

| Port | Protocol | Usage            |
| ---- | -------- | ---------------- |
| 445  | TCP      | Direct SMB       |
| 139  | TCP      | NetBIOS SMB      |
| 137  | UDP      | NetBIOS Name     |
| 138  | UDP      | NetBIOS Datagram |

Example:

```
445/tcp open  microsoft-ds
139/tcp open  netbios-ssn
```

---

# 6. SMB URL Format

SMB path format:

```
\\SERVER\SHARE
```

Example:

```
\\192.168.1.10\backup
```

Linux format:

```
smb://192.168.1.10/share
```

---

# 7. Installing SMB Server (Linux)

Linux uses **Samba**.

### Install Samba

Ubuntu / Parrot / Kali:

```bash
sudo apt update
sudo apt install samba
```

Check version:

```bash
smbd --version
```

---

# 8. Creating a Shared Folder

Create folder:

```bash
mkdir /shared
```

Edit config:

```bash
sudo nano /etc/samba/smb.conf
```

Add:

```
[shared]
path = /shared
read only = no
guest ok = yes
```

Restart Samba:

```bash
sudo systemctl restart smbd
```

Now access:

```
\\SERVER_IP\shared
```

---

# 9. Access SMB From Linux

Install client:

```bash
sudo apt install smbclient
```

List shares:

```bash
smbclient -L //192.168.1.10
```

Connect:

```bash
smbclient //192.168.1.10/shared
```

---

# 10. Basic SMB Commands

Inside smbclient:

| Command  | Function         |
| -------- | ---------------- |
| ls       | list files       |
| get file | download         |
| put file | upload           |
| cd       | change directory |
| exit     | quit             |

Example:

```
smb: \> ls
smb: \> get secret.txt
```

---

# 11. SMB Enumeration (Cybersecurity)

Since you are learning **web security / hacking**, SMB is very important in **penetration testing**.

Tools used:

* Nmap
* smbclient
* enum4linux
* crackmapexec
* smbmap

---

# 12. Discover SMB With Nmap

Scan SMB ports:

```bash
nmap -p445 192.168.1.10
```

Result:

```
PORT    STATE SERVICE
445/tcp open  microsoft-ds
```

---

### SMB Version Detection

```bash
nmap -p445 --script smb-protocols 192.168.1.10
```

Output:

```
SMBv1 enabled
SMBv2 enabled
SMBv3 enabled
```

---

### List SMB Shares

```bash
nmap --script smb-enum-shares -p445 192.168.1.10
```

Example output:

```
ADMIN$
IPC$
Users
Public
```

---

# 13. Anonymous SMB Login

Some servers allow **guest login**.

Example:

```bash
smbclient -L //192.168.1.10 -N
```

`-N` means **no password**.

If allowed, you can access:

```
smbclient //192.168.1.10/public -N
```

---

# 14. SMB Brute Force

Tools:

### Hydra

```bash
hydra -l admin -P passwords.txt smb://192.168.1.10
```

### CrackMapExec

```bash
crackmapexec smb 192.168.1.10 -u admin -p password
```

---

# 15. Real World Case (HackTheBox Example)

Since you are using **HTB**, SMB appears in many machines.

Example target:

```
10.129.1.10
```

### Step 1 – Scan

```
nmap -sC -sV 10.129.1.10
```

Output:

```
445/tcp open  microsoft-ds
```

---

### Step 2 – Enumerate Shares

```
smbclient -L //10.129.1.10
```

Output:

```
Sharename       Type
---------       ----
ADMIN$
Users
Public
IPC$
```

---

### Step 3 – Access Public Share

```
smbclient //10.129.1.10/Public
```

---

### Step 4 – Download Files

```
get flag.txt
```

---

### Result

You retrieve the flag:

```
HTB{example_flag}
```

---

# 16. Famous SMB Attacks

## 1. EternalBlue

Used in:

* **WannaCry ransomware**

Exploit:

```
MS17-010
```

Metasploit module:

```
exploit/windows/smb/ms17_010_eternalblue
```

---

## 2. Pass The Hash

Attackers use **NTLM hash instead of password**.

Tool:

```
crackmapexec
```

Example:

```
crackmapexec smb 192.168.1.10 -u admin -H HASH
```

---

# 17. SMB Security Best Practices

Always:

* Disable **SMBv1**
* Use **SMB encryption**
* Strong passwords
* Restrict network access
* Monitor logs

---

# 18. Real Corporate Use Case

Example company network:

```
Employee PC
      |
      |
File Server (SMB)
      |
      |
Shared Documents
```

Employees access:

```
\\company-server\projects
```

Uses:

* accounting files
* HR documents
* backups

---

# 19. SMB vs FTP

| Feature        | SMB              | FTP               |
| -------------- | ---------------- | ----------------- |
| OS             | Windows networks | Internet          |
| Authentication | NTLM/Kerberos    | Username/password |
| Security       | SMB3 encryption  | Usually plaintext |
| Use case       | Internal network | File transfer     |

---

# 20. Tools for SMB

| Tool         | Purpose          |
| ------------ | ---------------- |
| smbclient    | connect SMB      |
| smbmap       | enumerate shares |
| enum4linux   | user enumeration |
| crackmapexec | pentesting       |
| metasploit   | exploitation     |

---

# 21. Quick SMB Pentesting Workflow

Typical workflow:

### 1 Scan

```
nmap -p445 -sV target
```

### 2 Enumerate

```
smbclient -L //target
```

### 3 Anonymous access

```
smbclient //target/share -N
```

### 4 Enumerate users

```
enum4linux target
```

### 5 Password attack

```
hydra smb://target
```

---

# Final Summary

SMB is used for:

* File sharing
* Printer sharing
* Network drives
* Windows authentication

But in **cybersecurity**, SMB is also a **major attack surface**.

Many pentesting labs (like **HackTheBox**) include SMB vulnerabilities.

---

✅ If you want, I can also show you:

* **Complete SMB hacking methodology (used by real pentesters)**
* **SMB enumeration cheat sheet for HTB**
* **10 SMB attacks every ethical hacker must know**
* **Full HTB walkthrough using SMB**.
