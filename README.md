#  Network Scanning, Enumeration & Packet Analysis Lab

![Nmap](https://img.shields.io/badge/Tools-Nmap-blue)
![Wireshark](https://img.shields.io/badge/Tools-Wireshark-orange)
![Docker](https://img.shields.io/badge/Environment-Docker%20%7C%20Kali%20Linux-black)
![Security](https://img.shields.io/badge/Domain-Network%20Security%20%26%20Traffic%20Analysis-green)

---

##  Executive Summary
This lab demonstrates practical hands-on experience in **Network Reconnaissance, Port Scanning, Enumeration, and Traffic Analysis**. Using a containerized **Kali Linux environment via Docker**, web security scanners (**Pentest Tools**), and **Wireshark**, various network probing techniques were conducted on authorization-granted targets (`scanme.nmap.org` and public domain hosts).

The objective is to analyze host responses, identify open services and potential attack surfaces, and inspect raw packet behavior at the network layer.

---

##  Tools & Technologies Used
* **Operating System / Container:** Kali Linux (Docker Container), Windows 11
* **Reconnaissance & Scanning:** Nmap, Pentest-Tools
* **Packet Inspection & Traffic Analysis:** Wireshark
* **Protocols Analyzed:** TCP, UDP, ICMP, DNS, ARP, TLSv1.3, HTTP/HTTPS

---

##  Lab Execution & Technical Breakdown

### Part 1: Automated Vulnerability & Port Scanning (Pentest-Tools)
* **Target:** `scanme.nmap.org`
* **Configuration:** Light TCP Scan, Top 100 ports, Version-based detection.
* **Key Findings:**
  * **Port 22/TCP (Open):** OpenSSH `6.6.1p1 Ubuntu 2ubuntu2.13` (Remote Management).
  * **Port 80/TCP (Open):** Apache `httpd 2.4.7` (Web Server).
  * **Risk Score:** Overall Risk Level identified as **High** due to outdated service versions exposing potential vulnerabilities.

---

### Part 2: Advanced Nmap Reconnaissance via Dockerized Kali Linux

#### 1. Environment Setup
Built and executed an interactive Kali Linux container inside Docker to perform low-level network commands safely:
```bash
docker build -t kalilinux .
docker run --tty --interactive kalilinux
apt-get install nmap -y
