Here’s a **start-to-end guide to `whatweb`** with **real-world use cases**, practical commands, workflow, output explanation, and how it fits into recon.

---

# What is `whatweb`?

`whatweb` is a **web technology fingerprinting tool**.

It helps you identify:

* web server
* CMS
* frameworks
* analytics tools
* JavaScript libraries
* programming language hints
* security headers
* login panels
* admin technologies
* hosting/CDN clues

Example:
You scan a website and `whatweb` may tell you:

* Apache
* WordPress
* jQuery
* PHP
* Cloudflare
* Google Analytics

So instead of attacking blindly, you know **what technologies are running**.

---

# Why `whatweb` is useful

In real-world recon, before testing a website, you want answers like:

* Is this WordPress?
* Is it running Apache or Nginx?
* Is there a WAF like Cloudflare?
* Is there a framework like Laravel, Django, Rails?
* Is the app using outdated libraries?
* Is there a known CMS login page?

That is where `whatweb` helps.

It is often used in:

* bug bounty recon
* penetration testing
* CTF/HTB web enumeration
* red team external recon
* learning web stack analysis

---

# Installation

## On Kali / Parrot / Debian-based

```bash
sudo apt update
sudo apt install whatweb
```

## Check installation

```bash
whatweb --version
```

## Help menu

```bash
whatweb --help
```

---

# Basic syntax

```bash
whatweb [options] <target>
```

Example:

```bash
whatweb http://example.com
```

---

# First simple example

```bash
whatweb http://example.com
```

Possible output:

```bash
http://example.com [200 OK] Country[UNITED STATES][US], HTML5, HTTPServer[nginx], IP[93.184.216.34], Title[Example Domain]
```

This tells you:

* site is reachable
* returned HTTP 200
* server may be nginx
* page title is “Example Domain”
* IP resolved

---

# Understanding output pieces

Example:

```bash
http://target.com [200 OK] Apache[2.4.52], Bootstrap, Country[US], HTML5, JQuery[3.6.0], PHP[8.1.2], WordPress[6.2], Title[Home]
```

Meaning:

* `[200 OK]` → page responded successfully
* `Apache[2.4.52]` → Apache web server
* `Bootstrap` → frontend CSS framework
* `JQuery[3.6.0]` → JavaScript library version
* `PHP[8.1.2]` → backend language clue
* `WordPress[6.2]` → CMS detected
* `Title[Home]` → HTML title

This is valuable because now you know:

* it is probably a WordPress site
* backend may be PHP
* maybe check WordPress-specific attack surface
* maybe check plugin/theme exposure

---

# Most important commands

## 1. Scan one target

```bash
whatweb http://target.com
```

## 2. Scan with more detail

```bash
whatweb -v http://target.com
```

`-v` = verbose

This shows more plugin details and matches.

---

## 3. Aggression levels

`whatweb` supports aggression to make detection stronger.

```bash
whatweb -a 1 http://target.com
```

Levels are commonly:

* `1` = stealthier / basic
* `3` = balanced
* `4` = more aggressive

Example:

```bash
whatweb -a 3 http://target.com
```

More aggressive scans may send extra requests to identify technologies better.

---

## 4. Scan multiple targets

```bash
whatweb http://site1.com http://site2.com
```

---

## 5. Scan targets from a file

Create a file:

```bash
cat targets.txt
```

Example content:

```txt
http://site1.com
http://site2.com
http://10.10.10.10
```

Then run:

```bash
whatweb -i targets.txt
```

Very useful for recon on many domains or subdomains.

---

## 6. Follow redirects

```bash
whatweb --follow-redirect=always http://target.com
```

Why useful:
Many websites redirect:

* http → https
* bare domain → www
* login → portal

Without following redirects, you may miss the real tech stack.

---

## 7. Use a proxy

```bash
whatweb --proxy http://127.0.0.1:8080 http://target.com
```

Useful with:

* Burp Suite
* OWASP ZAP

This lets you inspect every request `whatweb` sends.

---

## 8. Output to file

```bash
whatweb http://target.com > result.txt
```

Or use built-in logging options depending on version/help output.

Example safer workflow:

```bash
whatweb -v -a 3 http://target.com | tee result.txt
```

---

## 9. No errors in output

```bash
whatweb --no-errors http://target.com
```

Useful when scanning many hosts and wanting cleaner output.

---

## 10. Colorless output

```bash
whatweb --color=never http://target.com
```

Useful for scripts and logs.

---

# Useful workflow from start to end

A practical recon flow:

## Step 1: Check the target is alive

```bash
curl -I http://target.com
```

or directly:

```bash
whatweb http://target.com
```

## Step 2: Run a basic fingerprint

```bash
whatweb http://target.com
```

## Step 3: Run with better detection

```bash
whatweb -a 3 -v http://target.com
```

## Step 4: Follow redirects

```bash
whatweb -a 3 -v --follow-redirect=always http://target.com
```

## Step 5: Send through Burp

```bash
whatweb --proxy http://127.0.0.1:8080 -a 3 http://target.com
```

## Step 6: Use results to guide next actions

For example:

* If WordPress found → use WordPress-specific enumeration
* If Joomla found → Joomla checks
* If Apache and PHP found → look for PHP app routes, uploads, misconfigs
* If Cloudflare found → note WAF/CDN presence
* If login page found → inspect auth flows
* If outdated JS library found → research client-side vulnerabilities

---

# Real-world case 1: Bug bounty reconnaissance

Suppose you found:

```bash
whatweb https://shop.target.com
```

Output:

```bash
https://shop.target.com [200 OK] Cloudflare, HTTPServer[nginx], JQuery[1.12.4], Magento, PHP[7.4], Title[Online Store]
```

## What this tells you

* Site is likely behind Cloudflare
* Uses Magento
* Uses old jQuery
* PHP backend
* Nginx server

## What you do next

* Look for Magento-specific paths
* Check exposed admin paths carefully and legally
* Inspect JavaScript for outdated libraries
* Note Cloudflare may hide origin IP and filter some requests
* Test only within program scope and rules

This is how `whatweb` helps you move from **unknown website** to **structured testing plan**.

---

# Real-world case 2: Internal pentest

You are given an internal IP:

```bash
whatweb http://10.10.20.15
```

Output:

```bash
http://10.10.20.15 [200 OK] Apache[2.4.49], PHP[7.2.24], Title[Employee Portal], JQuery[2.2.4]
```

## Why this matters

Now you know:

* Apache version may matter
* PHP version may be old
* There is a web portal
* Old frontend dependencies exist

## Next steps

* Manual browsing
* Directory enumeration
* Security header review
* Auth testing
* Check whether the Apache/PHP versions are outdated
* Search for known issues only within authorized scope

---

# Real-world case 3: HTB / CTF enumeration

You get a web target:

```bash
whatweb http://10.129.XX.XX
```

Output:

```bash
http://10.129.XX.XX [200 OK] Bootstrap, HTML5, PHP[8.0], Title[Hospital Management System]
```

This hints:

* likely PHP app
* maybe custom CMS or panel
* not a static site
* maybe inspect source code, forms, upload, login, hidden endpoints

Then you continue with:

* browser inspection
* `curl`
* directory brute force
* parameter discovery
* login testing
* version clue analysis

---

# Aggression levels explained simply

## `-a 1`

Low-noise/basic detection.

```bash
whatweb -a 1 http://target.com
```

Use when:

* you want quick identification
* you want fewer requests
* you want to stay less noisy

## `-a 3`

Balanced and common choice.

```bash
whatweb -a 3 http://target.com
```

Use when:

* basic scan missed things
* you want more reliable fingerprints

## `-a 4`

More aggressive.

```bash
whatweb -a 4 http://target.com
```

Use when:

* in lab/HTB/internal authorized environment
* you want maximum detection

---

# Important options you should know

## Verbose

```bash
-v
```

## Aggression

```bash
-a <level>
```

## Input file

```bash
-i targets.txt
```

## Proxy

```bash
--proxy http://127.0.0.1:8080
```

## Follow redirects

```bash
--follow-redirect=always
```

## Disable color

```bash
--color=never
```

## Suppress errors

```bash
--no-errors
```

Always confirm exact options with:

```bash
whatweb --help
```

because package versions can differ.

---

# Practical examples

## Example 1: Basic website tech fingerprint

```bash
whatweb https://example.org
```

## Example 2: Better fingerprint with redirects

```bash
whatweb -a 3 -v --follow-redirect=always https://example.org
```

## Example 3: Internal IP scan

```bash
whatweb -a 3 http://192.168.1.50
```

## Example 4: Scan many subdomains

```bash
whatweb -i subdomains.txt
```

## Example 5: Through Burp Suite

```bash
whatweb --proxy http://127.0.0.1:8080 -a 3 -v https://target.com
```

---

# How `whatweb` actually detects technologies

It fingerprints using clues such as:

* HTTP headers
* cookies
* HTML source
* meta generator tags
* JavaScript files
* URL patterns
* response behavior
* favicon or known signatures
* page titles and framework traces

For example:

* `X-Powered-By: PHP/8.1.2`
* `/wp-content/` → WordPress clue
* `generator` meta tag
* `Set-Cookie` names
* script names like `jquery.js`

---

# When `whatweb` may be wrong or incomplete

`whatweb` is helpful, but not perfect.

It can:

* miss hidden tech
* show false positives
* fail behind WAF/CDN
* miss SPA frameworks
* detect only surface-level clues
* fail if headers are stripped

So never trust it blindly.

Always verify with:

* browser dev tools
* `curl -I`
* page source
* directory enumeration
* manual inspection
* Burp Suite

---

# Good companion tools

After `whatweb`, people often use:

## `curl`

Check headers:

```bash
curl -I http://target.com
```

## `nmap`

Check open ports and service versions:

```bash
nmap -sC -sV target.com
```

## `ffuf` or `gobuster`

Find hidden directories:

```bash
ffuf -u http://target.com/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

## browser / Burp

Manual testing and deeper inspection

## `wpscan`

If WordPress detected

---

# Example full mini workflow

Imagine target: `http://demo.local`

## 1. Fingerprint tech

```bash
whatweb -a 3 -v http://demo.local
```

Output:

```bash
http://demo.local [200 OK] Apache[2.4.54], PHP[8.1], WordPress[6.3], JQuery[3.6.0], Title[Demo Site]
```

## 2. Verify manually

```bash
curl -I http://demo.local
```

## 3. Inspect in browser

* source code
* scripts
* forms
* comments
* login/admin links

## 4. Enumerate directories

```bash
ffuf -u http://demo.local/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

## 5. If WordPress confirmed

Use WordPress-specific enumeration tools and manual checks.

That is a real beginner-to-practical workflow.

---

# Common mistakes beginners make

## 1. Using only one scan

Run at least:

* basic
* verbose
* with redirects

## 2. Ignoring redirects

A site may look empty until redirected.

## 3. Trusting fingerprint blindly

Always verify.

## 4. Scanning aggressively on unauthorized targets

Only test systems you own or are authorized to assess.

## 5. Ignoring proxy usage

Using Burp with `whatweb` teaches a lot.

---

# Safe and legal note

Use `whatweb` only on:

* your own systems
* lab machines
* HTB/CTF targets
* systems where you have permission

Do not scan random websites without authorization.

---

# Beginner cheat sheet

## Basic

```bash
whatweb http://target.com
```

## Verbose

```bash
whatweb -v http://target.com
```

## Aggressive

```bash
whatweb -a 3 http://target.com
```

## With redirects

```bash
whatweb --follow-redirect=always http://target.com
```

## Through proxy

```bash
whatweb --proxy http://127.0.0.1:8080 http://target.com
```

## Multiple targets

```bash
whatweb -i targets.txt
```

---

# Best learning path for you

Since you like practical learning, do this:

## Lab exercise 1

Run `whatweb` on:

* a simple static website
* a WordPress demo site
* a local test web app

Compare outputs.

## Lab exercise 2

Run with Burp proxy:

```bash
whatweb --proxy http://127.0.0.1:8080 -a 3 http://target.com
```

Watch the requests and understand how detection works.

## Lab exercise 3

Take one result like:

* Apache
* PHP
* WordPress
  and manually verify each clue from headers or source.

That builds real skill.

---

# Final summary

`whatweb` is used to:

* fingerprint website technologies
* guide recon
* identify CMS/framework/server clues
* build a smarter enumeration plan

A strong basic command is:

```bash
whatweb -a 3 -v --follow-redirect=always http://target.com
```

That gives a practical balance of detail and usefulness.

If you want, next I can give you a **WhatWeb full lab guide with HTB-style examples**, or a **WhatWeb + Nmap + ffuf web recon workflow from start to end**.
