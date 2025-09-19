# 🧪 TryHackMe — Surfer Challenge Writeup
Welcome to a quirky web application challenge where we’ll uncover hidden internal features by intercepting and manipulating requests. This room is a great hands-on exercise in Server-Side Request Forgery (SSRF) and access control bypass — perfect for practicing Burp Suite techniques.

## 🚀 Getting Started
1. Deploy the vulnerable machine using the green “Start Machine” button.
2. Launch the AttackBox from the top-right corner of the page.
3. Navigate to the app using: http://MACHINE_IP

### 🔍 Initial Recon: Directory Fuzzing & Discovery
Started with basic directory fuzzing to map out accessible paths.
- gobuster dir --url http://10.10.91.108/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt

Found a login page and a robots.txt file.
robots.txt revealed a disallowed path: /backup/chat.txt 

<img width="732" height="179" alt="image" src="https://github.com/user-attachments/assets/71d7761e-57e5-4878-add1-8cef6f569b4a" />

This hinted at sensitive content being hidden from crawlers

📁 Exploring /backup/chat.txt
During initial recon, I accessed the path /backup/chat.txt, which revealed a chat log between two internal users — Admin and Kate. The conversation hinted at:

<img width="832" height="401" alt="image" src="https://github.com/user-attachments/assets/7114a40e-c2bb-4e97-a6cf-29b2fa4df377" />

- A newly deployed export2pdf tool
- A requirement for daily system reports in PDF format
- Confirmation that the internal server now serves the flag
- A warning about weak credentials

🧠 Key Takeaway: This chat confirms that the flag is hosted internally and may be accessible via the PDF export feature. It also suggests that the admin might still be using admin:admin as login credentials — worth testing.

Logged in successfully as admin

<img width="1910" height="795" alt="image" src="https://github.com/user-attachments/assets/3c6f465d-a84f-4bcf-8fc5-1b9d16cc0c94" />

While exploring the application, I accessed the Recent Activity section, which revealed a timeline of system events. One entry stood out:

 Internal pages hosted at /internal/admin.php. It contains the system flag.

<img width="512" height="321" alt="image" src="https://github.com/user-attachments/assets/6c803e73-fd76-4845-8914-fcc4b6505fcc" />

This page hosts the flag, but direct access is blocked

<img width="715" height="235" alt="image" src="https://github.com/user-attachments/assets/61f4e122-3fb3-4de3-b1e9-e8d0524a0e2d" />

Found a PDF generation feature that exports server metrics. The service pulls content from internal endpoints and renders it as a PDF — a potential SSRF vector.

<img width="1907" height="861" alt="image" src="https://github.com/user-attachments/assets/dd6142fb-7077-4f35-9905-719c80d4d5e2" />

<img width="1045" height="745" alt="image" src="https://github.com/user-attachments/assets/d2f38d80-bf1d-4734-9436-208ea1b0e540" />


### 🛠 Exploitation Steps
Step 1: Intercepted the request using Burp Suite Proxy while exporting the default server-info PDF.

<img width="1906" height="893" alt="image" src="https://github.com/user-attachments/assets/189de8b0-12f1-48ce-a479-2e22fd08df41" />

Step 2: Modified the request URL to target: /internal/admin.php and forward the request 

<img width="1233" height="775" alt="image" src="https://github.com/user-attachments/assets/09b0ade5-1539-4297-987a-71b152721b3f" />

✅ Result: The PDF response contained the flag

<img width="614" height="245" alt="image" src="https://github.com/user-attachments/assets/684ff142-e3fb-4fdd-a951-fbe5c154b2a8" />




