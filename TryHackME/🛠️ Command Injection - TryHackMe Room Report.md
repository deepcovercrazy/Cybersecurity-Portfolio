# 🛠️ Command Injection - TryHackMe Room Report

---

## 🎯 Overview

In this room, we explored the **Command Injection** web vulnerability, learning:

- What command injection is  
- How to discover and exploit it on different operating systems  
- How to prevent it in applications  
- Practical application of theory in a hands-on exercise

---

## 🔍 What is Command Injection?

Command Injection is a vulnerability where an attacker abuses an application's behavior to execute arbitrary commands on the underlying **operating system** with the application's privileges.

> For example, if a web server runs as user `joe`, commands executed via injection run with `joe`'s permissions.

It’s often called **Remote Code Execution (RCE)** because it enables remote code execution inside the application environment.

### 💡 Why is it dangerous?

- Allows attackers to read sensitive files  
- Access system information  
- Run arbitrary commands, potentially compromising the entire system

---

## ⚙️ How Command Injection Works — Example Scenario

A web app lets users search for a song title by executing a system command:

```bash
grep "$title" songtitle.txt
```
$title is user input

The app runs grep with $title to find matches

Vulnerable point: If an attacker injects commands into $title, they can execute arbitrary commands, e.g.:

bash
Copy
Edit
; cat /etc/passwd
🐍 Code Examples
Python Flask example
Uses subprocess to execute commands from a URL parameter

Visiting http://flaskapp.thm/whoami runs the command whoami on the server

🔎 Detection Methods

| Method      | Description                                                                                        |
| ----------- | -------------------------------------------------------------------------------------------------- |
| **Blind**   | No output shown after running a payload. Success inferred by changes in app behavior (e.g., delay) |
| **Verbose** | Direct output of the command is shown on the application page (e.g., result of `whoami`)           |

Blind Command Injection
No immediate feedback

Use time-delay payloads like ping or sleep to test

Example payload:

bash
Copy
Edit
curl http://vulnerable.app/process.php?search=The Beatles; ping -c 5 127.0.0.1
Verbose Command Injection
Immediate feedback on command output

Easier to detect and exploit

💥 Useful Payloads

| OS          | Payload   | Description                                 |
| ----------- | --------- | ------------------------------------------- |
| **Linux**   | `whoami`  | Show current user                           |
|             | `ls`      | List directory contents                     |
|             | `ping`    | Cause delay for blind injection testing     |
|             | `sleep`   | Delay command if ping unavailable           |
|             | `nc`      | Spawn reverse shell (advanced exploitation) |
| **Windows** | `whoami`  | Show current user                           |
|             | `dir`     | List directory contents                     |
|             | `ping`    | Cause delay for blind injection testing     |
|             | `timeout` | Delay command alternative to ping           |

🛡️ Prevention Techniques
Vulnerable Functions in PHP
exec()

passthru()

system()

These functions execute system commands with provided input and can lead to command injection if used improperly.

Input Validation & Sanitization
Accept only expected data formats (e.g., digits only)

Use filtering functions, e.g., PHP’s filter_input() to allow only numeric input

Remove or escape special characters like ;, &, >

Example: Restricting input to digits only to block malicious commands.

🕵️ Bypassing Filters
Attackers may bypass filters by encoding special characters in different formats, e.g., hexadecimal encoding of quotes.

🔑 Summary
Command Injection lets attackers execute OS commands via web app input

Detection involves blind (time-based) or verbose (output-based) testing

Exploitation differs slightly based on OS (Linux vs Windows)

Prevention requires strict input validation and minimal use of dangerous functions

🧪 Practical Takeaway
You practiced exploiting and detecting command injection vulnerabilities using payloads and learned to identify the different types of injection.

