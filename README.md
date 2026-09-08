## 👤 Author
**Clement Modebe**  
Advanced Cybercrime Investigation  
International Cybersecurity and Digital Forensics Academy

---

# ICDFA Digital Forensics Labs

This repository contains hands‑on laboratory work completed as part of the  
**Advanced Cybercrime Investigation** program at the  
**International Cybersecurity and Digital Forensics Academy (ICDFA)**.

The labs focus on practical digital forensics fundamentals, low‑level data analysis,
disk and partition interpretation, Linux command‑line usage, and evidence integrity.

This repository is structured to reflect how real digital forensics lab work is
documented, preserved, and reviewed.

---

## 📘 Repository Contents

### ACI801 Digital Evidence Collection

### Lab 01 – Number Systems and Digital Forensics Fundamentals
This lab introduces how computers represent and store data, and why understanding
number systems is essential for forensic investigations.

**Topics covered:**
- Binary, Decimal, and Hexadecimal conversions
- ASCII and text encoding
- Unix epoch time and timestamps
- Endianness and memory representation
- Hashing and evidence integrity

📄 **Full Step‑by‑Step Documentation:**  
[Documentation.md](./Lab01_NumberSystems/Lab01_NumberSystems-Solution-Clement-Documentation.md)

📑 **Formal Lab Report (with screenshots):**  
Lab01_NumberSystems_Report_WithScreenshots.docx

---

### Lab 02 – PC Introduction and Disk Analysis
This lab focuses on PC architecture, disk structures, partition tables, and manual
MBR analysis using hex‑level offsets.

**Topics covered:**
- Computer system layers
- HDD mechanics and disk geometry (CHS)
- PC boot process (BIOS vs UEFI)
- Manual MBR partition analysis
- Hex → decimal → GB size calculations
- File system identification

📄 **Full Step‑by‑Step Documentation:**  
[Documentation.md](./Lab02_PC_Intro/Lab02_PC_Introduction_Solution-Clement-Documentation.md)

📑 **Formal Lab Report:**  
Lab02_PC_Introduction_Report_WithScreenshots.docx

---

### Lab 03 – Linux Command Line Mastery for Digital Forensics
This lab emphasizes Linux command‑line skills required for forensic analysis and
remote evidence acquisition.

**Topics covered:**
- Linux navigation and file management
- Permissions and inodes
- Searching with grep
- Networking and file transfer
- Standard input/output redirection
- Netcat for remote evidence acquisition

📄 **Full Step‑by‑Step Documentation:**  
[Documentation.md](./Lab03_Linux_CommandLine/Lab03_LinuxCommandLine-Solution-Clement-Documentation.md)

📑 **Formal Lab Report:**  
Lab3_LinuxCommandLine_Report_with_Screenshots.docx

---

## 🧪 Tools and Environment
- Oracle VirtualBox
- Kali Linux (Forensics VM)
- Linux CLI utilities
- Python 3
- WinHex (used in disk and partition analysis)

---

## 🎯 Learning Outcomes
- Understand how raw data is represented and interpreted in forensic tools
- Perform manual and automated number system conversions
- Interpret timestamps correctly using the proper epoch
- Analyze disk structures using hex‑level offsets
- Verify evidence integrity using cryptographic hashes
- Document forensic procedures in a professional, reproducible manner

---

### Lab 04 – Autopsy & Sleuth Kit (TSK) Disk Image Analysis
This lab focuses on examining a forensic disk image using Autopsy and Sleuth Kit
to validate evidence integrity, locate and recover deleted files, perform keyword
searches, and generate a forensic report.

**Topics covered:**
- Digital investigation workflow (acquire image, examine, analyze, report)
- Hash verification to confirm evidence integrity
- Creating an Autopsy case and adding a disk image
- Identifying and recovering deleted files
- Tagging artifacts for reporting
- Keyword searching and reviewing results
- Generating an Autopsy report
- Basic TSK analysis and recovery commands (mmls, fsstat, fls, istat, icat, blkls, blkcat)
- Read-only image handling for safe inspection

📄 **Full Step‑by‑Step Documentation:**  
[documentation.md](./Lab04_Evidence_Acquisition/Lab04_From_Evidence_Acquisition_to_Command-Line-Solution-Clement-Documentation.md)

📑 **Formal Lab Report:**  
Lab 4 – From_Evidence_Acquisition_to_Command-line_with_screenshots

---

## 🧪 Tools and Environment
- Autopsy
- The Sleuth Kit (TSK)
- Forensic disk image file (DD image)
- Hashing utilities (for integrity verification)
- Linux environment/terminal tools for analysis and recovery

---

## 🎯 Learning Outcomes
- Explain the stages of a digital forensic investigation from acquisition to reporting
- Validate evidence integrity using cryptographic hashes
- Use Autopsy to find deleted files, tag artifacts, and generate a report
- Use TSK commands to examine partitions/file systems and recover files from images
- Document findings clearly with screenshots and reproducible steps

---

### ✅ Lab 05 – USB Image Acquisition (Windows, Linux, and Network)
This lab focuses on creating a forensic disk image of a USB drive using Windows
and Linux methods, verifying integrity with hashes, and practicing image transfer
over a network.

**Topics covered:**
- Understanding what a disk image is and what it contains (contents + structure)
- USB imaging in Windows using FTK Imager
- USB imaging in Linux using dd
- Handling acquisition errors using dd options (e.g., continue on read errors)
- Network-based acquisition/transfer using nc (Netcat)
- Integrity verification using hash values before and after acquisition

📄 **Full Step‑by‑Step Documentation:**  
[documentation.md](./Lab05_Forensic_USB_Image/Lab05_USB_ImageSolution-Clement-Documentation.md)

📑 **Formal Lab Report:**  
Lab05_USB_Image_Acquisition_Report_with_Screenshots_ClementModebe

---

## 🧪 Tools and Environment
- USB flash drive (test media)
- Windows host system (for FTK Imager acquisition)
- Kali Linux / Linux terminal (for dd imaging)
- VirtualBox (if using a VM with USB passthrough)
- Hashing utilities (for integrity verification)
- Netcat (nc) for network transfer during acquisition

---

## 🎯 Learning Outcomes
- Explain what a disk image is and why forensic imaging is required
- Acquire a USB image using Windows tools and Linux command-line methods
- Apply correct dd imaging procedures and safe handling practices
- Verify evidence integrity by calculating and comparing hash values
- Perform basic network-based acquisition/transfer while maintaining integrity
- Document acquisition steps clearly with screenshots and reproducible commands

---

## 📌 Disclaimer
This repository is intended for **educational and academic purposes only**.
All labs were performed in controlled environments using test data and virtual machines.

---

