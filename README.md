# 🔵 Lesson: Detecting Port Scanning with Nmap & Wireshark

---

## 📌 Overview
In this lesson, I simulated a port scan using Nmap from a Kali Linux machine and analysed the traffic using Wireshark on a Windows 10 virtual machine. The aim was to understand how attackers scan for open ports and how this behaviour can be detected at a network level.

---

## 🎯 Objectives
- Understand what port scanning is  
- Learn how SYN packets are used in scanning  
- Capture and analyse scan traffic in Wireshark  
- Identify suspicious behaviour patterns  
- Understand how firewalls affect scan results  

---

## 🖥️ Lab Setup

| Machine | Role | IP Address |
|--------|------|-----------|
| Kali Linux | Attacker | 192.168.56.104 |
| Windows 10 | Target | 192.168.56.103 |

- Network: Host-only Adapter  
- Tools: Nmap, Wireshark  

---

## 🧠 Key Concept: What is Port Scanning?

Port scanning is when an attacker sends packets to multiple ports on a system to check which services are open and potentially vulnerable.

👉 Each port is like a “door”  
👉 Open port = possible entry point  

---

## 🧠 Key Concept: What is SYN?

SYN (Synchronize) is the first step in starting a TCP connection.

### TCP 3-Way Handshake:
1. SYN → “Can I connect?”  
2. SYN-ACK → “Yes, I hear you”  
3. ACK → “Connection established”  

👉 In a SYN scan, the attacker sends only the first step (SYN) and does not complete the connection.

---

## 🧪 Steps Performed

### 1. Start Packet Capture
- Opened Wireshark on Windows  
- Selected active network interface  
- Started capturing traffic  

---

### 2. Run Port Scan from Kali

```
nmap -sS 192.168.56.103
```

👉 This performs a **SYN scan (stealth scan)** by sending SYN packets to many ports.

---

### 3. Stop Capture and Analyse

Applied Wireshark filter:

```
tcp.flags.syn == 1 and tcp.flags.ack == 0
```

👉 This shows only SYN packets (initial connection attempts)

---

## 🔍 Observations

- Kali (192.168.56.104) sent SYN packets to Windows (192.168.56.103)  
- Multiple destination ports were targeted rapidly  
- No full TCP handshake was completed  

👉 This is clear evidence of **port scanning behaviour**

---

## 📊 Nmap Output Explanation

```
All 1000 scanned ports are in ignored states
Not shown: 1000 filtered tcp ports (no-response)
```

### Meaning:
- 1000 ports were scanned  
- No responses were received  
- All ports are **filtered**

👉 “Filtered” means the firewall blocked the scan

---

## 🔁 What Happened Step-by-Step

1. Kali sent SYN packets to multiple ports  
2. Windows firewall blocked responses  
3. Wireshark captured outgoing scan attempts  
4. Nmap reported all ports as filtered  

---

## 🚨 Why This is Suspicious

Normal traffic:
- Few connections  
- Specific ports  
- Completed handshakes  

Port scanning behaviour:
- One IP → many ports  
- Rapid SYN packets  
- No completed connections  

👉 This indicates **reconnaissance activity**

---

## 🛡️ Security Perspective

Port scanning is often the **first stage of an attack**, used to:
- Discover open services  
- Identify vulnerabilities  
- Plan further exploitation  

---

## 🛡️ Defensive Actions

- Monitor for repeated SYN packets across multiple ports  
- Use firewall rules to block or limit scanning  
- Use IDS/IPS or SIEM tools (e.g. Splunk) to detect patterns  
- Investigate unknown or suspicious IP addresses  

---

## 💡 Key Takeaway

This lesson demonstrated how attackers use SYN scans to probe systems and how this activity can be detected using Wireshark. Even when a firewall blocks responses, scanning behaviour can still be identified through packet analysis.

---

## 🧾 One-Line Summary

A SYN scan sends multiple TCP SYN packets to different ports to identify open services, and it can be detected by observing repeated connection attempts without completing the full TCP handshake.

---
