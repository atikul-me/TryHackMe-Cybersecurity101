# Command Line & System Fundamentals

This document contains key notes covering Windows Alternate Data Streams (ADS), Bash Scripting, User Management, Network Scanning with Nmap, and Windows Utility Commands (CMD & PowerShell).

---

## 1. Alternate Data Streams (ADS) - Windows

Alternate Data Streams (ADS) is a feature of the NTFS file system that allows hiding data within files without altering their content or size in normal File Explorer views.

- **Purpose:** Used to hide text, images, malicious code, or confidential information inside a file.
- **Uniqueness:** Normal Windows File Explorer cannot detect or show these hidden file entities.

### Commands for ADS:

- **Build/Create ADS:**
  ```powershell
  Set-Content -Path "C:\Path\to\file.txt" -Stream "hidden" -Value "This is hidden"

  Get-Content -Path "C:\Path\to\file.txt" -Stream "hidden"

  Get-Item -Path "C:\Path\to\file.txt" -Stream *

2. Bash Scripting - Linux
Bash scripting allows automation of tasks using shell scripting syntax.
Shebang (#!/bin/bash): Specifies which interpreter will execute the script.

- **Sample Script (f_script.sh):**
```Bash
#!/bin/bash
# Defines interpreter

echo "Name?"
read name
echo "Welcome, $name"
```
Execution Permissions & Running Script:
- **Grant Execution Permission:**

```Bash
chmod +x f_script.sh
```
- **Run Script:**
```Bash
./f_script.sh
```
3. Local User & Group Management (Windows)
GUI Tool Access:
DOS
lusrmgr.msc
(Run command to open Local Users and Groups console)

4. Nmap Stealth Scan
- **Stealth Scan Command:**

```Bash
nmap -sS 192.168.1.1
```
Mechanism: Does not complete the full TCP 3-way handshake. Sends only SYN packets to detect open ports stealthily.

6. Network & System Commands (CMD & PowerShell)
On Command Prompt (CMD):
Network Connections & Listening Ports:
DOS
netstat -abon
(Displays all active network connections, listening ports, and associated executable processes)

On PowerShell:
- **Search Specific File Extension:**

```PowerShell
Get-ChildItem | Where-Object "Extension" -eq ".txt"
```
- **Search File by Name Pattern:**

```PowerShell
Get-ChildItem | Where-Object -Property "Name" -like "Ship"
```
- **Calculate File Hash:**

```PowerShell
Get-FileHash -Path .\flag.txt
```
