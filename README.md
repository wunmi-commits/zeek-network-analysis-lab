# Zeek Network Security Monitoring Lab

## 📖 Overview
This repository documents my completion of the TryHackMe Zeek room. Zeek (formerly Bro) is a powerful, open-source network security monitor. This project demonstrates my ability to use Zeek for network traffic analysis (PCAP forensics), writing custom signatures, utilizing built-in frameworks (Intel, GeoIP, File Extraction), and analyzing generated log files.

## 🛠️ Tools & Technologies
*   **Operating System:** Ubuntu (Linux)
*   **Network Monitor:** Zeek (v4.2.1)
*   **Traffic Analysis:** CLI (`zeek-cut`, `grep`, `sort`, `uniq`)
*   **Core Skills:** Network Forensics, Signature Creation, Log Analysis, Scripting.

## 📂 Repository Contents
*   **`01-Traffic-Signatures/`**: Custom `.sig` files created to detect cleartext passwords and brute-force attempts.
*   **`02-Custom-Scripts/`**: Zeek scripts used for counting connections and analyzing HTTP status codes.
*   **`03-Investigations/`**: Detailed markdown write-ups explaining the methodology used to analyze specific PCAP files (HTTP, DHCP, Intel Framework).

---

## 🔍 Key Investigations & Findings

Below is a summary of the investigations performed and the commands used to uncover the findings. 

### 1. Custom Signatures (HTTP & FTP)
*   **Objective:** Detect cleartext credentials and brute-force attempts.
*   **Action:** Created `http-password.sig` and `ftp-bruteforce.sig`. Note: During this process, I learned how Zeek's regex engine handles special characters (resolving "error compiling pattern /?/?" issues by ensuring proper syntax `.*`).
*   **Verification:** Used `zeek -C -r http.pcap -s http-password.sig` and analyzed `signatures.log`.
*   **Finding:** Identified cleartext password events and successfully counted 1410 brute-force matches.

### 2. Log Analysis (DHCP)
*   **Objective:** Identify unique hostnames in a large capture (`bigFlows.pcap`).
*   **Command:** `cat dhcp.log | zeek-cut host_name | grep -v "-" | sort -u | wc -l`
*   **Finding:** Identified **17** unique hostnames, ensuring old logs were cleared before generating new ones.

### 3. The Intelligence Framework
*   **Objective:** Utilize Zeek's Intel framework to match traffic against a threat intelligence feed.
*   **Command:** `zeek -C -r case1.pcap intelligence-demo.zeek`
*   **Finding:** Identified a threat indicator matching `IN_HOST_HEADER` by analyzing `intel.log`.

### 4. File Extraction & GeoIP
*   **Objective:** Extract downloaded executables and map IPs to geographic locations.
*   **Action:** Used `file-extract-demo.zeek` to extract `Down.exe` from HTTP traffic. Used `geoip-conn` to resolve IP `23.77.86.54` to **Chicago**.

---

## 💡 Lessons Learned & Troubleshooting
*   **Log Generation:** Zeek only generates `.log` files (like `conn.log`, `dhcp.log`) *after* successfully processing a `.pcap` file. If a log is missing, the script or pcap execution failed.
*   **Overwriting Logs:** Zeek writes logs to the *current working directory*. Navigating to a new folder without clearing old logs (or running new pcaps) will result in analyzing old data.
*   **Regex Compilation:** Zeek signature files are strict. Ensuring `.` and `*` are properly formatted is crucial to avoid `fatal error: errors occurred while initializing`.

---
*Disclaimer: All activities were conducted in a controlled, isolated TryHackMe lab environment for educational purposes.*

Author: Wunmi Adam.   
Project: zeek-network-analysis-lab.   
Focus: Cybersecurity | Threat Analysis | Defensive Security.  
