# John the Ripper: The Basics — TryHackMe Guide

A comprehensive room reference and practical guide for **John the Ripper (JtR)**, one of the most popular and versatile offline password cracking tools used in penetration testing and cybersecurity assessments.

---

## 1\. Overview & Installation

**John the Ripper (JtR)** is an open-source, offline password cracker designed to detect weak passwords. It automatically detects hash types, supports custom wordlists, and offers specific modes for cracking formats ranging from Linux system hashes (`/etc/shadow`) to password-protected archives (ZIP, RAR, PDF).

### Basic Syntax

```sh
john [options] [path_to_hash_file]
```

### Key Variations

* **JtR Core (Standard):** Basic installation shipped with most Linux distros.  
* **JtR Jumbo Version:** Enhanced community edition supporting significantly more hash formats, algorithms, and custom scripts (`zip2john`, `ssh2john`, etc.).

---

## 2\. Fundamental Commands & Hash Cracking

### Wordlist Attack (Dictionary Attack)

Supplying a specific custom wordlist (such as `rockyou.txt`):

```sh
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
```

### Auto-Detecting Hashes

John automatically inspects input files to determine the target hash algorithm. To view cracked passwords:

```sh
john --show hashes.txt
```

### Specifying Hash Formats

If auto-detection fails or multiple hash algorithms match, explicitly define the format using `--format`:

```sh
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
```

#### Common Format Tags:

* `raw-md5` — Standard MD5  
* `raw-sha1` — Standard SHA-1  
* `raw-sha256` — Standard SHA-256  
* `NT` — Windows NTLM Hashes  
* `bcrypt` — Blowfish-based Hashes

To view all supported formats in your JtR installation:

```sh
john --list=formats
```

---

## 3\. Cracking Specific Target Systems

### A. Cracking Linux Passwords (`/etc/passwd` & `/etc/shadow`)

Linux system hashes are split across two files: `/etc/passwd` (user info) and `/etc/shadow` (salted hash values).

1. **Combine files using `unshadow`:**

```sh
unshadow /etc/passwd /etc/shadow > unshadowed.txt
```

2. **Crack the unshadowed file:**

```sh
john --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt
```

### B. Cracking Windows Passwords (SAM / NTLM)

Windows uses **NTLM** (NT LAN Manager) hashes to store passwords in the SAM database.

```sh
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt ntlm_hashes.txt
```

---

## 4\. Hash Extraction Utilities (`*2john`)

JtR Jumbo includes specialized helper scripts to extract hashes from encrypted files into a format readable by John.

| Target File Type | Extraction Command Example | Cracking Command Example |
| :---- | :---- | :---- |
| **ZIP File** | `zip2john protected.zip > zip.hash` | `john --wordlist=rockyou.txt zip.hash` |
| **RAR File** | `rar2john archive.rar > rar.hash` | `john --wordlist=rockyou.txt rar.hash` |
| **PDF File** | `pdf2john document.pdf > pdf.hash` | `john --wordlist=rockyou.txt pdf.hash` |
| **SSH Private Key** | `ssh2john id_rsa > ssh.hash` | `john --wordlist=rockyou.txt ssh.hash` |

---

## 5\. Single User Mode & Custom Rules

### Single User Mode

Uses information inside the hash file (usernames, real names, home directories) and applies internal mutation rules to crack simple passwords rapidly without a large wordlist.

```sh
john --single hashes.txt
```

### Applying Mangling Rules

Wordlist attacks can be augmented using JtR’s rules engine to mutate words (e.g., changing `password` to `P@ssword123`):

```sh
john --wordlist=/usr/share/wordlists/rockyou.txt --rules hashes.txt
```

---

## 6\. Best Practices & Quick Cheatsheet

1. **Check Session Progress:** Press `Spacebar`, `Enter`, or any key while John is running to display real-time status (ETA, current speed in c/s, hashes cracked).  
2. **Restore Paused Sessions:**

```sh
john --restore
```

3. **Session Management:** Name specific sessions to run multiple concurrent instances:

```sh
john --session=job1 --wordlist=rockyou.txt hashes.txt
```

---

*Reference document structured for security training and practical lab assessments.*