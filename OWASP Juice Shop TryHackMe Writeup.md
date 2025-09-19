# 🧪 TryHackMe — Surfer Challenge Writeup
Welcome to a quirky web application challenge where we’ll uncover hidden internal features by intercepting and manipulating requests. This room is a great hands-on exercise in Server-Side Request Forgery (SSRF) and access control bypass — perfect for practicing Burp Suite techniques.

## 🚀 Getting Started
1. Deploy the vulnerable machine using the green “Start Machine” button.
2. Launch the AttackBox from the top-right corner of the page.
3. Navigate to the app using: http://MACHINE_IP

### 🔍 Initial Recon: Directory Fuzzing & Discovery
Started with basic directory fuzzing to map out accessible paths.


Found a login page and a robots.txt file.
robots.txt revealed a disallowed path: /backup/chat.txt 

<img width="732" height="179" alt="image" src="https://github.com/user-attachments/assets/71d7761e-57e5-4878-add1-8cef6f569b4a" />

This hinted at sensitive content being hidden from crawlers

📁 Exploring /backup/chat.txt
During initial recon, I accessed the path /backup/chat.txt, which revealed a chat log between two internal users — Admin and Kate. The conversation hinted at:

<img width="832" height="401" alt="image" src="https://github.com/user-attachments/assets/7114a40e-c2bb-4e97-a6cf-29b2fa4df377" />

A newly deployed export2pdf tool

A requirement for daily system reports in PDF format

Confirmation that the internal server now serves the flag

A warning about weak credentials

🧠 Key Takeaway: This chat confirms that the flag is hosted internally and may be accessible via the PDF export feature. It also suggests that the admin might still be using admin:admin as login credentials — worth testing.

Logged in successfully as admin

<img width="1910" height="795" alt="image" src="https://github.com/user-attachments/assets/3c6f465d-a84f-4bcf-8fc5-1b9d16cc0c94" />

While exploring the application, I accessed the Recent Activity section, which revealed a timeline of system events. One entry stood out:

 Internal pages hosted at /internal/admin.php. It contains the system flag.

<img width="512" height="321" alt="image" src="https://github.com/user-attachments/assets/6c803e73-fd76-4845-8914-fcc4b6505fcc" />

