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

so we can access the hidden page by changing our User-Agent in burp suite, by assuming its R as mentioned Agent R
<img width="1580" height="787" alt="image" src="https://github.com/user-attachments/assets/5a9bbbc2-ed94-4f77-bef1-e6bd992343f9" />

Since the message mentioned 25 agents, I figured each codename might be a single letter — probably A to Y. Instead of manually testing each one in Burp Repeater, I decided to automate the process using Burp Intruder.
I sent the intercepted request to Intruder (Ctrl + I), and in the payload position tab, I highlighted the part of the request I wanted to change. Then I switched to the Payloads tab and loaded a custom list containing single letters from A to Y, since the message hinted there are 25 agents
<img width="1898" height="867" alt="image" src="https://github.com/user-attachments/assets/3a5e6161-be9c-4c49-ba5c-02e79e9aa84a" />

Once I launched the attack in Burp Intruder, it started sending requests to the server — swapping out the User-Agent value with each letter from my payload list. Most of the responses came back with a 200 OK status, meaning nothing changed.
But when the User-Agent was set to C, the server responded with a 302 Redirect — that was the clue I needed.
<img width="1430" height="360" alt="image" src="https://github.com/user-attachments/assets/44be4a8c-256b-42e1-914b-92c2c4a0b295" />

I followed the redirect to: http://$IP/agent_C_attention.php
<img width="772" height="600" alt="image" src="https://github.com/user-attachments/assets/2b3d5fae-b11d-43af-ac28-8fa0a0444149" />

and got this message which is the answer of question 3 ( What is the agent name?) 
<img width="1021" height="205" alt="image" src="https://github.com/user-attachments/assets/0835e79d-f7a4-4e7c-ab41-57f5c9019bbf" />

## Task3: Hash cracking and brute-force
Now that I’ve got a valid username, and the hint suggests the password isn’t strong, it’s time to brute-force the login. I’ll use Hydra with the classic rockyou.txt wordlist to try and crack the FTP credentials. Let’s see if we can break in. 
- hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://10.10.247.245
  <img width="930" height="221" alt="image" src="https://github.com/user-attachments/assets/4436e5bd-e4ca-4cc2-bdb0-a7229564f97f" />

Hydra quickly returned valid credentials. I logged into the FTP server and found three files:
<img width="668" height="315" alt="image" src="https://github.com/user-attachments/assets/b1ef4210-c745-4276-970a-82d42960d586" />

I downloaded all of them using get 
<img width="946" height="368" alt="image" src="https://github.com/user-attachments/assets/2ab575d6-00a7-47fd-940a-e2995562634e" />

First, I will read the text file: 
<img width="950" height="162" alt="image" src="https://github.com/user-attachments/assets/9f614a56-74c9-4aae-b1e8-b0d72f28b6e8" />

The message was addressed to Agent J and revealed several key clues:
- The alien-like photos are fake.
- The real image is stored in Agent J’s directory.
- The login password is somehow hidden inside the fake image.
 so I ran binwalk on cutie.png, since PNGs often carry embedded data:
  <img width="943" height="239" alt="image" src="https://github.com/user-attachments/assets/70024751-cb20-42a1-877e-fc92c1ba45aa" />

This gave us a zip file, 8702.zip, which was password protected, I used zip2john to extract the hash and then cracked it with john.
After extracting the embedded zip file from cutie.png, I needed to crack its password to access To_agentR.txt. I used John the Ripper, a powerful password-cracking tool.
First, I converted the zip file into a format John could read:
- zip2john 8702.zip > john.zip
Then I ran the cracking process:
- john john.zip
John used its default wordlist and quickly found the password
<img width="820" height="246" alt="image" src="https://github.com/user-attachments/assets/27362607-2c95-491e-87e4-34eb4165e5ab" />

Now that I had the password, I could extract the contents of the zip file and move on to decoding To_agentR.txt
<img width="625" height="137" alt="image" src="https://github.com/user-attachments/assets/c211b93b-c4c2-4e15-9d9c-e6151445f494" />

Inside was a file named To_agentR.txt. When I opened it, the content looked encoded — specifically, it was base64
I decoded it using:
- echo 'QXJlYTUx' | base64 -d

This is likely the password for a steganography tool — probably used to extract hidden data from one of the images I downloaded earlier.

I ran Steghide on cute-alien.jpg, using the password:

<img width="367" height="78" alt="image" src="https://github.com/user-attachments/assets/a5086b6a-aecd-48ad-a90b-d497b2906c1f" />

- After entering the password, Steghide successfully extracted a file named message.txt
- Inside message.txt, I found credentials for another agent
- These credentials will be used for SSH access in the next task.

## Task 4: Capture the user flag 
- Now that I had the credentials for the new agent, I attempted to log into the target machine via SSH
- Once inside, I looked for the user flag. It was located in James’s home directory

<img width="398" height="101" alt="image" src="https://github.com/user-attachments/assets/bdf1527a-db42-44cd-b6c7-e5b47f76269e" />

I also found a file named Alien_autospy.jpg. I transferred it to my local machine for further inspection:

<img width="446" height="101" alt="image" src="https://github.com/user-attachments/assets/117d7dc0-d369-431a-84c9-e005712a49c9" />

where
- james@10.10.247.245: → remote user and IP
 - Alien_autospy.jpg → file to copy
 -  . → destination (your current directory)

Doing some google image search: 
<img width="579" height="173" alt="image" src="https://github.com/user-attachments/assets/9735bc9c-fba3-41f5-ade4-1c4801f94e4c" />

## Task 5: Privilege escalation
After retrieving the user flag and transferring the alien image, I began investigating ways to escalate privileges and access the root flag.

<img width="1002" height="120" alt="image" src="https://github.com/user-attachments/assets/a7571079-d61e-4f15-8708-3be48b86ff46" />

This listed the commands james could run with sudo. The output revealed: (ALL, !root) /bin/bash
- At first glance, it looks like the user james is not allowed to run /bin/bash as root. But the ALL part means the user can run it as any user, and the !root exclusion is supposed to block root access.

This caught my attention — it looked exploitable

A quick search led me to CVE-2019-14287, which affects versions of sudo before 1.8.28. The vulnerability allows a user with Runas ALL permissions to bypass the !root restriction by using a crafted UID.

Specifically, running: sudo -u#-1 /bin/bash

And it worked — I was dropped into a root shell:
<img width="692" height="82" alt="image" src="https://github.com/user-attachments/assets/ab3a2f11-ad05-457d-bf26-0914b2a2f66e" />

I navigated to the root directory and grabbed the final flag:
<img width="919" height="162" alt="image" src="https://github.com/user-attachments/assets/62e9645a-2451-4fb9-b157-beaead46b2bd" />

After escalating to root and retrieving the final flag from /root/root.txt, I found a message revealing the identity of Agent R.

This wraps up the investigation and answers the final bonus question in the room 🎁
