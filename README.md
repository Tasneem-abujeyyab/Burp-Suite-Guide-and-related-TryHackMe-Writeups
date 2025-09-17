# Burp-Suite-Guide-and-related-TryHackMe-Writeups
## 📍 Introduction
Burp Suite is one of the most powerful tools in a web security analyst’s arsenal — and it’s often the first hands-on experience many aspiring professionals have with real-world vulnerability testing. Whether you're exploring injection flaws, broken authentication, or insecure deserialization, Burp gives you the ability to intercept, manipulate, and analyze HTTP traffic like a pro.

This guide is built from my own learning journey — not just theory, but actual practice through TryHackMe rooms, lab walkthroughs, and real debugging sessions. I created it for learners like me: those who want to understand how Burp works, what each feature does, and how to apply it in realistic scenarios without getting lost in jargon.

If you're just starting out or want to sharpen your Burp skills through practical exercises, you're in the right place.

## 💡 What You’ll Learn
How to set up Burp Suite and configure your browser proxy

What each core module (Proxy, Repeater, Intruder, etc.) does

How to use Burp Suite in TryHackMe rooms to exploit and analyze vulnerabilities

Tips for filtering noisy traffic, handling HTTPS, and chaining Burp with other tools

How to document your findings for reports

## 🧭 What Is Burp Suite and Why It Matters
Burp Suite is a powerful web vulnerability scanner and traffic interception tool used by security professionals, penetration testers, and bug bounty hunters to assess the security of web applications. It acts as a man-in-the-middle proxy, allowing you to intercept, inspect, and manipulate HTTP and HTTPS traffic between your browser and a target application.

What makes Burp Suite essential is its versatility. Whether you're testing for SQL injection, cross-site scripting (XSS), insecure authentication, or broken access controls, Burp provides the tools to:

Capture and replay requests

Modify parameters and headers

Automate attacks with payloads and fuzzing

Analyze responses for signs of vulnerability

In real-world testing scenarios Burp Suite becomes the central hub for understanding how a web app behaves under pressure. It’s not just about finding bugs; it’s about learning how applications communicate, where they break, and how attackers exploit those weaknesses.

## 🛠️ Installation and Setup
Burp Suite is available in both Community Edition (free) and Professional Edition (paid). For learning and TryHackMe labs, the Community Edition is more than enough.

1. Download Burp Suite from PortSwigger’s official site

2. Install it like any standard application (Windows, macOS, or Linux)

3. Launch Burp Suite and select Temporary Project → Use Burp Defaults

4. Click Start Burp to open the main dashboard

## 🧰 Key Features Explained
Burp Suite is more than just a proxy — it’s a full toolkit for web application testing. Each module serves a specific purpose, and learning how to use them together is what turns you from a passive observer into an active analyst.

## 🔹 Proxy
The Burp Proxy is the core engine of Burp Suite — it intercepts and logs all HTTP/S traffic between your browser and the target web server. This is where you gain full visibility and control over how the application communicates.

Why It Matters ? 
It allows you to pause, inspect, and modify requests before they reach the server

You can forward, drop, or edit requests in real time

It’s the gateway to deeper analysis using other Burp tools like Repeater and Intruder

📌 Key Capabilities
1. Intercepting Requests: Requests are held in the Proxy tab, giving you the chance to manipulate them before they’re sent. You can toggle interception using the "Intercept is on" button.

2. Capture and Logging: Even when interception is off, Burp logs all traffic in the HTTP history and WebSocket history tabs — perfect for retrospective analysis.

3. WebSocket Support: Burp also captures WebSocket messages, which is crucial for testing modern web apps with real-time communication.

4. Request Control: You can send intercepted requests directly to Repeater, Intruder, or other modules for deeper testing.

5. Proxy Settings: Accessible via the Proxy → Options tab, these settings let you fine-tune behavior. For example; By default, Burp doesn’t intercept server responses — but you can enable this selectively using custom rules.

