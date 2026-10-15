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
<img width="848" height="404" alt="pentest-risk" src="https://github.com/user-attachments/assets/70d15ab3-d039-43ad-a3d8-50bdd2546848" />
<img width="843" height="403" alt="Screenshot 2026-10-15 141835" src="https://github.com/user-attachments/assets/b2bc4526-f540-4eeb-b6b0-86fd943d4343" />


---

### Part 2: Advanced Nmap Reconnaissance via Dockerized Kali Linux

#### 1. Environment Setup
Built and executed an interactive Kali Linux container inside Docker to perform low-level network commands safely:
```
docker build -t kalilinux .
docker run --tty --interactive kalilinux
apt-get install nmap -y
```
<img width="626" height="311" alt="nmapsetup" src="https://github.com/user-attachments/assets/0b154179-b542-4f27-9f62-aedd9e420c33" />

2. Service Discovery & Port Probing
Ran full and SYN stealth scans (-sS) to map active services:


# Basic Nmap Scan
```
nmap scanme.nmap.org
```
<img width="604" height="417" alt="Screenshot 2026-10-15 142546" src="https://github.com/user-attachments/assets/0b507eb8-54c6-4aaa-92a7-ec39e3e17c79" />

# SYN Stealth Scan for Specific Ports
```
sudo nmap -p 22,113,139 -sS scanme.nmap.org
```
Discovered Services:

22/tcp - Open (SSH)

80/tcp - Open (HTTP)

9929/tcp - Open (nping-echo)

31337/tcp - Open (Elite)

113/tcp & 139/tcp - Filtered (Ident & NetBIOS-SSN protected by firewall rules).
<img width="643" height="224" alt="Screenshot 2026-10-15 142654" src="https://github.com/user-attachments/assets/ec38d05e-a050-45fa-b063-87eac79cb00c" />


3. Packet Trace & Diagnostic Analysis
Traced raw probe packets sent during ping and port scanning on google.com:

```
nmap -d -packet-trace -p 22,80,443 google.com
```
Observed Mechanism: Examined outgoing ICMP Echo requests, TCP SYN/ACK probes, and incoming TCP RST packets sent by remote servers to confirm host availability.

<img width="637" height="453" alt="Screenshot 2026-10-15 143209" src="https://github.com/user-attachments/assets/0509d38c-521e-43ff-bf45-9e8c89e63db5" />

---

Part 3: Deep Packet Inspection & Traffic Analysis (Wireshark)
1. Frame Analysis (TLSv1.3 Session)
Inspected frame details for an encrypted session over HTTPS:

Captured Packet: No. 415 (TLSv1.3 Protocol)

Source Address: 2001:162a:2:c376:32a2

Destination Address: 2603:1063:1b:108::3

Payload Length: 64,185 Bytes

TCP Control Flags: [ACK] with Seq=3565, Ack=4194304 confirming established payload delivery.

<img width="764" height="369" alt="Screenshot 2026-10-15 143926" src="https://github.com/user-attachments/assets/10d5670b-fa16-4764-86d4-5b8e83e8c55d" />


2. Local Protocol Behavior Inspection
ARP Resolution: Identified ARP broadcast requests (Who has 192.168.8.19? Tell 192.168.8.1) resolving MAC addresses for local subnet routing.

TCP Handshake & Keep-Alive: Observed 3-way handshake initiation, sequence/acknowledgment progression, and keep-alive control packets maintaining session state.
<img width="847" height="222" alt="Screenshot 2026-10-15 144115" src="https://github.com/user-attachments/assets/41c09f4b-cbcc-497b-a67a-014f9bc9f9e7" />


Hex & Bit View: Analyzed raw Ethernet II headers, IPv4 packet fields, and hex payload dumps.
<img width="777" height="241" alt="Screenshot 2026-10-15 144251" src="https://github.com/user-attachments/assets/e3b9f58c-e384-4948-b47e-a0985f645423" />


<img width="775" height="177" alt="Screenshot 2026-10-15 144232" src="https://github.com/user-attachments/assets/65bfa4f3-dea8-486a-af62-6b117ad4140b" />

---

### Key Takeaways & Security Implications
Service Fingerprinting: Version detection is crucial for identifying legacy software (e.g., Apache 2.4.7) that may require patching or virtual patching via WAF.

Stealth Scanning Utility: SYN scanning (-sS) minimizes full TCP connections, providing stealth during authorization-approved security evaluations.

Traffic Visibility: Wireshark deep packet inspection provides critical insights into encrypted versus unencrypted protocol usage, essential for network anomaly detection and Incident Response (IR).

Author
Name: Ghida Alnahar
