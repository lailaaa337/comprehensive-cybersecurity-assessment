
# comprehensive-cybersecurity-assessment

Cybersecurity toolkit for vulnerability analysis, network scanning, and reporting. Includes scripts, tools, and guides for hands-on learning and penetration testing.

## Overview

This repository provides a collection of scripts, tooling, and documentation for practical cybersecurity assessments. It is intended for students, security practitioners, and educators who want hands-on experience with vulnerability discovery, network scanning, and concise reporting.

## Features

- Tools and scripts for vulnerability discovery and verification
- Network scanning and reconnaissance helpers
- Basic reporting templates and examples
- Guidance for hands-on learning and lab exercises

## Getting Started

### Prerequisites

- Git for cloning the repository
- Tool-specific requirements (see each tool's docs). Install dependencies listed in each folder before running scripts.

### Installation

1. Clone the repository:

```bash
git clone <your-repo-url>
cd comprehensive-cybersecurity-assessment
```

2. Inspect the repository structure and read any per-tool README files for usage and requirements.

## Usage

- Follow the README or usage notes in each tool folder. Example commands vary by script (Python, Bash, PowerShell, etc.).
- Always run tools in a controlled, authorized environment. Do not run scanning or exploitation tools against systems you do not own or have explicit permission to test.

## Contributing

Contributions are welcome. Please:

1. Open an issue to discuss major changes.
2. Create a fork and a feature branch for code contributions.
3. Submit a pull request with clear descriptions and tests/examples where applicable.

## Responsible Disclosure

If you discover a critical vulnerability in this repository or any referenced tooling, please contact the maintainers privately and avoid public disclosure until it's resolved.

## License

Include your preferred license file (e.g., MIT, Apache-2.0) in the repository root. If no license is present, add one before sharing or distributing the code.

## Contact

For questions or help, open an issue in this repository.

---

## Detailed Project Log (from `CyberSecurity Project.pdf`)

This section explains, in detail, the hands-on steps performed and recorded in the project PDF. The work was carried out in a controlled lab using multiple virtual machines (Kali Linux, Metasploitable2, Ubuntu, and Windows) on the same virtual network.

1) Network setup and discovery

- Environment: VMs were configured to use the same virtual network (initially NAT with DHCP, later host-only in some steps). Example IPs observed during the lab: Kali `10.0.2.15` (or later `10.0.2.100`), Metasploitable `10.0.2.5`, Ubuntu `10.0.2.6` (later `10.0.2.103`), Windows `10.0.2.4`.
- Discovery: From Kali, Nmap was used to discover hosts and services.

	Example commands:

	```bash
	# discover live hosts
	nmap -sn 10.0.2.0/24

	# perform OS detection and port scan
	nmap -O 10.0.2.5
	```

- Outcome: Hosts were enumerated and services/ports identified. Metasploitable exposed many TCP services (ftp, ssh, smtp, mysql), while other VMs initially showed few open services.

2) Man-in-the-middle (MITM) using Bettercap

- Goal: Intercept and inspect HTTP traffic to the DVWA instance hosted on Metasploitable.
- Tools/commands used on Kali:

	```bash
	sudo bettercap -iface eth0
	# inside bettercap
	net.probe on
	net.show
	set arp.spoof.targets 10.0.2.5
	arp.spoof on
	proxy on
	```

- Actions: After spoofing the ARP entries for the target and gateway, traffic was proxied and inspected. Submitted form data on DVWA (e.g., creating a test user) was observed in the proxy stream.

- Outcome: Successful ARP spoofing, traffic interception (stream/proxy/sniff) and observation of session data.

3) Database attack (SQL injection) against DVWA

- Setup: DVWA on Metasploitable was set to `low` security to permit SQLi testing.
- Approach: Injecting typical payloads like `1 OR 1=1` to verify SQL injection vulnerability and extract data. Session hijacking via stolen `PHPSESSID` cookie was also demonstrated to impersonate sessions.

	Example (conceptual):

	```bash
	# using sqlmap (conceptual example)
	sqlmap -u "http://10.0.2.5/dvwa/vulnerabilities/sqli/?id=1" --cookie="PHPSESSID=<session>" --dump
	```

- Outcome: Database records (including credential data) were enumerated, demonstrating the impact of SQL injection when input validation is poor.

4) SSH brute-force (Hydra) and SSH hardening on Ubuntu

- Network adjustment: The lab network was reconfigured (host-only + NAT adapter) to ensure SSH reachability. SSH server was installed and enabled on the Ubuntu VM.
- Attack: `hydra` was used from Kali to brute-force SSH using a wordlist. A smaller, tailored `fastlist.txt` improved efficiency when generic lists proved too large.

	Example commands:

	```bash
	# run hydra against SSH
	hydra -l root -P fastlist.txt ssh://10.0.2.103
	```

- Hardening: `ufw` (Uncomplicated Firewall) was installed and configured on Ubuntu to deny incoming connections by default and enable rate-limiting on SSH.

	Example hardening steps:

	```bash
	sudo apt update && sudo apt install ufw
	sudo ufw default deny incoming
	sudo ufw default allow outgoing
	sudo ufw allow OpenSSH
	sudo ufw limit ssh
	sudo ufw enable
	```

- Verification: After enabling UFW and limiting SSH, repeated brute-force attempts were rate-limited and failed. Authentication failures were visible in `/var/log/auth.log`, confirming the firewall mitigated the attack.

5) Securing Metasploitable services and verification

- Actions: Firewall rules were applied on the Metasploitable VM to deny access to critical services (example ports: `21`, `23`, `3306`).

	Example (conceptual):

	```bash
	# Install/enable firewall (if supported on the image)
	sudo ufw deny 21
	sudo ufw deny 23
	sudo ufw deny 3306
	sudo ufw enable
	```

- Verification: A follow-up Nmap scan from Kali showed the previously open ports as filtered/denied, indicating the firewall rules were effective.

6) CVE-2017-0144 (EternalBlue)

- The PDF mentions applying CVE-2017-0144 (EternalBlue) as a test. Note: exploitation of real CVEs against systems you do not own or have explicit permission to test is illegal and unethical. If you documented an exploit in the lab, ensure it was run in an isolated environment with explicit permission.

General notes and lessons

- Always document exact commands, timestamps, and system snapshots when performing security testing.
- Use minimal-scope wordlists and limit attack speed in tests to reduce noise and avoid unintended denial-of-service on lab systems.
- After testing, apply security controls (firewalls, patched services, strong credentials) and re-scan to validate mitigations.

Repository contents

- `CyberSecurity Project.pdf` — Original lab report (source for this README).
- `README.md` — This file.

Responsible use

This repository documents penetration testing techniques for educational purposes. Do not use these techniques against systems without explicit authorization. Always follow legal and ethical guidelines.



