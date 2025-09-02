# 🕵️‍♂️ Digital Forensics Report - USB Evidence Analysis

---

## 📁 Case Information

| Field              | Value                          |
|--------------------|-------------------------------|
| **Case Name**      | USB-Investigation2             |
| **Examiner**       | DeepCover                      |
| **Image Format**   | EWF (.E01)                     |
| **Acquisition Tool** | Guymager                   |
| **Analysis Tool**  | Autopsy                        |
| **Date**           | 2025-07-12                     |

---

## 🔍 Objective

To simulate the investigation of a suspicious USB drive image containing:
- A **deleted text file** with sensitive content.
- A **fake image** (misleading file with `.jpg` extension).

---

## 🛠️ Tools Used

| Tool        | Purpose                            |
|-------------|-------------------------------------|
| `dd` / `losetup` | Create and mount raw disk image |
| **Guymager** | Acquire `.E01` image file         |
| **Autopsy**  | Analyze disk image for artifacts   |

---

## 📦 Step-by-Step Process

---

### 🧰 1. Creating a Fake USB Disk

```bash
dd if=/dev/zero of=usb-evidence.raw bs=1M count=50
losetup -fP --show usb-evidence.raw
```
* Partitioned and formatted as FAT32 using parted

* Mounted, and created files:
 ```
echo "Top secret content here" > /mnt/fakeusb/secret.txt
echo "Not really a picture, but good enough!" > /mnt/fakeusb/picture.jpg
```
* 🔴 Deleted secret.txt to simulate file deletion.
## 🧪 2. Creating E01 Image with Guymager
* Used /dev/loopX as source (loopback from raw image)

* Saved image as: /home/kali/dfir-images/usbcase1.E01

* Format: EWF (.E01)

| Field            | Value                                |
| ---------------- | ------------------------------------ |
| **Case Number**  | 2025-USB-01                          |
| **Evidence No.** | 001                                  |
| **Description**  | USB with deleted file and fake image |

![Guy](file:///C:/Users/moham/OneDrive/Desktop/Guy.png)

## 🕸️ 3. Analysis in Autopsy

* Launched via:

```
bashCopyEditautopsy
```

* Loaded `usbcase1.E01` into a case named **USB-Investigation2**
* Selected volume: `Partition 1 (FAT32)`
* Enabled modules:

    * Deleted Files ✅
    * File Type Identification ✅
    * Metadata Extraction ✅
    
![Auto1](file:///C:/Users/moham/OneDrive/Desktop/Auto1.png)

# 🔎 Key Findings
* 📄 secret.txt
* ✅ Status: Deleted

* ✅Recovered content:
```
Top secret content here
```
![Auto2](file:///C:/Users/moham/OneDrive/Desktop/Auto2.png)
## 🔗 Chain of Custody
Step	Responsible	Action	Timestamp
Evidence Created	DeepCover	Created USB image (.raw)	2025-07-12 09:50 UTC
Image Acquired	DeepCover	Created .E01 via Guymager	2025-07-12 10:01 UTC
Image Analyzed	DeepCover	Loaded in Autopsy	2025-07-12 10:10 UTC

# # 📌 Conclusion
This investigation demonstrates the full digital forensic workflow:

* Disk imaging

* Image acquisition in forensically sound format

* Deleted file recovery

* Fake file detection

* Chain of custody documentation

🧠 Useful for portfolio and demonstrating DFIR (Digital Forensics & Incident Response) skills.



🔐 Prepared by: Mohammad.Mohammadi
🕓 Date: July 12, 2025