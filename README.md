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

1. Capture and replay requests
2. Modify parameters and headers
3. Automate attacks with payloads and fuzzing
4. Analyze responses for signs of vulnerability

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

- It allows you to pause, inspect, and modify requests before they reach the server
- You can forward, drop, or edit requests in real time
- It’s the gateway to deeper analysis using other Burp tools like Repeater and Intruder

### 📌 Key Capabilities
1. Intercepting Requests: Requests are held in the Proxy tab, giving you the chance to manipulate them before they’re sent. You can toggle interception using the "Intercept is on" button.
2. Capture and Logging: Even when interception is off, Burp logs all traffic in the HTTP history and WebSocket history tabs — perfect for retrospective analysis.
3. WebSocket Support: Burp also captures WebSocket messages, which is crucial for testing modern web apps with real-time communication.
4. Request Control: You can send intercepted requests directly to Repeater, Intruder, or other modules for deeper testing.
5. Proxy Settings: Accessible via the Proxy → Options tab, these settings let you fine-tune behavior. For example; By default, Burp doesn’t intercept server responses — but you can enable this selectively using custom rules.

   <img width="986" height="519" alt="image" src="https://github.com/user-attachments/assets/492df41f-a1e6-4a9e-a7b0-d430fd730ac2" />

## 🔹 Repeater
Burp Suite’s Repeater tool is designed for manual request manipulation. It allows you to resend HTTP/S requests with custom modifications and observe how the server responds — without needing to reload the page or interact with the browser.

Why It Matters ? 

- It gives you full control over request structure and content
- You can test how the server reacts to different inputs
- It’s essential for verifying vulnerabilities like injection, authentication bypass, and logic flaws

### 📌 Key Capabilities
1. Manual Request Editing: Repeater lets you modify headers, parameters, cookies, and body content with precision — ideal for crafting payloads or testing edge cases.
2. Real-Time Response Analysis: Each request generates a response in a separate tab, allowing you to compare server behavior across different inputs.
3. Multiple Tabs: You can open multiple Repeater tabs to test variations side-by-side — useful for tracking subtle differences in responses.
4. No Automation Noise: Unlike Intruder, Repeater is fully manual, making it perfect for focused, deliberate testing without overwhelming the server.
5. Send from Proxy: You can right-click any intercepted request in Proxy and send it directly to Repeater for deeper analysis — a common workflow in web app testing.

      <img width="1868" height="540" alt="image" src="https://github.com/user-attachments/assets/9035d3fe-7a58-40c9-8988-0576b77879fc" />

### 📌 Repeater interface components:

1. Request List – Displays all Repeater requests; lets you manage and switch between multiple entries.
2. Request Controls – Buttons to send, cancel, or navigate through request history.
3. Request and Response View – Main area to edit requests and view server responses side by side.
4. Layout Options – Allows you to switch between horizontal, vertical, or tabbed views.
5. Inspector – Offers a structured way to analyze and modify requests beyond raw editing.
6. Target Field

   <img width="1914" height="860" alt="image" src="https://github.com/user-attachments/assets/45bdb5f6-49b7-4a5e-aecd-925939f02ff1" />

### 📌 Repeater response view components: 

<img width="400" height="100" alt="image" src="https://github.com/user-attachments/assets/259215de-4555-41c7-9278-532c11673de5" />

1. Pretty – Displays the response in a clean, readable format (HTML rendered or structured JSON).
2. Raw – Shows the full unprocessed response, including headers and body.
3. Hex – Presents the response in hexadecimal format for low-level analysis.
4. Render – Attempts to visually render the response as a webpage (useful for HTML content).
5. Newline Icon (\n) – Toggles line endings for better readability in raw views.

### 📌 Repeater Inspector Components: 

<img width="298" height="248" alt="image" src="https://github.com/user-attachments/assets/f6a306b2-c600-497c-b246-1fc01821099e" />

1. Headers – Displays and allows editing of request headers (e.g., Host, User-Agent, Cookie) in a structured format.
2. Query Parameters – Lists URL parameters and lets you modify them easily without touching raw text.
3. Body Parameters – Shows form fields or JSON keys in POST requests, making it easier to test input manipulation.
4. Cookies – Separates cookies from headers for clearer inspection and editing.

## 🔹 Intruder
Burp Suite’s Intruder tool is designed for automated request manipulation and payload injection. It allows you to send multiple variations of a request to a target and analyze how the server responds — ideal for fuzzing, brute-forcing, and vulnerability discovery.

Why It Matters ?
 
- It automates repetitive testing tasks like fuzzing and brute-force attacks
- You can test multiple payloads across different parameters efficiently
- It’s essential for discovering input-based vulnerabilities like SQLi, XSS, and authentication flaws

### 📌 Intruder interface components:

<img width="1352" height="700" alt="image" src="https://github.com/user-attachments/assets/dbd2d742-972a-4f17-b1ff-53411e5fe11c" />


1. Target – Displays the host and port of the target server. Auto-filled when sending a request from Proxy or Repeater.
2. Positions Tab – Lets you define which parts of the request will be replaced with payloads. You highlight the insertion points here.
3. Payloads Tab – Configure payload types, wordlists, encoding, and payload processing rules.
4. Options Tab – Fine-tune attack behavior, grep settings, redirection handling, and response analysis preferences.
5. Results Tab – Shows all responses from the attack, sortable by status code, response length, and custom match rules.
6. Attack Controls – Buttons to start, pause, resume, or stop the attack and monitor its progress.
7. Request Preview – Displays the raw request being sent, including payload markers (§) for visual reference.
8. Payload Position Markers – Highlighted with § symbols in the request to indicate where payloads will be injected.

### 📌 Intruder Payloads Tab Components 

<img width="1295" height="809" alt="image" src="https://github.com/user-attachments/assets/07e38c1b-be63-4b50-96c5-a2bec962ef0c" />

The Payloads tab in Burp Suite Intruder is divided into four key sections, each offering granular control over how payloads are created, processed, and encoded:

1. Payload Options

- Configure the payload position, payload type, and payload set number.
- Displays the request count for the attack.
- The dropdowns adjust based on the selected attack type (Sniper, Battering Ram, Pitchfork, Cluster Bomb).

2. Payload Configuration

- Add payloads manually using the Add box.
- Use buttons like Paste, Load, Save, Remove, Clear, and Deduplicate to manage your payload list.
- This section is dynamic — options change depending on the selected payload type (e.g., Simple list, Runtime file, Numbers).

3. Payload Processing

- Define rules to transform or filter payloads before sending.
- Use Add, Edit, Remove, Up, and Down to manage rule order and behavior.
- Examples: capitalize words, apply regex filters, encode values.

4. Payload Encoding

- Customize how payloads are encoded before transmission.
- Options include: URL-encode these characters, URL-encode all characters
- Useful for bypassing filters or matching expected input formats.


### 📌 Intruder Attack Types

1. **Sniper**  
   - Uses **one payload set**  
   - Sends payloads **one at a time** into each marked position  
   - Ideal for testing **single-parameter vulnerabilities** like XSS or SQLi  
   - Example: password brute-force or fuzzing for API endpoints

2. **Battering Ram**  
   - Uses **one payload set**  
   - Inserts the **same payload** into **all marked positions simultaneously**  
   - Useful when multiple parameters are expected to match or behave similarly  
   - Example: test the same payload against multiple positions at once 

3. **Pitchfork**  
   - Uses **multiple payload sets**  
   - Inserts **one payload per position**, advancing **each set in parallel**  
   - Great for testing **paired inputs** like username/password combos  
   - Example: Brute-forcing credentials 

4. **Cluster Bomb**  
   - Uses **multiple payload sets**  
   - Inserts **every possible combination** of payloads across all positions  
   - Most thorough but also the slowest — ideal for **complex multi-field testing**  
   - Example: Testing combinations of input fields for logic flaws or chained injections
  
## 🔹 Decoder
Burp Suite’s Decoder tool is designed for manual encoding and decoding of data. It helps you transform strings between formats like Base64, URL encoding, HTML encoding, and more — making it essential for analyzing obfuscated or encoded payloads.

Why It Matters ? 
- It helps you understand and manipulate encoded or obfuscated data
- You can decode intercepted payloads or responses for analysis
- It’s useful for crafting encoded payloads during testing

### 📌 Decoder Interface Components:

<img width="1918" height="381" alt="image" src="https://github.com/user-attachments/assets/581bcf2d-5ae4-4cf3-9c08-73e8ec4af431" />

1. Input Panel
   - The top red-outlined box is where you paste or type the data you want to decode or encode.
   - You can work with plain text, encoded strings, or obfuscated payloads.

2. Output Panel
   - The bottom red-outlined box displays the result of the transformation — whether it’s decoded, encoded, or hashed.
   - You can copy the output or send it to other Burp tools like Repeater or Intruder.

3. Dropdown Menus (Right Side of Each Panel)
  - Text – Specifies the format of the input/output (e.g., plain text, hex, Base64).
  - Decode as... – Lets you choose the decoding method (e.g., URL, HTML, Base64).
  - Encode as... – Lets you choose the encoding method for the output.

4. Smart Decode – Automatically detects and applies the correct decoding method — useful for layered encodings.

### 📌 Decoder Hashing Features

<img width="192" height="148" alt="image" src="https://github.com/user-attachments/assets/c13be3a5-315b-4f25-88d3-eec62b98452f" />
Burp Suite’s Decoder tab includes built-in support for generating cryptographic hashes from any input string. This is useful for analyzing or crafting data used in authentication, integrity checks, or obfuscated payloads.

🔹 Supported Hashing Algorithms
MD5, SHA-1, SHA-256, SHA-384, SHA-512
💡 Note: A hashing algorithm’s output is binary — not readable ASCII or Unicode text. That’s why it’s typically converted into a hexadecimal string, which is the familiar “hash” format you see in tools and databases.

### 🔹 Comparer
Burp Suite’s Comparer tool is designed for side-by-side analysis of data. It helps you identify differences between two requests, responses, or strings — making it ideal for spotting subtle changes in server behavior, session tokens, or application logic.

Why It Matters ? 
- It reveals hidden changes between two pieces of data
- You can compare responses from different payloads or user roles
- It’s essential for analyzing authentication flows, session management, and logic bypasses

### 📌 Key Capabilities
1. Byte-Level Comparison: Shows differences at the binary level — useful for spotting encoding changes or subtle token shifts.
2. Text Comparison: Highlights differences in readable text — ideal for comparing HTML, JSON, or error messages.
3. Visual Diffing: Uses color-coded highlights to make differences easy to spot — especially useful when analyzing long responses.

### 📌 Comparer Interface Components:

<img width="1919" height="877" alt="image" src="https://github.com/user-attachments/assets/5f71b6b1-be71-4d56-b4a5-da865825ef99" />

Perfect, Tasneem — here’s a clean breakdown of the **📌 Comparer interface components**, styled to match your guide and based on your description:

---

### 📌 Comparer Interface Components

1. **Data Table (Left Panel)**  
   - Displays the items to be compared.  
   - Each row represents a dataset you’ve loaded or pasted.  
   - You select **two rows** to perform a comparison.

2. **Data Management Controls (Upper Right)**  
   - **Paste** – Inserts clipboard content as a new dataset.  
   - **Load** – Imports data from a file.  
   - **Remove** – Deletes the currently selected row.  
   - **Clear** – Removes all datasets from the table.

3. **Comparison Mode Buttons (Lower Right)**  
   - Choose how to compare the selected datasets:  
     - **Words** – Compares based on readable text.  
     - **Bytes** – Compares at the binary level.  
   - You can switch between modes anytime — the initial choice isn’t permanent.  
   - These buttons trigger the actual comparison once two datasets are selected.

### 🔹 Sequencer
Burp Suite’s Sequencer tool is designed for analyzing the randomness and predictability of tokens — such as session IDs, CSRF tokens, or password reset links. It helps you determine whether a web application is generating secure, unpredictable values.

Why It Matters ? 
- It evaluates the entropy of tokens used in authentication and session management
- You can detect weak randomness that could lead to token prediction or session hijacking
- It’s essential for assessing the strength of security-critical identifiers

### 📌 Sequencer Interface Components

<img width="1193" height="612" alt="image" src="https://github.com/user-attachments/assets/78bb1cce-4edd-43da-bb76-1bad952f691b" />

1. **Select Live Capture Request**  
   - This section lets you configure how Burp Suite will collect tokens for analysis.  
   - You can send requests from **Proxy**, **Repeater**, or other tools into Sequencer.  
   - The table displays captured requests with columns like **Host** and **Request**.  
   - Controls include:  
     - **Start live capture** – Begins collecting tokens from selected requests  
     - **Remove** – Deletes a selected request from the list  
     - **Clear** – Empties the entire request list

2. **Token Location Within Response**  
   - Defines where Burp should extract the token from each response.  
   - You can choose from three options:  
     - **Cookie** – Extracts tokens from Set-Cookie headers  
     - **Form Field** – Extracts tokens from hidden or visible form fields  
     - **Custom Location** – Lets you manually configure token extraction using response structure or regex  
   - The **Configure** button opens a dialog to fine-tune custom extraction rules

### 🔹 Organizer
Burp Suite’s Organizer tool is designed to help you manage and annotate your testing workflow. It allows you to group, tag, and comment on requests, responses, and findings — making it easier to track progress, revisit key observations, and prepare for reporting.

Why It Matters ? 
- It helps you stay organized during complex or multi-phase testing
- You can tag and comment on important requests or findings
- It’s ideal for building structured notes and preparing for client reports or portfolio documentation

  <img width="1315" height="716" alt="image" src="https://github.com/user-attachments/assets/8815f7e8-4f99-492b-bb7f-ec34eb0baa56" />

### 🔹 Extensions
Burp Suite’s Extensions tab gives you access to the Burp Extender API, allowing you to install, manage, and develop custom plugins that enhance Burp’s functionality. It’s a powerful way to tailor the tool to your workflow or integrate with external systems.

<img width="1908" height="600" alt="image" src="https://github.com/user-attachments/assets/a574ca7a-c350-4df7-b843-8551fb053d1e" />

Why It Matters
- You can extend Burp’s capabilities with custom scripts and third-party plugins
- It supports automation, integration, and customization for advanced testing
- It’s essential for building repeatable workflows, custom scanners, or tool integrations

# ✅ To put everything we've learned into practice, we'll now move on to solving relevant TryHackMe challenges.
