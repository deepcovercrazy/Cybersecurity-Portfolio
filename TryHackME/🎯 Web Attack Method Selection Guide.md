# 🎯 Web Attack Method Selection Guide

> \*\*Purpose:\*\*  
> Choosing the right attack method for a website depends on the \*\*reconnaissance results\*\* you gather first.  
> You must analyze the target to identify the \*\*most likely vulnerabilities\*\* before attempting specific exploits.

---

## 🗺️ Step 1 – Start with Reconnaissance

Before attacking, \*\*map out the target\*\*:
- Understand the \*\*site structure\*\*  
- Identify \*\*input points\*\*  
- Note \*\*technologies\*\* and \*\*frameworks\*\*  
- Look for \*\*authentication\*\* and \*\*API endpoints\*\*  

---

## 📊 Decision Table – When to Test Each Attack

| 🔥 Attack Type | 🔍 Initial Signs (Before Testing) | 🛠️ Detection Tools / Techniques |
|---------------|----------------------------------|----------------------------------|
| \*\*Walking an Application\*\* | First step to understand structure: links, forms, APIs | Browser, \*\*Burp Suite Spider\*\* |
| \*\*Content Discovery\*\* | Find hidden/sensitive files/directories | \*\*gobuster\*\*, \*\*dirsearch\*\* |
| \*\*Subdomain Enumeration\*\* | Target may have multiple or hidden subdomains | \*\*sublist3r\*\*, \*\*amass\*\* |
| \*\*Authentication Bypass\*\* | Login system present, possible weak login (empty password, parameter changes) | Empty input tests, \*\*Burp Intruder\*\* |
| \*\*IDOR\*\* (Insecure Direct Object Reference) | URL/API uses IDs (`id=123`) that may be altered without authorization | Manual parameter changes in \*\*Burp Repeater\*\* |
| \*\*File Inclusion (LFI/RFI)\*\* | Parameter loads files (`?file=about.php`) | Test with `../../../../etc/passwd` |
| \*\*SSRF\*\* | Parameter accepts URLs (`?url=http://...`) | Test with `http://127.0.0.1` |
| \*\*XSS\*\* | Inputs reflected directly into HTML | `` |
| \*\*Race Conditions\*\* | Sensitive operations can be triggered simultaneously | \*\*Burp Turbo Intruder\*\* |
| \*\*Command Injection\*\* | User input used in OS commands (e.g., ping) | `; ls` or `&& whoami` |
| \*\*SQL Injection\*\* | Parameters interact with a database | `' OR '1'='1`, \*\*sqlmap\*\* |

---

## 🔄 Logical Attack Flow

1. \*\*Mapping & Discovery Phase\*\*  
   - \*\*Walking an Application\*\*  
   - \*\*Content Discovery\*\*  
2. \*\*Subdomain Enumeration\*\*  
   - If the domain structure is large or complex.  
3. \*\*Targeted Testing Based on Recon Findings\*\*:
   - \*\*Login forms?\*\* → Test \*\*Authentication Bypass\*\*  
   - \*\*URLs with IDs?\*\* → Test \*\*IDOR\*\*  
   - \*\*File parameters?\*\* → Test \*\*File Inclusion\*\*  
   - \*\*URL inputs?\*\* → Test \*\*SSRF\*\*  
   - \*\*Unfiltered text output?\*\* → Test \*\*XSS\*\*  
   - \*\*Sensitive multi-step actions?\*\* → Test \*\*Race Conditions\*\*  
   - \*\*OS-level input handling?\*\* → Test \*\*Command Injection\*\*  
   - \*\*Database-driven content?\*\* → Test \*\*SQL Injection\*\*  

---

## 🧠 Pro Tip
To make attack selection \*\*fast and logical\*\*, create a \*\*decision flowchart\*\*:  
Start from \*\*Recon\*\* → Follow \*\*Yes/No questions\*\* to narrow down the right vulnerability type.

💡 This process avoids random testing and focuses on \*\*evidence-based exploitation\*\*.

---