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

## 🧰 Key Features 
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

   <img width="986" height="519" alt="image" src="https://github.com/user-attachments/assets/492df41f-a1e6-4a9e-a7b0-d430fd730ac2" />

## 🔹 Repeater
Burp Suite’s Repeater tool is designed for manual request manipulation. It allows you to resend HTTP/S requests with custom modifications and observe how the server responds — without needing to reload the page or interact with the browser.

Why It Matters ? 

It gives you full control over request structure and content

You can test how the server reacts to different inputs

It’s essential for verifying vulnerabilities like injection, authentication bypass, and logic flaws

📌 Key Capabilities
1. Manual Request Editing: Repeater lets you modify headers, parameters, cookies, and body content with precision — ideal for crafting payloads or testing edge cases.

2. Real-Time Response Analysis: Each request generates a response in a separate tab, allowing you to compare server behavior across different inputs.

3. Multiple Tabs: You can open multiple Repeater tabs to test variations side-by-side — useful for tracking subtle differences in responses.

4. No Automation Noise: Unlike Intruder, Repeater is fully manual, making it perfect for focused, deliberate testing without overwhelming the server.

5. Send from Proxy: You can right-click any intercepted request in Proxy and send it directly to Repeater for deeper analysis — a common workflow in web app testing.

      <img width="1868" height="540" alt="image" src="https://github.com/user-attachments/assets/9035d3fe-7a58-40c9-8988-0576b77879fc" />

📌 Repeater interface components:

1. Request List – Displays all Repeater requests; lets you manage and switch between multiple entries.

2. Request Controls – Buttons to send, cancel, or navigate through request history.

3. Request and Response View – Main area to edit requests and view server responses side by side.

4. Layout Options – Allows you to switch between horizontal, vertical, or tabbed views.

5. Inspector – Offers a structured way to analyze and modify requests beyond raw editing.

6. Target Field

   <img width="1914" height="860" alt="image" src="https://github.com/user-attachments/assets/45bdb5f6-49b7-4a5e-aecd-925939f02ff1" />



