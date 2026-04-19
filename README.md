# Cybersecurity Lab – Offensive & Defensive Security Simulation

## Overview
This project is a hands-on **cybersecurity lab simulation** where I built a controlled virtual environment to perform real-world security testing techniques.

The project covers both:
-  **Offensive Security (Attacks)**
-  **Defensive Security (Hardening & Protection)**

Using tools like Kali Linux, Metasploit, Nmap, SQLmap, Hydra, and Bettercap, I simulated attacks on vulnerable machines and then applied security measures to mitigate them.

---

##  Key Objectives

- Understand how attackers discover and exploit systems  
- Perform real-world attacks in a safe lab environment  
- Analyze vulnerabilities and system weaknesses  
- Apply defensive techniques to secure systems  
- Verify security improvements using testing tools  

---

## Lab Environment

The lab was built using virtual machines connected in the same network:

- **Kali Linux** → Attacker machine  
- **Metasploitable2** → Vulnerable target  
- **Ubuntu** → Target for SSH attacks & hardening  
- **Windows 7** → Target for EternalBlue exploit  

All machines were configured on a virtual network with assigned IPs and tested for connectivity.

---

##  Project Workflow

### 1.  Network Discovery
- Used **Nmap** to scan the network and identify active hosts  
- Detected IP addresses and operating systems  
- Enumerated open ports and running services  

> As shown in the project, multiple machines (Ubuntu, Windows, Metasploitable) were discovered and analyzed.

---

### 2.  Man-in-the-Middle Attack (MITM)
- Used **Bettercap** to perform ARP spoofing  
- Intercepted traffic between victim and gateway  
- Enabled:
  - `arp.spoof`
  - `net.probe`
  - `net.sniff`
  - `http.proxy`

> Demonstrated how attackers can monitor and manipulate network traffic.

---

### 3.  SQL Injection Attack (SQLmap)
- Targeted **DVWA (Damn Vulnerable Web App)**  
- Exploited input fields using SQL injection (`1 OR 1=1`)  
- Used **SQLmap** to:
  - Extract database structure  
  - Dump user credentials  
  - Retrieve sensitive data  

> Successfully accessed full database including usernames and passwords.

---

### 4.  Brute Force Attack (Hydra)
- Targeted Ubuntu SSH service (port 22)  
- Generated password lists using **Crunch**  
- Used **Hydra** to perform brute-force login  

> Successfully cracked credentials and gained SSH access.

---

### 5.  System Hardening (Defense Phase)

####  Firewall Configuration (UFW)
- Installed and enabled **UFW firewall** on Ubuntu  
- Applied rules to:
  - Deny incoming connections  
  - Allow controlled access  

####  SSH Protection
- Applied rate limiting on port 22  
- Prevented brute-force attacks  

> After applying security measures, Hydra attack failed due to firewall restrictions.

---

### 6.  Securing Services (Metasploitable)
- Disabled vulnerable ports (21, 23, 3306)  
- Verified protection using Nmap  

> Ports changed from **open → filtered**, confirming firewall effectiveness.

---

### 7.  Exploitation (EternalBlue – CVE-2017-0144)
- Targeted **Windows 7 machine**  
- Used **Metasploit Framework**  
- Exploited SMB vulnerability (port 445)  

> Successfully gained **Meterpreter session with SYSTEM privileges**, demonstrating full system compromise.

---

## Tools & Technologies

- **Kali Linux**
- **Nmap**
- **Bettercap**
- **SQLmap**
- **Hydra**
- **Metasploit Framework**
- **DVWA**
- **UFW Firewall**
- **VirtualBox / VM Environment**


---

##  Project Documentation

Full step-by-step documentation with screenshots is available here:

 [View Full Project Report](CyberSecurity%20Project.pdf)

---

##  What I Learned

- Practical experience with real cybersecurity tools  
- How attackers exploit vulnerabilities  
- Importance of proper network configuration  
- How firewalls and security rules mitigate attacks  
- Hands-on exploitation using Metasploit  
- Defense strategies against brute-force and network attacks  

---

##  Future Improvements

- Automate attacks and defenses using scripts  
- Expand to cloud-based environments  
- Add IDS/IPS systems (Snort / Suricata)  
- Improve logging and monitoring  
- Simulate more advanced attack scenarios  

---

##  Disclaimer

This project was conducted in a **controlled lab environment for educational purposes only**.  
No real systems or networks were harmed.

---

##  Author

**Laila Tarek**  
GitHub: https://github.com/lailaaa337