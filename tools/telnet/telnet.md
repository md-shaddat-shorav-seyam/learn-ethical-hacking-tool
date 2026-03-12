## Telnet — Start-to-End Guide (with Real-World Cases)

Telnet is a **network protocol used to remotely connect to another computer or device over TCP/IP** and execute commands through a terminal session.

Today Telnet is **mostly used for testing ports and interacting with simple network services**, because it **sends data in plaintext** (not secure). Modern remote access uses **SSH instead**.

---

# 1. What is Telnet

**Telnet = TELecommunication NETwork**

It allows a user to:

* Connect to a **remote system**
* Execute commands remotely
* Interact with network services

Example:

```bash
telnet 192.168.1.10
```

This opens a **terminal session** to the remote machine.

---

# 2. How Telnet Works

Telnet works on **TCP port 23** by default.

Workflow:

```
Client (Telnet)  →  TCP Connection  →  Server
```

Steps:

1. Client opens TCP connection
2. Telnet session starts
3. User authenticates
4. User executes commands

Example flow:

```
User → Telnet Client → Internet → Telnet Server → OS Shell
```

---

# 3. Installing Telnet

### Linux (Debian/Ubuntu/Kali/Parrot)

```bash
sudo apt update
sudo apt install telnet
```

Check installation:

```bash
telnet
```

---

### Mac

```bash
brew install telnet
```

---

### Windows

Enable feature:

```
Control Panel → Programs → Turn Windows features on/off → Telnet Client
```

---

# 4. Basic Telnet Syntax

```
telnet [host] [port]
```

Example:

```bash
telnet example.com 80
```

Meaning:

* connect to **example.com**
* using **port 80**

---

# 5. Important Telnet Commands

Inside telnet prompt:

```
open
close
quit
status
set
display
```

Example:

```
telnet> open 192.168.1.1
```

---

# 6. Connecting to a Telnet Server

Example:

```bash
telnet 192.168.1.5
```

Output:

```
Trying 192.168.1.5...
Connected to 192.168.1.5.
login:
```

Then login:

```
username: admin
password: admin
```

---

# 7. Telnet for Port Testing

Telnet is widely used to **check if a port is open**.

Example:

```bash
telnet google.com 80
```

If port is open:

```
Connected to google.com
```

If closed:

```
Connection refused
```

---

# 8. Telnet for HTTP Testing

You can manually send HTTP requests.

Example:

```
telnet example.com 80
```

Then type:

```
GET / HTTP/1.1
Host: example.com

```

Press **Enter twice**

Response:

```
HTTP/1.1 200 OK
Content-Type: text/html
```

This is how **web servers respond**.

---

# 9. Telnet for SMTP Testing

Telnet is often used to test **mail servers**.

Connect to SMTP server:

```bash
telnet smtp.gmail.com 25
```

Example conversation:

```
220 smtp.gmail.com ESMTP
HELO test.com
250 Hello

MAIL FROM:test@test.com
250 OK

RCPT TO:user@gmail.com
250 Accepted

DATA
Subject: Test Email

Hello from Telnet
.
250 Message queued
```

---

# 10. Telnet for Banner Grabbing (Ethical Hacking)

Telnet helps identify **service versions**.

Example:

```bash
telnet 192.168.1.10 21
```

Output:

```
220 (vsFTPd 3.0.3)
```

Now attacker knows:

```
FTP version = vsFTPd 3.0.3
```

Used during **reconnaissance**.

---

# 11. Telnet in Penetration Testing

Common usage:

### Check open service

```
telnet target 80
telnet target 22
telnet target 21
```

### Check login access

```
telnet target 23
```

### Banner grabbing

```
telnet target port
```

---

# 12. Telnet with HackTheBox (Real Example)

Example target:

```
10.129.1.20
```

Scan:

```bash
nmap -sV 10.129.1.20
```

Result:

```
23/tcp open telnet Linux telnetd
```

Connect:

```bash
telnet 10.129.1.20
```

Output:

```
login:
```

Try default credentials:

```
admin:admin
root:root
guest:guest
```

If successful:

```
Welcome to Linux server
$
```

Now attacker has shell.

---

# 13. Telnet vs SSH

| Feature    | Telnet  | SSH                  |
| ---------- | ------- | -------------------- |
| Encryption | ❌ No    | ✅ Yes                |
| Security   | Low     | High                 |
| Port       | 23      | 22                   |
| Usage      | Testing | Secure remote access |

Example SSH:

```bash
ssh user@192.168.1.10
```

---

# 14. Real World Case 1 — Router Access

Old routers use telnet.

Example:

```bash
telnet 192.168.0.1
```

Login:

```
admin
admin
```

Admin can configure router.

This is why many **routers disable telnet now**.

---

# 15. Real World Case 2 — IoT Botnet (Mirai)

The **Mirai botnet** infected millions of devices.

Attack process:

1. Scan internet for **port 23**
2. Connect using **telnet**
3. Try default passwords

Example attack list:

```
root:root
admin:admin
guest:guest
```

After login:

```
download malware
join botnet
```

Millions of devices were compromised.

---

# 16. Telnet Security Risks

Problems:

* No encryption
* Password visible
* Easy to sniff traffic
* Easy brute-force

Example attack:

```
Wireshark capture → password visible
```

---

# 17. Modern Alternatives

Instead of Telnet use:

| Tool   | Usage               |
| ------ | ------------------- |
| SSH    | secure remote shell |
| Netcat | network testing     |
| Nmap   | port scanning       |
| Curl   | HTTP testing        |

Example:

```
nc example.com 80
```

---

# 18. Practical Lab (Try This)

Step 1 — Start local server

```
python3 -m http.server 8000
```

Step 2 — connect with telnet

```
telnet localhost 8000
```

Send request:

```
GET / HTTP/1.1
Host: localhost

```

You will see HTTP response.

---

# 19. Advanced Usage — Debugging APIs

Telnet can interact with APIs:

```
telnet api.server.com 80
```

Send:

```
GET /users HTTP/1.1
Host: api.server.com

```

---

# 20. Telnet Cheat Sheet

Connect to host

```
telnet host
```

Connect to port

```
telnet host port
```

Exit telnet

```
quit
```

Open connection

```
open host port
```

---

# 21. Telnet Architecture

```
+----------------+
| Telnet Client  |
+----------------+
        |
        | TCP
        |
+----------------+
| Telnet Server  |
+----------------+
        |
        |
+----------------+
| Remote System  |
+----------------+
```

---

# 22. When Hackers Still Use Telnet

Even today:

* router exploitation
* IoT hacking
* port testing
* banner grabbing
* service interaction

---

✅ **Summary**

Telnet is used for:

* remote terminal access
* network service testing
* banner grabbing
* penetration testing
* debugging servers

But replaced by **SSH for security reasons**.

---

If you want, I can also show you:

* **Telnet hacking techniques used in penetration testing**
* **Telnet in HackTheBox (step-by-step lab)**
* **How attackers exploit Telnet services**
* **How to build your own Telnet server in Python**

These will help a lot for **ethical hacking and HTB labs** you are doing.
