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
```
2. Service Discovery & Port Probing
Ran full and SYN stealth scans (-sS) to map active services:

Bash
# Basic Nmap Scan
nmap scanme.nmap.org

# SYN Stealth Scan for Specific Ports
sudo nmap -p 22,113,139 -sS scanme.nmap.org
Discovered Services:

22/tcp - Open (SSH)

80/tcp - Open (HTTP)

9929/tcp - Open (nping-echo)

31337/tcp - Open (Elite)

113/tcp & 139/tcp - Filtered (Ident & NetBIOS-SSN protected by firewall rules).

3. Packet Trace & Diagnostic Analysis
Traced raw probe packets sent during ping and port scanning on google.com:

Bash
nmap -d -packet-trace -p 22,80,443 google.com
Observed Mechanism: Examined outgoing ICMP Echo requests, TCP SYN/ACK probes, and incoming TCP RST packets sent by remote servers to confirm host availability.

Part 3: Deep Packet Inspection & Traffic Analysis (Wireshark)
1. Frame Analysis (TLSv1.3 Session)
Inspected frame details for an encrypted session over HTTPS:

Captured Packet: No. 415 (TLSv1.3 Protocol)

Source Address: 2001:162a:2:c376:32a2

Destination Address: 2603:1063:1b:108::3

Payload Length: 64,185 Bytes

TCP Control Flags: [ACK] with Seq=3565, Ack=4194304 confirming established payload delivery.

2. Local Protocol Behavior Inspection
ARP Resolution: Identified ARP broadcast requests (Who has 192.168.8.19? Tell 192.168.8.1) resolving MAC addresses for local subnet routing.

TCP Handshake & Keep-Alive: Observed 3-way handshake initiation, sequence/acknowledgment progression, and keep-alive control packets maintaining session state.

Hex & Bit View: Analyzed raw Ethernet II headers, IPv4 packet fields, and hex payload dumps.

### Key Takeaways & Security Implications
Service Fingerprinting: Version detection is crucial for identifying legacy software (e.g., Apache 2.4.7) that may require patching or virtual patching via WAF.

Stealth Scanning Utility: SYN scanning (-sS) minimizes full TCP connections, providing stealth during authorization-approved security evaluations.

Traffic Visibility: Wireshark deep packet inspection provides critical insights into encrypted versus unencrypted protocol usage, essential for network anomaly detection and Incident Response (IR).
