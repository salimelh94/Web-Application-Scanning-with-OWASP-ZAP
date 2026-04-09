# Web-Application-Scanning-with-OWASP-ZAP
A hands-on project performing real-world security assessments using OWASP ZAP and open-source AppSec tools. Includes manual/automated DAST scanning to identify vulnerabilities like SQLi and XSS, integrates SAST/SCA tools, and demonstrates a complete reporting workflow for SDLC/SOC.

## Step 1: Configure Your Browser to Work With ZAP

OWASP ZAP works as a **Man-in-the-Middle (MITM) proxy**, capturing and analyzing traffic between your browser and the web server. To simplify the setup, we use the **Manual Explore** feature.

### Setup Instructions:
1. **Open Manual Explore:**
   In the ZAP main interface, locate and click the **Manual Explore** button.

![images alt](https://github.com/salimelh94/Web-Application-Scanning-with-OWASP-ZAP/blob/707a609b54c6b3697dab60264ccfffad262819ad/images/1.png)

   
3. **Configure the Target:**

   - **URL to Explore:** `http://testphp.vulnweb.com`
   - **Enable HUD (Heads-Up Display):** ✔ (Checked)
   - **Browser:** Select your preferred browser (Chrome, Firefox, or Edge).
   > **Note:** The HUD provides an overlay inside your browser to see vulnerabilities in real-time. It is highly recommended for beginners.
   
5. **Launch:**
   Click **"Launch Browser"**. ZAP will automatically open a new browser instance pre-configured to proxy all traffic through ZAP.

   ![images alt](https://github.com/salimelh94/Web-Application-Scanning-with-OWASP-ZAP/blob/707a609b54c6b3697dab60264ccfffad262819ad/images/2.png)

### 💡 Pro Tip:
If this is your first time using the HUD, follow the **built-in HUD tutorial** that appears on the right/left panels of the browser window. It provides a great walkthrough of the tool's features.
