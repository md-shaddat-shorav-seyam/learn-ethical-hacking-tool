## 🔍 What is **ffuf?**

**ffuf (Fuzz Faster U Fool)** is a **fast web fuzzing tool** used mainly in **penetration testing** and **bug bounty hunting**. It helps discover hidden resources like directories, files, parameters, and subdomains on web servers by sending large numbers of requests with different inputs (wordlists).

---

## ⚙️ How ffuf Works (Conceptually)

![Image](https://images.openai.com/static-rsc-4/eYa8HRGJNK0MR_JdqVUY6_rQPpDBA8DHOZ2pU6TyQvFcnqefN_0ZtoNTwxpq8PuSOLEpbtamyefLmX_S-ywhF3WAKj6JOgK7qpKEc2Tk9xDR4qHkMUfOA4-5qOjGE8zJ3XoBOtYYDH_A8xn2Pg_T5NSOg6i-BbzQDe7AWUKmC-89dPATWhJVu165Pkgiu8B4?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/mtZDoeQjbdR7KMEnz2qaFr6MB0jEZ--cdNBDOjX05iifegugEIcvoDJexTvf5kZFfUFMd-92lSdlBaAZ_cFm8RmUaIF2_Cq-0Z1shoUpGu7Txwa233lMVnM96wLRFk5Dlmpqc7vQRmu0ltlc6eVBM4X_WGXgoTTsADrFP-dkgkgbQQq3TI5fckXLtDRJ_2Tj?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/_2KhRehgGv9UpPXgrs4oVjwchp_RVb3CRHGZ9fGpsN84VXrp2tYfGAZ4oqg31fMgADIZ2rsdlV45lp2ZR-LFdfpQGgfo1jqIQP5iTGHXICMqMsBOm-vjjCQ2JUA3jFEeqGpHAlbdoLyEJHstnXBtiOxtJHE9o7Qy4iMNvw1LJsaXMDb7aE5U2t-wFL26AwWf?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/UwDIYLR3uu_O_5gUgBsB4YmebjN8AGqX4vxAJSxkJ1z4hgxv3n1stgU5ruWHMH5vYyhl7J8CEi7dqufJ7oOu5GcgtfHdUw57EMNmnoYk_Lfz-zAzpSou5gRkzJWUsnvi5MllTcOxik-JxLbgWpjF9va3Hs4pSAq-e1ey4oiFfIhrs39pnIk1Qgf__iVrZHm5?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/mHBjEDHZarYWWeJQ3x3eslSsCJH7ODVRVLFbsCS_dlBp89Y3I_Li-K9JkMeIUl1UVejNIQbTZwfGbjm1MGNoVut_ZopQI-dG6J-1yS84gZtQpvK4L-YAWoV43ttvStg2mgrkTqGmMuc_CBvbpGB6UvG7i2K4UQlOD8c9tkrA427vrAjpvUgeLHucH4LjyBMa?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/GhXngZv3cbZbqf3ZmFhs3VJ8gX5tqv5nlmrFBfE_gjUZ2WZmbpmO0SNuyjN9q0YmlaSfg1Vug-SBU98n8RPr-L9qySCdjTHGnFO4qGqF45ZCnP1fgVEBHf6wQVtx1GI8jqQY6tWepzA5T_OtGBaeDA97PLDvlmd1HNkGkCIQn8kB6sF3XxOuyZEAZ4Fgn7uh?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/TWRRZEUmLrPNDAOLNAIJEB_WatX37UnE2CKrRavx4-lk6ehxTgk9Y2nW9jbFWEWkW7canhMrzd4IhNDn0NIz-Bo4SaXOPu0dJpwYu68YEmtsFsj48yfl9znXkSu6cMbbRgEt8IDSD0hGeVIeHuAF1pw1hOtoyFe1Ua0hyh4RzQNgh6OCGqEzIrIjKKqp7nSW?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/rutbBY9p4Lq860qIxqwA4j93B096FFpaUFSI1JHey0HUxpgXGRPz2x8vVmlFUN_3_1C1TB5puWhr9XLF7JPEhDmZuSmAy1qGzzXV9O6R-joUPcPzcUMUwWKrUV1PifEY6WOFk_vM4NX_wgg-th5hlW2dx2h07TtO76Qcyfj0bi0g3ykzMgYX0zAf1-dSlbMy?purpose=fullsize)

* You provide:

  * A **target URL**
  * A **wordlist** (e.g., common directory names like `admin`, `login`, `backup`)
* ffuf:

  * Replaces a keyword (like `FUZZ`) in the URL
  * Sends requests rapidly
  * Filters responses (status codes, size, words, etc.)

---

## 🚀 Key Features

* ⚡ **Extremely fast** (written in Go)
* 🎯 **Flexible fuzzing** (URL paths, headers, POST data, etc.)
* 🔍 **Smart filtering** (hide false positives)
* 📂 Supports recursion (find deeper directories)
* 🧠 Match responses by:

  * Status code (200, 403, etc.)
  * Response size
  * Word count
  * Regex patterns

---

## 🧪 Basic Usage Examples

### 1. Directory Fuzzing

```bash
ffuf -u http://example.com/FUZZ -w wordlist.txt
```

👉 Replaces `FUZZ` with each word in the list.

---

### 2. Filter by Status Code

```bash
ffuf -u http://example.com/FUZZ -w wordlist.txt -mc 200,403
```

👉 Shows only valid or restricted pages.

---

### 3. Fuzz GET Parameters

```bash
ffuf -u "http://example.com/page.php?user=FUZZ" -w users.txt
```

---

### 4. Fuzz POST Data

```bash
ffuf -u http://example.com/login -X POST -d "username=FUZZ&password=test" -w users.txt
```

---

### 5. Add Headers (e.g., Authorization)

```bash
ffuf -u http://example.com/FUZZ -H "Authorization: Bearer token" -w wordlist.txt
```

---

## 📊 Important Options

| Option       | Meaning                   |
| ------------ | ------------------------- |
| `-u`         | Target URL                |
| `-w`         | Wordlist                  |
| `-mc`        | Match HTTP status codes   |
| `-fc`        | Filter status codes       |
| `-fs`        | Filter by response size   |
| `-fw`        | Filter by word count      |
| `-t`         | Number of threads         |
| `-recursion` | Enable recursive scanning |

---

## 🔐 Common Use Cases

* 🔎 **Directory & file discovery** (`/admin`, `/backup.zip`)
* 🔑 **Login brute force testing**
* 🧪 **Parameter fuzzing** (find hidden GET/POST params)
* 🌐 **Subdomain fuzzing**
* 🧬 **API endpoint discovery**

---

## ⚠️ Important Note (Ethical Use)

* Only use ffuf on:

  * Systems you **own**, or
  * Targets where you have **explicit permission**
* Unauthorized fuzzing can be illegal.

---

## 🆚 ffuf vs Other Tools

| Tool       | Speed       | Language | Best For                    |
| ---------- | ----------- | -------- | --------------------------- |
| ffuf       | ⚡ Very Fast | Go       | Modern fuzzing              |
| Burp Suite | Medium      | Java     | Manual + automated testing  |
| Dirbuster  | Slow        | Java     | Basic directory brute-force |
| Gobuster   | Fast        | Go       | Simple brute forcing        |

---

## 💡 Pro Tips

* Use good wordlists (e.g., SecLists)
* Combine filters to remove noise:

  ```bash
  -fc 404 -fs 0
  ```
* Use recursion carefully (can generate huge traffic)
* Save output:

  ```bash
  -o result.json -of json
  ```

---

If you want, I can show:

* 🔥 Real bug bounty workflows using ffuf
* 🧰 Best wordlists for hacking
* 🧪 Practice labs to learn ffuf step-by-step





Here’s a **complete A → Z practical use-case workflow of ffuf** — exactly how ethical hackers and bug bounty hunters use it in real scenarios.

---

# 🔥 Full A → Z ffuf Workflow (Realistic Scenario)

## 🎯 Target Situation

You are testing:

```
http://target.com
```

Goal: **Find hidden attack surfaces (dirs, files, params, APIs, creds)**

---

# 🅰️ A — Asset Discovery (Start Point)

👉 First, identify where to fuzz:

* Main domain
* Subdomains (if available)
* APIs

Example:

```bash
ffuf -u http://target.com/FUZZ -w common.txt
```

✔ Finds:

* `/admin`
* `/login`
* `/dashboard`

---

# 🅱️ B — Brute Force Directories

```bash
ffuf -u http://target.com/FUZZ -w dirlist.txt -mc 200,403
```

🎯 You discover:

```
/admin (403)
/backup (200)
```

👉 **Huge finding:** `/backup`

---

# 🅲 C — Check Hidden Files

```bash
ffuf -u http://target.com/FUZZ -w files.txt
```

Try:

* `.git`
* `.env`
* `config.php`
* `backup.zip`

💥 Example result:

```
.env → 200
```

---

# 🅳 D — Dump Sensitive Files

👉 Open `.env` → contains:

```
DB_PASSWORD=secret123
API_KEY=abcd1234
```

🔥 Sensitive data exposure!

---

# 🅴 E — Endpoint Discovery (API Fuzzing)

```bash
ffuf -u http://target.com/api/FUZZ -w api.txt
```

Find:

```
/api/users
/api/admin
/api/debug
```

---

# 🅵 F — Fuzz Parameters (Hidden Inputs)

```bash
ffuf -u "http://target.com/page.php?FUZZ=test" -w params.txt
```

Find:

```
?debug=true
?admin=1
```

---

# 🅶 G — GET Parameter Testing

```bash
ffuf -u "http://target.com/page.php?id=FUZZ" -w numbers.txt
```

Find:

* IDOR (Insecure Direct Object Reference)
* Sensitive records

---

# 🅷 H — Header Fuzzing

```bash
ffuf -u http://target.com -H "X-Forwarded-For: FUZZ" -w ip.txt
```

🎯 Try:

* `127.0.0.1`
* `localhost`

💥 Might bypass admin restrictions

---

# 🅸 I — Identify Login Points

From earlier:

```
/login
/admin
```

---

# 🅹 J — Username Enumeration

```bash
ffuf -u http://target.com/login -X POST -d "user=FUZZ&pass=test" -w users.txt
```

✔ Detect valid usernames via response differences

---

# 🅺 K — Keyword Injection Testing

Test payloads:

```bash
ffuf -u "http://target.com/search?q=FUZZ" -w payloads.txt
```

Payloads:

* `' OR 1=1--`
* `<script>alert(1)</script>`

🎯 Find:

* SQL Injection
* XSS

---

# 🅻 L — Login Brute Force (If Allowed)

```bash
ffuf -u http://target.com/login -X POST -d "user=admin&pass=FUZZ" -w passwords.txt
```

⚠️ Only in **legal testing scope**

---

# 🅼 M — Match & Filter Noise

Use filters:

```bash
ffuf -fc 404 -fs 0
```

✔ Removes junk responses

---

# 🅽 N — Nested Directory Recursion

```bash
ffuf -u http://target.com/FUZZ -w dirs.txt -recursion
```

Find:

```
/admin/panel
/admin/logs
```

---

# 🅾️ O — Output Results

```bash
ffuf -o results.json -of json
```

✔ Save for reporting

---

# 🅿️ P — Parameter Bruteforce (Advanced)

```bash
ffuf -u http://target.com/page.php -X POST -d "FUZZ=test" -w params.txt
```

---

# 🅀 Q — Query Fuzzing Combinations

```bash
ffuf -u "http://target.com/page.php?user=FUZZ&role=FUZZ" -w users.txt:FUZZ1 -w roles.txt:FUZZ2
```

---

# 🅁 R — Rate Control

```bash
ffuf -t 50 -rate 100
```

✔ Avoid detection / blocking

---

# 🅂 S — Subdomain Fuzzing

```bash
ffuf -u http://FUZZ.target.com -w subdomains.txt
```

Find:

```
dev.target.com
admin.target.com
```

---

# 🅃 T — Test for Misconfigurations

Check:

* `/server-status`
* `/phpinfo.php`

---

# 🅄 U — User Enumeration via APIs

```bash
ffuf -u http://target.com/api/user/FUZZ -w ids.txt
```

---

# 🅅 V — Virtual Host Discovery

```bash
ffuf -u http://target.com -H "Host: FUZZ.target.com" -w vhosts.txt
```

---

# 🅆 W — Wordlist Optimization

Use:

* Small → fast scan
* Large → deep scan

---

# 🅇 X — eXploit Findings

After discovery:

* SQLi → dump DB
* IDOR → access data
* Exposed files → credentials

---

# 🅈 Y — Yield Report

Document:

* Vulnerability
* Proof
* Impact

---

# 🅉 Z — Zero-Day Hunting Mindset

👉 Combine everything:

* dirs + params + headers + APIs

That’s where real bugs live.

---

# 🧠 Real Flow Summary

![Image](https://images.openai.com/static-rsc-4/_FeofJfU_K418_Jay3HiLE0EexJjMXneh2SuOY3k_rC97RPiRqEI_Sy9G3LCiKL5iEWWHFoxmQr0RlKrv7fdxkhgW3plTE2cj6Pbfxw9mgDmbq5al_pvurNMZ_oizdETSJhfTFhxZeyh-9kbhLnVS_xVZcxZYCtaJQbvpe2VSGh2wG8ey4L3K_OV311HAiuE?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/RTNuazuCMXuojT78WqgeiFROeSPxclZ6lA8K3t9PNAhcVu9nrJ53oUxP1gy9VvbvoOfot3IZ6KLVLtkfxtrZl11c3mfncp68PJByiDCUvLzqFezhXIZzwFNF_1tSC-vuuPM2D_uLLu-LD9ZhOhq5RlwlqGkzEwMYq4NuVHAvf3hc2KLE5MohPUYsSZ51xWxV?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/jN6e1vC8vZKF4zfLemSLRnfVag7r1YO8tesNidSGtHon4bdFZ6ECXrsIv9lHCfklhR0fQ5N-ahgPJ3WYxGIg40IPmRkQBxsqgr1HH4P6hrOza6lrFndivK2QWhvuto1k3NGSBAGzJuNLm3fYAlzKBP_H1yjB7jaG0kqZlP-lWv5FnuRbphAn4nmpJKJ9pP44?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/E9gxSYeqf4f5Lw73_mT2jCRo6Onv13xD5_FPVaoh5uQ5sbXTD1JWuoSoz7jP9KLcYLBYK8v2sQsfr5C8ZqDmSO9ICkgLcQShumFWglhNGWW4eMgFj2lIscdAJfRL4AQTBi6gx-0invjUcuhD7Tt8yEtir7waTPfqtyXJSn4R1xUR4MJSqJMsJyPRoze2Icu8?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/vJao4G40sMDKGwr9QX_lRTPNs6hyxkpgBSqyF6KPgnVcSFlvCVDQdFFsp8uqYmmxRex04-rsRpwhDZWGKeLON1IqCkleRVMu9cvX-mFfwbYVzwUI4m6Ujhlu2V-HTT8X_trQ0uYAPTaf_BsXeMDlm_H0V-khziBjxRiZkU2Q7tcOUpXRkOwLsa8CUSznbDzj?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/_m0qO8XsMdQ_MJOCQl9gaLdkyHon0pZ0rBh6ij8HQTgUNVFm18zu9UpTWVFEYahkSgHbaKbE6JHSNi9abhl1XEecxz6mrsncMhfIzdQ9yN03ac81oGzbP8M5ht0LA8i4JSG4OvxGn_giVspk52tJOF7KreFhNOF77A_ozbCWSNOm8en2tlA6-SF_QS_OAoaT?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/_jzby6szoYF_31QboCDjutqOlwkinPf8_EnlPnGHSy4tI5R6MvZO0vKf4CHbmqg_mN2UyjMluqkwUGpeV9LGR8HxtFNugqCNAzgI9UgoTnB9W6jTSqe2agwK8m5wcSWkwJ0albz8faEVs2rU0KXKU5qHKYcpyvaFHM9Q522IPLHh9c_4PWI0OH137L8wafKC?purpose=fullsize)

---

## ⚡ Final Insight

👉 ffuf is not just a tool — it's a **discovery engine**.

Most beginners:

* Only use directory fuzzing ❌

Advanced users:

* Combine fuzzing everywhere (URL + headers + params + API) ✅

---

## 🚀 If You Want Next

I can guide you through:

* 🧪 Live lab practice (step-by-step)
* 🧰 Best wordlists (SecLists setup)
* 🎯 Real bug bounty tricks using ffuf
* ⚡ Automation scripts (pro level)

Just tell me 👍
