# 🕵️‍♂️ Web Content Discovery Report - TryHackMe

* * *

## 📌 Introduction

In the context of **web application security**, what is **content**?

Content can be many things: files, videos, pictures, backups, or website features.

When we talk about **content discovery**, we mean uncovering hidden or unintended areas of a website — things not immediately visible or meant for public access. Examples include:

- Staff-only pages or portals
- Old website versions
- Backup or configuration files
- Admin panels

We’ll explore **three main methods** to discover hidden content:

1. Manual
2. Automated
3. OSINT (Open-Source Intelligence)

* * *

## 🛠️ Manual Content Discovery Techniques

### 🔍 robots.txt

`robots.txt` guides search engines on which pages to crawl or avoid. While designed to keep some areas hidden from search results (like admin panels or customer files), it ironically provides a great roadmap for penetration testers to find sensitive locations.

* * *

### 🖼️ Favicon

Favicons are small icons shown in browser tabs. Sometimes, default favicons reveal the underlying **framework** used to build the website, helping attackers identify the technology stack.

> 
> 
> **Example:**
> 
> On the TryHackMe AttackBox, downloading and hashing the favicon can help identify frameworks by matching the MD5 hash with [OWASP's favicon database](https://wiki.owasp.org/index.php/OWASP_favicon_database).
> 

```
bashCopyEditcurl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum
```

*Note: Free AttackBox users may need to run this on a local VM or PowerShell.*

* * *

### 🗺️ Sitemap.xml

Unlike `robots.txt` which blocks access, `sitemap.xml` lists files the website owner *wants* indexed. It can reveal hidden or forgotten pages not easy to find by browsing.

* * *

### 🖥️ HTTP Headers

HTTP headers provide info about server software and languages in use, which might help find vulnerable versions.

Example output from:

```
bashCopyEditcurl http://10.10.72.194 -v
```

```
yamlCopyEdit< Server: nginx/1.18.0 (Ubuntu)
< X-Powered-By: PHP/7.4.3
```

* * *

### 🧱 Framework Stack

By inspecting favicons, page source comments, or copyright notices, you can identify:

- The framework in use
- Admin portal paths
- Useful external documentation

> 
> 
> Example: The Acme IT Support website linked to its framework documentation at:
> 
> `https://static-labs.tryhackme.cloud/sites/thm-web-framework`
> 
> This helped find an admin portal with a flag.
> 

* * *

## 🌐 OSINT (Open-Source Intelligence) Tools

### 🔎 Google Hacking / Dorking

Google advanced search filters can reveal sensitive content indexed on the web.

| Filter | Example | Description |
| --- | --- | --- |
| site | `site:tryhackme.com` | Limits search to a specific domain |
| inurl | `inurl:admin` | Finds URLs containing a keyword |
| filetype | `filetype:pdf` | Finds files of specific extensions |
| intitle | `intitle:admin` | Searches titles containing keyword |


Learn more: [Google Hacking - Wikipedia](https://en.wikipedia.org/wiki/Google_hacking)

* * *

### 🛠️ Wappalyzer

[Wappalyzer](https://www.wappalyzer.com/) detects technologies powering websites — frameworks, CMS, payment gateways, and version numbers.

* * *

### 🕰️ Wayback Machine

[Wayback Machine](https://archive.org/web/) archives historical snapshots of websites, helping to uncover old pages still accessible.

* * *

### 💻 GitHub

Searching GitHub for company or website names can expose:

- Source code
- Passwords or API keys
- Configuration files

* * *

### ☁️ AWS S3 Buckets

Amazon S3 buckets store cloud files, sometimes misconfigured as publicly accessible.

Common bucket URLs look like:

`http(s)://{name}.s3.amazonaws.com`

Example: `tryhackme-assets.s3.amazonaws.com`

You can find bucket names via page source, GitHub, or automated tools.

* * *

## 🤖 Automated Content Discovery

Automated discovery sends thousands or millions of requests to find hidden files/directories by checking common names from **wordlists**.

### What are wordlists?

Text files with lists of common directory or file names — e.g., `admin`, `backup`, `login`, etc.

TryHackMe AttackBox has excellent pre-installed lists curated by Daniel Miessler:

`/usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt`

* * *

### Automation Tools on AttackBox

| Tool | Command Example |
| --- | --- |
| **ffuf** | `ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://10.10.72.194/FUZZ` |
| **dirb** | `dirb http://10.10.72.194/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt` |
| **gobuster** | `gobuster dir --url http://10.10.72.194/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt` |



These tools probe the web server for directories/files that exist, helping discover hidden or forgotten content.

* * *

## 🎯 Summary & Recommendations

- Manual inspection (robots.txt, sitemap.xml, favicons, HTTP headers) provides initial clues and paths.
- OSINT tools enhance the discovery using external data sources.
- Automated tools scale content discovery with efficiency and accuracy.

Understanding and combining these approaches is essential for thorough web application penetration testing.

* * *

## 🌟 Final Note

Content discovery is a foundational skill in web app security testing. Mastering these techniques strengthens your ability to find hidden vulnerabilities and secure applications effectively!

* * *

# 🛡️ Keep Learning & Hacking Ethically! 🚀