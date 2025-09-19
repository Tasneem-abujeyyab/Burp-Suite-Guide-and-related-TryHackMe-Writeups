# 🧪 TryHackMe — Agent Sudo Walkthrough 
In this challenge, we dive into a secret server hidden beneath the deep sea 🌊. The room is beginner-friendly but covers a wide range of tools and techniques — making it ideal for practicing multi-step enumeration, exploitation, and privilege escalation.

## 🛠 Tools & Techniques Used
- 🐧 Basic Linux Commands — navigation, file inspection, privilege checks
- 🔍 Nmap — port scanning and service enumeration
- 🧪 Burp Suite — intercepting and modifying HTTP headers
- 🔐 Hydra — brute-forcing FTP credentials
- 🔓 John the Ripper — cracking password hashes
- 🖼 Steghide — extracting hidden data from image files

## Task2: Enumerate the machine and get all the important information
### How many open ports?
I will start by using Nmap to scan for open ports and services: 

<img width="808" height="373" alt="image" src="https://github.com/user-attachments/assets/feb3deae-6bec-49a0-90e9-e17c5ed9627a" />
Findings:
- Port 21: FTP
- Port 22: SSH
- Port 80: HTTP
### How you redirect yourself to a secret page?
I visited the web server on port 80. The homepage mentioned that agents should use their codename as the User-Agent to access hidden content
<img width="724" height="470" alt="image" src="https://github.com/user-attachments/assets/22996738-5426-4863-b942-5458989162f2" />

