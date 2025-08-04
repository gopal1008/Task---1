# Cybersecurity Internship - Task 1: Network Port Scanning

## Objective
The objective of this task was to discover open ports on devices in a local network to understand network exposure using Nmap on Kali Linux.

## Tools Used
- **Nmap**: A powerful network scanning tool for discovering hosts and services.
- **Kali Linux**: A Linux distribution designed for cybersecurity tasks.

## Procedure

### 1. Network Interface Configuration
First, the network interfaces of the Kali Linux virtual machine were checked using the `ip` command. The relevant details:
- **IP Address**: `192.168.247.140`
- **Interfaces**:
  - `lo` (Loopback): `127.0.0.1/8`
  - `eth0`: `192.168.247.140/24`
  - `docker0`: `172.17.0.1/16` (currently down)

### 2. Nmap Scans Performed

#### a. Stealth Scan (`-sS`) on Local Machine
- **Command**: `nmap -sS 192.168.247.140`
- **Result**: Only port `22/tcp` (SSH) was found open.

#### b. Stealth Scan (`-sS`) on Entire Subnet
- **Command**: `nmap -sS 192.168.247.0/24`
- **Results**:
  - `192.168.247.1`: Ports `135/tcp` (msrpc), `139/tcp` (netbios-ssn), `445/tcp` (microsoft-ds), `3306/tcp` (mysql) open.
  - `192.168.247.2`: Port `53/tcp` (domain) open.
  - `192.168.247.254`: All ports filtered.
  - `192.168.247.140`: Port `22/tcp` (SSH) open.

#### c. TCP Connect Scan (`-sT`) on Local Machine
- **Command**: `nmap -sT 192.168.247.140`
- **Result**: Only port `22/tcp` (SSH) was found open.

#### d. Comprehensive TCP Scan (All Ports)
- **Command**: `sudo nmap -p- -sS -T4 192.168.247.140`
- **Result**: Ports `22/tcp` (SSH) and `1716/tcp` (XMSG) were found open.

### 3. Open Ports and Their Risks

#### a. SSH (Port 22)
- **Purpose**: Secure remote administration.
- **Risks**:
  - Brute-force attacks targeting weak credentials.
  - Vulnerabilities in outdated SSH versions (e.g., CVE-2023-38408).
  - Privilege escalation if default/weak credentials are used.
  - MITM attacks if encryption is misconfigured.
- **Mitigation**:
  - Use key-based authentication.
  - Change the default port or restrict access to trusted IPs.
  - Enable `fail2ban` to block brute-force attempts.

#### b. XMSG (Port 1716)
- **Purpose**: Typically associated with messaging services or network analysis tools like Xplico.
- **Risks**:
  - Potential backdoor or malware communication channel.
  - Data leakage if the service handles sensitive data without encryption.
  - Exploitable vulnerabilities (e.g., RCE, DoS) in outdated software.
- **Mitigation**:
  - Verify the service running on this port.
  - Disable the service if unnecessary.
  - Monitor for suspicious traffic.

## Conclusion
The scans revealed two open ports on the local machine (`192.168.247.140`):
1. **SSH (Port 22)**: Essential for remote management but requires hardening.
2. **XMSG (Port 1716)**: Non-standard port that needs further investigation to ensure it is not a security risk.

The subnet scan also identified other devices with open ports, highlighting the importance of securing all networked devices to reduce attack surfaces.

## Repository Contents
- `Task 1.pdf`: Detailed scan report and analysis.
- `README.md`: Summary of the task (this file).
