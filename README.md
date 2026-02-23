# Soc-lab-ftp-bruteforce-detection

This project shows how I simulated an FTP brute force attack in my home lab and analyzed the logs to detect suspicious activity.

---

## Lab Environment

- Kali Linux (attacker)
- Metasploitable 2 (target)
- VirtualBox internal network
- Hydra for brute force
- Log analysis using /var/log/auth.log

---

## Goal

The goal of this lab was to simulate a brute force attack and then analyze authentication logs from a defender’s point of view.

---

## 1. Reconnaissance (Nmap Scan)

First, I scanned the target machine to see which services were running.

Command used:

nmap -sV 192.168.100.21

Result:

The scan showed that FTP (vsftpd 2.3.4) was running on port 21, which became the target for the attack.

---

## 2. Attack Simulation (Hydra FTP Brute Force)

I used Hydra to simulate a brute force attack against the FTP service.

Command used:

hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt ftp://192.168.100.21

Description:

- Username targeted: msfadmin
- Password list: rockyou.txt
- Protocol: FTP

Hydra generated multiple login attempts using a password wordlist. This allowed me to create realistic authentication failure logs for analysis.

---

## 3. Log Analysis (Detection Phase)

After running the attack, I checked the authentication logs on the target system.

Command used:

sudo cat /var/log/auth.log | grep 192.168.100.19

Findings:

- Multiple FAILED LOGIN entries detected
- Repeated authentication failures from same source IP
- High frequency login attempts within short time period

Example pattern:

FAILED LOGIN: Client "192.168.100.19"

This behavior is consistent with brute force activity.

---

## Indicators of Brute Force Attack

- High number of failed login attempts
- Same source IP address
- Repeated targeting of the same username
- Rapid authentication attempts

---

## SOC Perspective

From a defensive monitoring standpoint, this activity would typically trigger:

- SIEM alert for excessive failed logins
- Investigation of suspicious source IP
- Possible temporary IP blocking
- Credential abuse monitoring

---

## Mitigation Recommendations

- Enable rate limiting
- Implement fail2ban
- Enforce strong password policies
- Restrict FTP access
- Disable unnecessary services

---

## Screenshots

- 01-nmap-scan.png
- 02-hydra-ftp-attack.png
- 03-ftp-log-analysis.png

---

## Conclusion

This lab demonstrates practical understanding of service enumeration, brute force attack simulation, log-based detection analysis and SOC-style incident investigation. The entire exercise was performed in a controlled home lab environment strictly for educational purposes.
