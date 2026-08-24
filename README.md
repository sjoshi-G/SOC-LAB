# SOC-LAB
# Overview:
A fully isolated SOC-style home lab built using two Tsurugi Linux VMs, designed to simulate attacker behavior, capture network traffic, and perform packet analysis using Wireshark, tcpdump, tshark, and tcpflow.
This project demonstrates:
Network segmentation
Attack simulation
Packet capture
SOC investigation workflow
Forensic analysis
Documentation and reporting

# Lab Architecture:
Attacker (Tsurugi)  →  Victim (Tsurugi)
        │ Host-Only Network
        └───────────────┐
                        Wireshark Monitoring
4. Network Configuration
Victim VM
NAT: 10.0.2.11/24
Host‑Only: 192.168.35.8

Attacker VM
NAT: 192.168.35.4
Host-Only: 10.0.2.5/24

5. Tools Used
On Both VMs
Tsurugi Linux
Wireshark
tcpdump
tshark
tcpflow
nmap
hydra
netcat
connaissance
Nmap scanning
Service enumeration
OS fingerprinting

✔ Brute Force Attack
Hydra SSH brute force
auth.log correlation

✔ Reverse Shell
netcat listener
attacker shell connection

✔ HTTP Traffic
Python web server
curl requests

✔ Packet Capture
Wireshark live capture
tcpdump timed capture
protocol filtering

✔ SOC Analysis
DNS queries
TCP handshake
ARP discovery
SSH brute force patterns
HTTP GET/response inspection

# Folder Structure
Code
SOC-Lab-Workbook/
│
├── README.md
├── workbook/
│   ├── SOC-Lab-Workbook.md
│   ├── images/
│   │   ├── wireshark_dns.png
│   │   ├── tcp_handshake.png
│   │   └── ssh_bruteforce.png
│   └── investigations/
│       ├── recon_scan.md
│       ├── brute_force.md
│       └── http_analysis.md
│
└── pcaps/
    ├── attack_capture.pcap
    ├── dns_capture.pcap
    └── ssh_capture.pcap
# How to Run the Lab
Step 1 — Configure Network
Adapter 1: NAT
Adapter 2: Host‑Only

Step 2 — Start Victim Services
bash
sudo systemctl start ssh
python3 -m http.server 8080

Step 3 — Capture Traffic
bash
sudo tcpdump -i enp0s8 -w capture.pcap

Step 4 — Launch Attacks
bash
nmap -sV 192.168.35.4
hydra -l root -P rockyou.txt ssh://192.168.35.4
curl http://192.168.35.4:8080
# Analyze in Wireshark
Use filters:
Code
dns
icmp
tcp.flags.syn==1
ssh
http
arp
# Investigation Reports
Each attack scenario includes:
Summary
Evidence
Packet analysis
Timeline
Findings
Recommendations
Reports are stored in:
Code
workbook/investigations/

# Purpose of This Project
This SOC lab demonstrates hands-on skills in:
Network security
Packet analysis
Threat detection
Incident response
Documentation
Forensics
curl

Python3 HTTP server
