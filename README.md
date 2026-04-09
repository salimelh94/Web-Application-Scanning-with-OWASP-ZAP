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













 

### 💡 Pro Tip:
If this is your first time using the HUD, follow the **built-in HUD tutorial** that appears on the right/left panels of the browser window. It provides a great walkthrough of the tool's features.
