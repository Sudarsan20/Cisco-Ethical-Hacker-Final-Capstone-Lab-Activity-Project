# Cisco Ethical Hacker - Final Skills Assessment Capstone Write-Up

🛡️ **Verified Certification:** [![Cisco Ethical Hacker Badge](https://img.shields.io/badge/Credly-Verified_Badge-007A87?style=flat&logo=credly&logoColor=white)](https://www.credly.com/badges/1dc6334e-82fb-44e0-aa82-c08afed5a2e6)

## 🎖️ Certification Project Overview
This repository contains the comprehensive walkthrough, core methodologies, and exact terminal commands executed during the **Cisco Ethical Hacker Skills Assessment Capstone**. Having completed the practical hands-on evaluation and earned my certification, this documentation serves as a portfolio write-up demonstrating entry-level proficiency across web application exploitation, network scanning, credential auditing, and packet analysis.

---

## 🛠️ Challenge 1: SQL Injection & Password Cracking
**Objective:** Leverage a SQL Injection (SQLi) flaw inside a target web platform to exfiltrate password hashes, perform an offline brute-force attack against a cryptographic string, and pivot into a separate target machine via SSH.

### Step-by-Step Execution
1. **Target Web Authentication & Scope Realignment:**
   * Navigated to the staging web app environment at `http://10.6.6.100`.
   * Logged in using initial default administrative privileges: 
     * **Username:** `admin`
     * **Password:** `password`
   * Navigated to the security options tab and set the internal **DVWA Security** profiling tier to **Low**.

2. **Database Infiltration via SQL Injection Payload:**
   * Switched over to the designated **SQL Injection** sub-pane.
   * Injected the following database union logic string directly into the query submission box to pull hidden authentication elements:
     ```sql
     1' OR 1=1 UNION SELECT user, password FROM users #
     ```
   * Captured the raw MD5 password hash linked to system user **Gordon Brown (`gordonb`)**.

3. **Offline Cryptographic Hash Cracking:**
   * Saved the extracted MD5 hash string (`e99a18c428cb38d5f260853678922e03`) onto the local Kali machine filesystem:
     ```bash
     echo e99a18c428cb38d5f260853678922e03 > passwd.txt
     ```
   * Used John the Ripper to break the MD5 hash using standard wordlist rules:
     ```bash
     john --format=raw-md5 passwd.txt
     ```

4. **Network Pivot and Flag Exfiltration:**
   * Utilized the cracked plaintext credentials to establish an encrypted shell instance onto the isolated local target host at `172.17.0.2`:
     ```bash
     ssh gordonb@172.17.0.2
     ```
   * Explored the landing path directory, located the challenge document, and read the token payload:
     ```bash
     ls
     cat hkxisx.txt
     ```
   * **Retrieved Challenge Code:** `4E9f12`

---

## 📂 Challenge 2: Web Server Vulnerabilities (Directory Listing)
**Objective:** Probe server environments for structural misconfigurations exposing restricted internal index branches or developer web sheets.

### Step-by-Step Execution
1. **Server-Side Structural Enumeration:**
   * Launched an automated web security analyzer scan against the primary web host IP address (`10.6.6.100`) to uncover hidden directories or unprotected indexes:
     ```bash
     nikto -h 10.6.6.100
     ```
   * The utility successfully detected an open directory indexing configuration.

2. **Exposed Directory Traversal:**
   * Appended the discovered document indexing directory segment within the browser bar: `10.6.6.100/docs`.
   * Traversed the open structural file views, discovered the hidden utility form component named `user_form.html`, and loaded it to capture the embedded challenge flag.

---

## 🖥️ Challenge 3: Exploiting Loose SMB Server Shares
**Objective:** Map an active Local Area Network (LAN) subnet block, discover machines operating Server Message Block (SMB) protocols, and access open shares to pull sensitive business files.

### Step-by-Step Execution
1. **Network Discovery & Target Mapping:**
   * Initiated an active ping scan and port discovery sweep across the entire local subnet workspace block (`10.6.6.0/24`) to pinpoint targets running file-sharing listener ports:
     ```bash
     nmap 10.6.6.0/24
     ```
   * Identified target machine `10.6.6.23` listening on active network sharing channels.

2. **SMB Share Mapping:**
   * Leveraged specific share enumeration tools to discover file paths that permit unauthenticated or anonymous user connections:
     ```bash
     enum4linux -S 10.6.6.23
     ```
   * *Alternative Verification Script:*
     ```bash
     nmap -sV --script=smb-enum-shares 10.6.6.23
     ```

3. **Anonymous Extraction Connection:**
   * Connected directly to the target machine's open share endpoint via the native Linux SMB client framework:
     ```bash
     smbclient //10.6.6.23/print$
     ```
   * Provided the system profile password string `kali` when requested by the runtime handler to successfully pass anonymous authentication.
   * Executed remote file system commands to locate and download the hidden financial data record:
     ```smb
     ls
     cd OTHER
     get taxes.txt
     exit
     ```
   * Opened the document locally to retrieve the verification flag content:
     ```bash
     cat taxes.txt
     ```

---

## 🔍 Challenge 4: Forensic Packet Analysis (PCAP)
**Objective:** Triage raw network packet capture logs to map malicious communication strings and discover target asset files hidden across unencrypted channels.

### Step-by-Step Execution
1. **Deep Packet Analysis & Stream Triage:**
   * Initialized the Wireshark GUI tool interface to load and analyze the captured data packet trail (`.pcap`) file artifact located on the workspace disk path:
     ```bash
     wireshark /home/kali/OTHER/SA.pcap
     ```
   * Handled sequence filtering across unencrypted protocols to isolate the specific network system IP address destination alongside specific URL parameters handling file movements.

2. **Payload Retrieval:**
   * Discovered the critical server target IP address and path link from the streams: `http://10.6.6.14/data/accounts.xml`.
   * Interrogated that exact structured web asset path inside the browser client.
   * **Flag Isolation:** Inspected the structured XML code blocks to locate the required validation token within the `<Signature>` markup string tied directly to the employee listing block `ID="0"`.

---

### 🧰 Tools and Commands Cheat Sheet
* **Network Reconnaissance:** `nmap`, `enum4linux`
* **Web Directory Vulnerability Checkers:** `nikto`
* **Credential Recovery Frameworks:** `john` (John the Ripper)
* **Remote Terminal & Protocol Handlers:** `ssh`, `smbclient`
* **Network Forensics Engine:** `wireshark`

---
*Disclaimer: All processes and actions documented in this repository were executed solely inside an isolated, authorized academic training sandbox provided by Cisco Systems. No production environments were harmed.*
