Below is a **complete beginner → advanced guide to FTP (File Transfer Protocol)** including **concepts, commands, servers, security issues, and real-world hacking/admin use cases** — useful for networking, DevOps, and cybersecurity (like your HTB labs).

---

# FTP (File Transfer Protocol) — Start to End Guide

## 1. What FTP Is

**FTP (File Transfer Protocol)** is a network protocol used to **transfer files between a client and a server over TCP/IP networks**.

Example:

```
Your PC (client)  --->  FTP Server
upload/download files
```

Common uses:

* Website file uploads
* Backup transfers
* Firmware distribution
* Internal company file sharing

Default port:

```
TCP 21
```

---

# 2. FTP Architecture

FTP uses **two channels**.

### 1️⃣ Control Channel

Used for commands.

```
Client → Server
Port: 21
```

Example commands:

```
USER
PASS
LIST
RETR
STOR
```

---

### 2️⃣ Data Channel

Used for file transfers.

```
Server → Client
```

Two modes:

| Mode    | Description                    |
| ------- | ------------------------------ |
| Active  | Server connects back to client |
| Passive | Client connects to server      |

Passive mode is more common today because of firewalls.

---

# 3. FTP Workflow

Typical FTP session:

```
1 Client connects to server
2 Server asks for username
3 Client sends username/password
4 Server authenticates
5 Client runs commands
6 File transfers occur
```

Example session:

```
Client → USER seyam
Server → 331 Password required

Client → PASS 1234
Server → 230 Login successful
```

---

# 4. Installing FTP

## Linux FTP client

```
sudo apt install ftp
```

or

```
sudo apt install lftp
```

---

# 5. Connecting to FTP Server

Basic connection:

```
ftp <ip-address>
```

Example:

```
ftp 192.168.1.10
```

Then login.

```
Name: admin
Password: admin123
```

---

# 6. FTP Basic Commands

| Command | Meaning                |
| ------- | ---------------------- |
| help    | show commands          |
| ls      | list files             |
| pwd     | show current directory |
| cd      | change directory       |
| get     | download file          |
| put     | upload file            |
| delete  | remove file            |
| mkdir   | create directory       |
| quit    | exit ftp               |

---

# 7. Important FTP Commands Explained

---

## help

Shows all commands.

```
ftp> help
```

or

```
ftp> ?
```

---

## ls

List files on server.

```
ftp> ls
```

Output:

```
backup.zip
config.txt
readme.md
```

---

## cd

Change directory.

```
ftp> cd uploads
```

---

## get

Download file.

```
ftp> get file.txt
```

File will download to your machine.

---

## mget

Download multiple files.

```
ftp> mget *.txt
```

---

## put

Upload file.

```
ftp> put shell.php
```

---

## delete

Remove file.

```
ftp> delete test.txt
```

---

## bye / quit

Exit FTP.

```
ftp> quit
```

---

# 8. Anonymous FTP

Some FTP servers allow login without account.

Username:

```
anonymous
```

Password:

```
anonymous
```

or email.

Example:

```
ftp 192.168.1.10
Name: anonymous
Password: anonymous
```

This is commonly seen in **CTF machines like HackTheBox Meow**.

---

# 9. FTP Using URL

You can download files directly.

Example:

```
ftp://user:pass@ip/file.txt
```

Example:

```
wget ftp://anonymous@192.168.1.10/readme.txt
```

---

# 10. FTP Server Setup

Most common FTP server in Linux:

```
vsftpd
```

Install:

```
sudo apt install vsftpd
```

Start service:

```
sudo systemctl start vsftpd
```

Enable auto start:

```
sudo systemctl enable vsftpd
```

---

# 11. FTP Server Configuration

Config file:

```
/etc/vsftpd.conf
```

Example settings:

```
anonymous_enable=YES
local_enable=YES
write_enable=YES
```

Restart server:

```
sudo systemctl restart vsftpd
```

---

# 12. FTP File Transfer Example

Upload file.

```
ftp 192.168.1.20
```

Login.

Then:

```
put website.zip
```

Download file.

```
get backup.tar
```

---

# 13. Automating FTP (Scripts)

Example Bash script.

```
ftp -n 192.168.1.10 <<EOF
user admin password
binary
put backup.tar
quit
EOF
```

Used in:

* automated backups
* log transfers
* system maintenance

---

# 14. FTP Security Issues

FTP is **insecure**.

Why?

Because:

```
username
password
files
```

are sent **in plain text**.

Example packet capture:

```
USER admin
PASS password123
```

Hackers can sniff this using:

```
Wireshark
tcpdump
```

---

# 15. Secure Alternatives

| Protocol | Description       |
| -------- | ----------------- |
| SFTP     | SSH file transfer |
| FTPS     | FTP with SSL      |
| SCP      | Secure copy       |

Example SFTP:

```
sftp user@server
```

---

# 16. FTP Enumeration (Ethical Hacking)

Hackers first scan the server.

Example:

```
nmap -sV 192.168.1.10
```

Output:

```
21/tcp open ftp vsftpd 3.0.3
```

Then check anonymous login.

```
ftp 192.168.1.10
```

Username:

```
anonymous
```

---

# 17. FTP Enumeration Using Nmap Scripts

```
nmap --script ftp* 192.168.1.10
```

Example scripts:

```
ftp-anon
ftp-syst
ftp-bounce
```

---

# 18. Real World Case — Website Defacement

### Scenario

A poorly configured FTP server allows upload.

Attacker logs in.

```
ftp target.com
```

Uploads:

```
put shell.php
```

Now attacker opens:

```
http://target.com/shell.php
```

Server compromised.

---

# 19. Real World Case — HackTheBox Meow Machine

Steps used:

1️⃣ Scan target

```
nmap -sC -sV 10.129.x.x
```

Result:

```
21/tcp open ftp
```

---

2️⃣ Check anonymous login

```
ftp 10.129.x.x
```

Username:

```
anonymous
```

---

3️⃣ List files

```
ls
```

Output:

```
flag.txt
```

---

4️⃣ Download file

```
get flag.txt
```

---

5️⃣ Read flag

```
cat flag.txt
```

---

# 20. FTP Brute Force Attack

Hackers try passwords.

Example with Hydra:

```
hydra -l admin -P rockyou.txt ftp://192.168.1.10
```

---

# 21. FTP Bounce Attack

Rare old attack.

Uses FTP server to scan other networks.

Example:

```
nmap -b ftpuser:pass@ftpserver target
```

---

# 22. FTP Directory Traversal

Some servers allow accessing parent directories.

Example:

```
cd ../../
```

This can expose system files.

---

# 23. FTP Misconfiguration Exploits

Common issues:

| Vulnerability       | Risk             |
| ------------------- | ---------------- |
| Anonymous login     | data leak        |
| Weak password       | account takeover |
| Upload allowed      | web shell        |
| Directory traversal | system access    |
| Old FTP server      | RCE exploit      |

---

# 24. FTP Logging

Logs stored in:

```
/var/log/vsftpd.log
```

Example log entry:

```
Sat Jun 22 10:12:33 user admin uploaded shell.php
```

Admins monitor these logs.

---

# 25. Real World Use Cases

FTP is used in:

### Web hosting

Uploading website files.

```
index.html
style.css
```

---

### Firmware distribution

Router companies distribute firmware.

Example:

```
ftp.tp-link.com
```

---

### Automated backups

Servers upload backups to storage servers.

---

### Data migration

Moving large datasets.

---

# 26. Best FTP Clients

CLI:

```
ftp
lftp
ncftp
```

GUI:

| Tool      | OS            |
| --------- | ------------- |
| FileZilla | Linux/Windows |
| WinSCP    | Windows       |
| Cyberduck | Mac           |

---

# 27. Advanced FTP Tool (lftp)

Install:

```
sudo apt install lftp
```

Connect:

```
lftp ftp://user:pass@server
```

Mirror directory:

```
mirror /remote /local
```

---

# 28. FTP vs SFTP

| Feature    | FTP | SFTP |
| ---------- | --- | ---- |
| Encryption | ❌   | ✅    |
| Port       | 21  | 22   |
| Security   | Low | High |
| Protocol   | FTP | SSH  |

---

# 29. FTP Packet Structure

Example packet:

```
USER admin
PASS admin123
CWD /uploads
STOR shell.php
```

Captured easily via packet sniffing.

---

# 30. Summary

FTP is:

* Simple
* Old
* widely used
* insecure

Key points:

```
Port: 21
Commands: USER PASS LIST RETR STOR
Modes: Active / Passive
Alternatives: SFTP / FTPS
```

---

# If you want, I can also show you:

* **How hackers exploit FTP servers step-by-step (real attack chain)**
* **How to build your own FTP server in Python**
* **How to capture FTP passwords using Wireshark**
* **How to exploit FTP in HackTheBox machines**

These will help a lot for **ethical hacking + networking mastery**.
