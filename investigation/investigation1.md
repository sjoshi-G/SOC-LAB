# SOC Investigation Report — Attacker 192.168.35.4 → Victim 192.168.35.8

# 1. Executive Summary
This investigation analyzes simulated malicious activity performed in a controlled SOC lab environment. The attacker system (192.168.35.4) executed reconnaissance, SSH brute‑force attempts, and HTTP interactions against the victim system (192.168.35.8). Packet captures collected on the victim’s Host‑Only interface (enp0s8) were analyzed using Wireshark to identify attack patterns, protocol behavior, and indicators of compromise.

The activity was fully contained within an isolated virtual network and used for training and documentation purposes.
# 2. Environment Overview

# Attacker
- IP: 192.168.35.4
- Role: Threat simulator  
- Tools used: `nmap`, `sshpass`, `curl`

# Victim
- IP: 192.168.35.8
- Role: Monitored endpoint  
- Services:
  - SSH (OpenSSH 9.6p1)
  - Python HTTP server (port 8080)
- Packet capture tools: `tcpdump`, Wireshark

# Network
- Host‑Only network: 192.168.35.0/24
- NAT network for internet access

# 3. Attack Timeline

| Time | Event | Evidence |
|------|--------|----------|
| T1 | Connectivity verified | Ping screenshots (victim ↔ attacker) |
| T2 | Reconnaissance scan | Nmap scan showing port 22 open |
| T3 | SSH brute‑force attempts | Terminal output + SSH PCAP |
| T4 | HTTP server interaction | curl request + HTTP PCAP |
| T5 | Full network capture | timed_capture.pcap + full_capture.pcap |


# 4. Reconnaissance Analysis (Nmap)

# Activity
Attacker executed:
nmap -sV 192.168.35.8


# Findings
- Host responded normally (latency ~0.011s)
- Port 22/tcp OPEN
- Service: OpenSSH 9.6p1 Ubuntu
- OS fingerprint: Linux kernel

# SOC Interpretation
This is typical pre‑attack reconnaissance. The attacker successfully enumerated an exposed SSH service, which later became the target of brute‑force attempts.

# 5. SSH Brute‑Force Analysis

# Activity
Attacker executed a password‑guessing loop:

for pw in wrong1 wrong2 wrong3; do
    sshpass -p "$pw" ssh -o StrictHostKeyChecking=no -o ConnectTimeout=5 user@192.168.35.8 exit
done


# Terminal Evidence
- Multiple “Permission denied” messages  
- Host key added  
- No successful authentication

# Packet Capture Evidence
Wireshark filtered with:

tcp.port == 22

Shows:

- 192.168.35.4 → 192.168.35.8 (attacker → victim)
- SSH handshake packets
- Encrypted SSHv2 payloads
- Multiple repeated attempts
- TCP ACK, SYN, FIN sequences

# SOC Interpretation
This is a clear brute‑force pattern:
- Repeated SSH sessions
- Short connection timeouts
- No successful login
- Encrypted payloads consistent with SSH authentication attempts

This matches real-world brute‑force behavior seen in SIEM logs and IDS alerts.

# 6. HTTP Traffic Analysis

# Activity
Victim started:
python3 -m http.server 8080

Attacker executed:

curl http://192.168.35.8:8080

# Packet Capture Evidence
Wireshark shows:

- TCP handshake (SYN → SYN/ACK → ACK)
- HTTP GET request from attacker
- HTTP 200 OK response from victim
- Payload containing directory listing

# SOC Interpretation
This demonstrates:
- Successful attacker → victim communication
- Clear application-layer traffic
- No anomalies or malicious payloads
- Useful for understanding normal vs abnormal HTTP flows


# 7. Full Network Capture Analysis

# Files:
- full_capture.pcap
- timed_capture.pcap

# Observed Traffic
- ARP broadcasts  
- mDNS multicast  
- SSH encrypted packets  
- TCP ACK sequences  
- Background Host‑Only chatter  

# SOC Interpretation
Full captures provide baseline network behavior:
- Normal ARP resolution  
- Normal multicast service discovery  
- Background SSH traffic  
- Helps differentiate legitimate traffic from attacker activity



# 8. Indicators of Attack

# Recon Indicators
- External host probing port 22
- Service version enumeration

# Brute‑Force Indicators
- Multiple failed SSH authentication attempts
- Repeated short-lived SSH sessions
- Consistent source IP (192.168.35.4)

# HTTP Indicators
- Direct HTTP GET requests from attacker
- Interaction with victim’s Python server


# 9. Recommendations

# 1. Harden SSH
- Disable password authentication  
- Enforce key-based login  
- Enable fail2ban  
- Change default SSH port  
- Limit SSH to trusted IP ranges  

# 2. Monitor Host‑Only Network
- Deploy IDS (Suricata / Zeek)  
- Enable Sysmon or auditd logging  
- Forward logs to SIEM (Splunk / ELK / Wazuh)

# 3. Reduce Attack Surface
- Stop unnecessary services  
- Use firewall rules:
  
  sudo ufw allow from 192.168.35.4 to any port 22
  sudo ufw deny 22/tcp
  

# 4. Improve Alerting
- Trigger alerts on:
  - Multiple failed SSH logins  
  - Nmap scan signatures  
  - Unusual HTTP requests  



# 10. Conclusion
The investigation confirms that the attacker (192.168.35.4) successfully performed reconnaissance, brute‑force attempts, and HTTP interactions against the victim (192.168.35.8). All activity was captured, analyzed, and documented using industry-standard tools (tcpdump, Wireshark, Nmap, sshpass).




Just tell me what you want ne
