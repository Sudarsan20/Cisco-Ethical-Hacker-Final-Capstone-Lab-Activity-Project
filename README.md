# Cisco Ethical Hacker - Final Capstone Activity Write-Up

🛡️ **Verified Certification:** [![Cisco Ethical Hacker Badge](https://img.shields.io/badge/Credly-Verified_Badge-007A87?style=flat&logo=credly&logoColor=white)](https://www.credly.com/badges/1dc6334e-82fb-44e0-aa82-c08afed5a2e6)

## 🎖️ Certification Project Overview
This repository contains the walkthrough and write-up for the **Final Capstone Activity** of the Cisco Ethical Hacker course. Having successfully passed this capstone and earned my certification, this documentation acts as a detailed breakdown of the methodologies, penetration testing steps, and remediation processes applied during the lab assessment.

### 🌐 Scenario / Background
I was tasked to conduct a simulated penetration test for a corporate network environment. The target networks involved hosts residing within the **10.6.6.0/24** and **172.17.0.0/24** ranges. The main objective was to discover vulnerabilities, execute successful exploits, capture flags/codes, and document strict mitigation and remediation protocols to protect the affected systems.

---

## 🛠️ Challenge 1: SQL Injection & Password Cracking
**Objective:** Exploit a web application input field using SQL injection techniques to harvest system credentials, crack a specific user hash (**Gordon Brown**), and pivot to a separate target machine.

### Step-by-Step Execution
1. **Target Access & Setup:** * Formed a connection to the target web application interface hosting the Damn Vulnerable Web Application (DVWA) at `http://10.6.6.100`.
   * Authenticated using initial credentials (`admin / password`) and initialized the environment by adjusting the security level profile to **Low**.
2. **Vulnerability Assessment & Injection:**
   * Enumerated the platform for parameterized inputs and discovered a vulnerable text field susceptible to **SQL Injection (SQLi)**.
   * Injected payload logic to extract hidden backend database components (tables, schemas, columns) containing credential databases.
   * Successfully retrieved user records, isolating **Gordon Brown's** username and corresponding password hash value.
3. **Password Cracking:**
   * Inputted the collected cryptographic hash into offline cracking utilities (e.g., John the Ripper or Hashcat) alongside wordlist dictionaries to successfully crack the plaintext password.
4. **Pivoting & Flag Extraction:**
   * Leveraged the recovered plaintext credentials of Gordon Brown to authenticate onto the system located at network endpoint `172.17.0.2`.
   * Identified and unsealed the primary challenge file to extract the hidden confirmation code flag.

### 🛡️ Remediation Strategy
* **Prepared Statements:** Standardize the use of Parameterized Queries (Prepared Statements) for all SQL interactions to prevent runtime code misinterpretation.
* **Input Validation & Sanitization:** Enforce rigid server-side data allow-lists to block irregular payload syntax characters (such as `'`, `--`, `UNION`).

---

## 📂 Challenge 2: Information Disclosure via Directory Listing
**Objective:** Perform reconnaissance against the target network structure to identify data disclosure points via unprotected file indexing structures.

### Step-by-Step Execution
1. **Directory Enumeration:**
   * Utilized asset discovery scanners or manual URL profiling to identify directory indexing vulnerabilities on the application server.
   * Leveraged improper configurations allowing full visualization of directory roots containing core programming structures.
2. **Flag Retrieval:**
   * Navigated the exposed files system hierarchy to pin down the filename handling the explicit **Challenge 2** script.
   * Evaluated the underlying codebase assets to extract the hidden segment logic.

### 🛡️ Remediation Strategy
* **Disable Directory Indexing:** Modify the webserver master configuration files (e.g., `Options -Indexes` in Apache `httpd.conf` or removing directory browsing configs in Nginx/IIS) to prevent automated directory parsing.
* **Implement Index Catchers:** Deploy default, non-disclosing structural endpoints (such as blank or generic redirect `index.html` files) in asset folders.

---

## 🖥️ Challenge 3: SMB Share Vulnerability Exploitation
**Objective:** Profile a network node running Server Message Block (SMB) services to look for loose structural accesses and extract target files.

### Step-by-Step Execution
1. **Network Profiling & Enumeration:**
   * Ran targeted SMB inspection tools (such as `smbclient`, `enum4linux`, or Nmap SMB scripts) to systematically inventory accessible network resource paths.
   * Checked for unauthenticated or NULL session capabilities against the network endpoint.
2. **Exploiting Lax Configurations:**
   * Discovered loose shares that allowed connection read-rights without providing valid corporate credentials.
   * Connected directly to the public share path, traversed directory structures, and located and downloaded hidden challenge data artifacts.

### 🛡️ Remediation Strategy
* **Disable Anonymous Sessions:** Enforce complete restriction of Null Sessions and Anonymous access across the SMB server registry configurations.
* **Access Control Lists (ACLs):** Restrict sharing permissions to explicit authenticated groups, block SMB traffic across untrusted border barriers using local firewalls, and force the implementation of newer, encrypted versions of the protocol (SMBv3).

---

## 🔒 Post-Exploitation & Final File Protection Recommendations
To guarantee a solid defense-in-depth security structure across all completed lab environments and prevent unauthorized entities from looking into sensitive target records, the following standard methodologies were documented:

1. **Robust Role-Based Access Controls (RBAC):** Restrict system file access using rigorous operating system permissions (e.g., strict Linux `chmod` profiles or Windows NTFS ACLs) to verify that only authorized identities can interact with target files.
2. **Data-at-Rest Encryption:** Apply strong standard encryption mechanisms (such as AES-256 file/folder protection frameworks) to protect critical data payloads. Even if directory traversal or storage exfiltration occurs, raw contents remain unreadable without appropriate cryptographic material.

---
### ⚙️ Technologies & Tooling Profiled
* **Reconnaissance & Scan Engines:** Nmap, Directory Enumeration Utilities
* **Database Exploitation:** SQL Injection Payload Implementation
* **Credential Recovery:** Hashcat / John the Ripper
* **Network & File Systems Protocol Handlers:** SMB Client Tools, Web Browser Dev Panels

*Disclaimer: This write-up represents a completed exercise conducted solely inside a controlled network simulation environment for academic validation purposes.*
