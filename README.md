# Web-Application-Scanning-with-OWASP-ZAP
A hands-on project performing real-world security assessments using OWASP ZAP and open-source AppSec tools. Includes manual/automated DAST scanning to identify vulnerabilities like SQLi and XSS, integrates SAST/SCA tools, and demonstrates a complete reporting workflow for SDLC/SOC.

## Step 1: Configure Your Browser to Work With ZAP

OWASP ZAP works as a **Man-in-the-Middle (MITM) proxy**, capturing and analyzing traffic between your browser and the web server. To simplify the setup, we use the **Manual Explore** feature.

### Setup Instructions:
1. **Open Manual Explore:**
   In the ZAP main interface, locate and click the **Manual Explore** button.

![images alt](https://github.com/salimelh94/Web-Application-Scanning-with-OWASP-ZAP/blob/707a609b54c6b3697dab60264ccfffad262819ad/images/1.png)

   
2. **Configure the Target:**

   - **URL to Explore:** `http://testphp.vulnweb.com`
   - **Enable HUD (Heads-Up Display):** ✔ (Checked)
   - **Browser:** Select your preferred browser (Chrome, Firefox, or Edge).
   > **Note:** The HUD provides an overlay inside your browser to see vulnerabilities in real-time. It is highly recommended for beginners.
   
3. **Launch:**
   Click **"Launch Browser"**. ZAP will automatically open a new browser instance pre-configured to proxy all traffic through ZAP.

   ![images alt](https://github.com/salimelh94/Web-Application-Scanning-with-OWASP-ZAP/blob/707a609b54c6b3697dab60264ccfffad262819ad/images/2.png)


## Step 2: Explore the Target Website (Spidering)

Once the ZAP-configured browser opens, the goal is to "walk" the application. By interacting with the site, you allow ZAP to **map the entire application structure** and identify its **attack surface**.

### Activities to Perform:

* **Navigate:** Click through all available links and menu items.
* **Interact:** Fill out and submit forms (e.g., the search bar or guestbook).
* **Authenticate:** Visit the login page to let ZAP see how the app handles credentials.
* **Deep Dive:** Explore different product categories and URL parameters.

### What’s happening in the background?
As you browse, check the **Sites Tree** in the ZAP Desktop interface. You will see it populating with every URL, script, and image found. This "Passive Scan" identifies obvious issues (like missing security headers) without even sending an attack.

 ![images alt](https://github.com/salimelh94/Web-Application-Scanning-with-OWASP-ZAP/blob/7695e561b454d16a8e93ef6cedb364769c5c401e/images/3.png)


## Step 3: Run a Manual Active Scan (DAST)

After mapping the site, it’s time to perform the **Active Scan**. This is where ZAP sends modified requests (payloads) to the server to find exploitable vulnerabilities.

### Method A: Using the ZAP Desktop Interface
1. In the **Sites Tree**, locate the target URL (`http://testphp.vulnweb.com`).
2. **Right-click** on the site → **Attack** → **Active Scan**.
3. You can review advanced settings, but for this project, the **default configuration** is sufficient.
4. Click **Start Scan**.

![images alt](https://github.com/salimelh94/Web-Application-Scanning-with-OWASP-ZAP/blob/fd93123498c4b3859bf6ab381392830fe4d504bb/images/4.png)

### Method B: Using the Browser HUD
If you prefer staying in the browser:
1. Locate the **HUD panel** on the right side of the screen.
2. Find the **Active Scan icon** (the red flame symbol 🔥).
3. Click to initiate the scan directly on the page you are viewing.

![images alt](https://github.com/salimelh94/Web-Application-Scanning-with-OWASP-ZAP/blob/fd93123498c4b3859bf6ab381392830fe4d504bb/images/5.png)

### What is ZAP Testing For?
During this phase, ZAP automatically probes the application for critical flaws, including:
* **Injection Attacks:** SQL Injection (SQLi) and Command Injection.
* **Cross-Site Scripting (XSS):** Reflected and Persistent.
* **Broken Access Control:** Directory Traversal and sensitive file exposure.
* **Configuration Issues:** Cookie misconfigurations and missing Security Headers (HSTS, X-Frame-Options).
* **Information Leakage:** Outdated server versions or verbose error messages.


![images alt](https://github.com/salimelh94/Web-Application-Scanning-with-OWASP-ZAP/blob/fd93123498c4b3859bf6ab381392830fe4d504bb/images/6.png)


## 📊 Step 5: Review Vulnerabilities (Alerts Tab)

Once the Active Scan is complete, ZAP populates the **Alerts Tab** with its findings. This phase is critical for assessing the actual risk and preparing the final security report.

### 1. Understanding Risk Severities
ZAP categorizes findings using a color-coded system based on impact:

| Severity | Description |
| :--- | :--- |
| 🔴 **High** | Critical issues (e.g., SQLi) that allow full system compromise. |
| 🟡 **Medium** | Significant weaknesses (e.g., XSS) that could lead to data theft. |
| 🔵 **Low** | Minor configuration issues or best-practice violations. |
| ⚪ **Informational** | Technical insights that are not directly exploitable. |

### 2. Analyzing the Evidence
For every identified alert, ZAP provides deep technical context. When you click an alert, review the following fields:

* **URL & Parameter:** Exactly where the vulnerability exists and which input field is weak.
* **Attack / Evidence:** The specific payload ZAP used and the server's response that proved the flaw.
* **Description:** A clear explanation of what the vulnerability is.
* **Solution:** Recommended remediation steps to fix the code.


![images alt](https://github.com/salimelh94/Web-Application-Scanning-with-OWASP-ZAP/blob/6962cd001cd808741123579ac9b06ccafd3b9365/images/7.png)

---

### 💡 Validation Tip
Automated tools can sometimes produce **False Positives**. As a security analyst, you should always manually verify "High" risk alerts by attempting to replicate the attack (Evidence) in the browser to confirm it is actually exploitable.

## 📉 Scan Summary: Active Scan Results

The Active Scan was completed in **16m 37s**, executing **4,218 HTTP requests**. ZAP utilized over **40+ security plugins**, resulting in a total of **45 alerts**.

### Executive Findings Summary

| Severity | Vulnerability | Alerts | Analyst Interpretation |
| :--- | :--- | :---: | :--- |
| 🔴 **High** | **SQL Injection** | 29 | Multiple SQLi flaws (Standard & Time-Based). Targeted specifically at **MySQL**, suggesting the backend database type. |
| 🔴 **High** | **Reflected XSS** | 15 | Input parameters fail to sanitize user data, allowing script injection in real-time responses. |
| 🟡 **Medium** | **Persistent XSS** | 1 | Dangerous input is saved on the server, affecting all users who view the compromised page. |
| 🟡 **Medium** | **Server-Side Attacks**| 0 | Successfully blocked OS Command Injection, SSTI, XXE, and RCE. |
| 🟡 **Medium** | **File & Directory** | 0 | No Path Traversal or Directory Browsing vulnerabilities were found. |
| ⚪ **Info** | **Fuzzing/Recon** | 0 | Malformed User-Agent and Parameter Tampering tests produced no leaks. |

---

### 🔍 Key Vulnerability Deep-Dive

<details>
<summary><b>1. SQL Injection (SQLi) - High Risk</b></summary>

The scan reported **16 SQLi** and **13 MySQL Time-Based SQLi** alerts. 
* **Key Finding:** The lack of alerts for Oracle or PostgreSQL confirms the backend is MySQL. 
* **Technical Impact:** The application accepts unvalidated input directly into database queries. The presence of Time-Based SQLi is a "smoking gun," proving the server's predictable delay when processing malicious payloads.
</details>

<details>
<summary><b>2. Cross-Site Scripting (XSS) - High Risk</b></summary>

A total of **16 XSS** alerts were found (15 Reflected, 1 Persistent).
* **Technical Impact:** The application fails to properly encode user input. While Reflected XSS is dangerous, the **Persistent XSS** finding is critical as it indicates the database itself is storing malicious scripts.
* *Note: DOM-based XSS was skipped due to HUD browser connection constraints.*
</details>

<details>
<summary><b>3. Server-Side & File System Security - Passed</b></summary>

While the application layer is vulnerable to input attacks (SQLi/XSS), the core server configuration is robust:
* **Server-Side Protection:** Effectively mitigated RCE, XXE, and SSRF attempts.
* **Access Control:** Sensitive files (`.env`, `.htaccess`, `/WEB-INF`) are properly protected from unauthorized access or directory listing.
</details>

---

### ⚙️ Technical Scan Statistics
* **Total Runtime:** 16m 37s
* **Total HTTP Requests:** 4,218
* **Plugins Executed:** 40+
* **Skipped Tests:** * *DOM-Based XSS* (HUD connection timeout)
    * *Log4Shell* (OAST service not configured)
    * *Script-based Active Scan Rules* (No custom scripts active)











 

### 💡 Pro Tip:
If this is your first time using the HUD, follow the **built-in HUD tutorial** that appears on the right/left panels of the browser window. It provides a great walkthrough of the tool's features.
