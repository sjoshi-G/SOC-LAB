# SOC Attack & Investigation Lab  
# Attacker: 192.168.35.4 → Victim: 192.168.35.8

This repository documents a complete, end‑to‑end SOC investigation performed in a controlled virtual lab environment.  
It includes:

- Network setup  
- Reconnaissance  
- SSH brute‑force attempts  
- HTTP interaction  
- Packet captures (PCAPs)  
- Wireshark analysis  
- Full investigation report  

The goal of this project is to simulate attacker behavior, capture evidence on the victim machine, and analyze the traffic like a SOC analyst.


# Repository Structure

My repository structure
SOC-Lab-Workbook/
│
├── README.md
├── investigation_report.md
│
├── pcaps/
│   ├── full_capture.pcap
│   ├── timed_capture.pcap
│   ├── http_capture.pcap
│   ├── ssh_bruteforce.pcap
│   └── dns_capture.pcap
│
└── images/
    ├── attacker_networkInterface.png
    ├── victim_networkInterface.png
    ├── ping_victim.png
    ├── ping_attacker.png
    ├── nmap_scan.png
    ├── http_capture.png
    ├── ssh_bruteforce.png
    └── manualScript.png


# Lab Overview


# Attacker Machine
- IP: 192.168.35.4
- Tools used:
  - `nmap`
  - `sshpass`
  - `curl`

# Victim Machine
- IP: 192.168.35.8
- Services:
  - SSH (OpenSSH 9.6p1)
  - Python HTTP server (port 8080)
- Tools used:
  - `tcpdump`
  - Wireshark

# Network
- Host‑Only network: 192.168.35.0/24
- NAT for internet access

# Evidence Collected

# 1. Network Verification
- Attacker and victim IP addresses  
- Ping tests (bidirectional connectivity)

# 2. Reconnaissance
- Nmap scan showing:
  - Port 22 open  
  - SSH service version  
  - OS fingerprint  

# 3. SSH Brute‑Force
- Automated password attempts using `sshpass`
- Wireshark capture showing:
  - SSH handshake  
  - Encrypted authentication attempts  
  - Attacker ↔ victim traffic on port 22  

# 4.HTTP Interaction
- Victim running Python HTTP server  
- Attacker sending HTTP GET request  
- Wireshark capture showing:
  - TCP handshake  
  - HTTP GET  
  - HTTP 200 OK response  

# Full Network Capture
- ARP  
- mDNS  
- SSH  
- TCP  
- Background Host‑Only traffic  

# Investigation Report

A full SOC‑style investigation report is included:

It contains:

- Executive summary  
- Timeline  
- Recon analysis  
- Brute‑force analysis  
- HTTP analysis  
- Indicators of attack  
- Recommendations  
- Conclusion  




# Skills Demonstrated

- Packet capture using `tcpdump`
- Network analysis using Wireshark
- Reconnaissance detection
- SSH brute‑force detection
- HTTP traffic inspection
- Incident documentation
- SOC investigation workflow


# Purpose of This Project

This lab demonstrates practical blue-team skills:

- Detecting attacker behavior  
- Capturing evidence  
- Analyzing network traffic  
- Writing SOC investigation reports  
- Understanding attacker → victim flow  

It is designed to showcase hands-on cybersecurity ability for:

- SOC Analyst roles  
- Cybersecurity internships  
- Blue-team positions  
- Digital forensics roles  


# Future Improvements

- Add Suricata or Zeek IDS alerts  
- Add Wazuh or ELK SIEM correlation  
- Add Sysmon logs  
- Add MITRE ATT&CK mapping  
- Add reverse shell capture  
- Add exfiltration simulation  

