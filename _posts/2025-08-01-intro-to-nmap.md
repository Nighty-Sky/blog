---
title: "Intro to Nmap"
date: 2025-08-01
categories: pentesting
tags: [nmap, scanning]
---

Nmap is a powerful network scanning tool used for reconnaissance...

Here are **structured notes for Notion** based on the Medium blog post: **"Nmap Post Port Scans | TryHackMe (THM)" by Aircon**, formatted clearly for cybersecurity study/reference purposes.

---

# 🛰️ Nmap Post Port Scans — TryHackMe (THM)

🔗 **Lab:** [https://tryhackme.com/room/nmap04](https://tryhackme.com/room/nmap04)
🧠 **Author:** Aircon | 🕐 10 min read
🗓️ **Published:** June 1, 2022

---

## 📌 Topics Covered:

* Service & Version Detection (`-sV`)
* OS Detection (`-O`)
* Traceroute (`--traceroute`)
* Nmap Scripting Engine (NSE)
* Output Saving Formats (`-oN`, `-oG`, `-oX`, `-oA`)

---

## 🔎 1. Service & Version Detection (`-sV`)

* **Purpose:** Detects services & versions on open ports (e.g. Apache 2.4.18).
* **Command:**

  ```bash
  nmap -sV <target>
  ```
* **Intensity Levels:**

  * Range: 0 (light) → 9 (aggressive)
  * `--version-light`: Level 2
  * `--version-all`: Level 9

🛑 **Note:**

* `-sV` requires full **TCP 3-way handshake**, making it incompatible with `-sS` (stealth scan).
* Must use `sudo` for full functionality.

---

## 📋 \[Task 2]

* **Port 143 Detected Version:** `Dovecot imapd`
* **Service Without Version in `--version-light`:** `rpcbind`

---

## 🖥️ 2. OS Detection (`-O`)

* **Command:**

  ```bash
  nmap -sS -O <target>
  ```
* **Behavior:** Uses port behavior & TTL to guess the OS.
* Needs **1 open + 1 closed port** to make a reliable OS guess.
* ⚠️ **Not always accurate** due to virtualization, firewalls, etc.

🧾 **Answer (3.1):**
OS Detected = `Linux`

---

## 🌐 3. Traceroute in Nmap (`--traceroute`)

* **Command:**

  ```bash
  nmap -sS --traceroute <target>
  ```
* **Mechanism:** Works in reverse of regular traceroute.

  * Starts with **high TTL**, decreases it step-by-step.
* Some routers may block TTL expiry ICMP replies → incomplete paths.

---

## 🧰 4. NSE (Nmap Scripting Engine)

* **Location:**
  `/usr/share/nmap/scripts/`
* **Run default scripts:**

  ```bash
  sudo nmap -sS -sC <target>
  ```
* **Script Categories:**

  * `auth`, `brute`, `default`, `exploit`, `safe`, `vuln`, etc.
* **Run specific script:**

  ```bash
  sudo nmap --script <script-name> <target>
  ```

  Example:

  ```bash
  sudo nmap --script http-date <target>
  ```

### ✅ NSE Script Examples:

| Script                   | Purpose                            |
| ------------------------ | ---------------------------------- |
| `http-date`              | Shows HTTP server time and offset  |
| `http-robots.txt`        | Lists disallowed entries           |
| `http-vuln-cve2015-1635` | Detects MS15–034 RCE vulnerability |
| `ssh2-enum-algos`        | Enumerates SSH2 algorithms         |

---

## 🧪 \[Task 4 Answers]

* **4.1:** `disallowed entries`
* **4.2:** `http-vuln-cve2015–1635`
* **4.3 (Port 53 version):** `9.9.5–9+deb8u19-Debian`
* **4.4 (SSH kex\_algorithms with sha1):** `diffie-hellman-group14-sha1`

---

## 📁 5. Saving Scan Results

### 🔸 Normal Output

```bash
nmap -oN <file>
```

### 🔸 Grepable Output (for scripting/grep)

```bash
nmap -oG <file>
```

### 🔸 XML Output (for parsing)

```bash
nmap -oX <file>
```

### 🔸 Save All Formats

```bash
nmap -oA <base_filename>
```

### 🔸 Script Kiddie Output (not recommended)

```bash
nmap -oS <file>
```

---

## 🧾 \[Task 5 Answers]

* **5.1 (Systems on HTTPS port):** `3`
* **5.2 (Port 8089 IP):** `172.17.20.147`

---

## ✅ Conclusion

* Post port-scan features in Nmap are **essential for enumeration**.
* `-sV` gives version data critical for **vulnerability discovery**.
* NSE helps automate **vulnerability checks** and **info gathering**.
* Always **save scans** for documentation, reuse, or compliance.

---

> 💡 *Always confirm authorization before running intrusive scans/scripts.*

Let me know if you'd like this exported to **Notion Markdown**, **PDF**, or **docx** format.

