# SIEM Home Lab

A personal project built to gain hands-on experience with core cybersecurity 
concepts — specifically, understanding the full attack lifecycle from 
reconnaissance and exploitation through to log-based detection.

## Overview

Most learning resources cover either the offensive side (how attacks happen) 
or the defensive side (how they're detected) in isolation. This lab was built 
to understand both halves of that cycle firsthand, in a safe and isolated 
environment.

## Environment

- **Attacker machine:** Kali Linux (VirtualBox VM)
- **Target machine:** Metasploitable2 — an intentionally vulnerable Linux VM 
  designed for security practice
- **SIEM:** Splunk Enterprise (installed on Kali)
- **Networking:** Both VMs configured on VirtualBox's Bridged Adapter mode, 
  allowing direct communication over the local network

## Work Completed

**1. Lab Environment Setup**
Installed and configured both VMs, set up networking between them, and 
verified connectivity via a successful ping test (0% packet loss).

**2. Reconnaissance**
Ran an Nmap scan against the target to enumerate open ports and running 
services. Identified 23 open ports, including FTP, SSH, Telnet, HTTP, and 
MySQL. Researched the purpose and security implications of each service.

**3. Exploitation**
Identified that the target was running vsftpd 2.3.4, a version containing a 
publicly documented backdoor vulnerability (CVE-2011-2523). Used the 
Metasploit Framework to exploit this vulnerability, obtaining a Meterpreter 
session and confirming root-level access on the target via `whoami`.

**4. SIEM Setup and Log Ingestion**
Installed Splunk Enterprise on Kali and configured a UDP data input to 
receive syslog data. Configured Metasploitable's syslog daemon (sysklogd) to 
forward system logs to Splunk. Verified real-time log ingestion, including 
sudo sessions, cron activity, and system events.

**5. Detection Gap Analysis (Key Finding)**
Attempted to locate the vsftpd exploit activity within the forwarded syslog 
data — it was not present. Investigation revealed that vsftpd maintains its 
own dedicated log file (`/var/log/vsftpd.log`), separate from syslog, which 
was never configured for forwarding. This surfaced a real-world visibility 
gap: default OS-level log forwarding does not automatically capture 
application-specific logs, meaning security-relevant events can go 
completely unseen by a SIEM unless explicitly configured. Attempted to 
manually forward this log via netcat; this specific forwarding method did 
not succeed within the scope of this lab, but the underlying gap was clearly 
identified and confirmed via direct log file inspection on the target.

## Skills Demonstrated

- VM provisioning and network configuration
- Port and service enumeration using Nmap
- Vulnerability identification and exploitation using Metasploit
- SIEM installation, configuration, and log ingestion (Splunk)
- Syslog/rsyslog vs sysklogd troubleshooting across different Linux systems
- Identifying real-world log visibility gaps — a core SOC analyst skill

## Screenshots

See the `/screenshots` folder for evidence of each step, including Nmap scan 
results, root access confirmation, the Splunk dashboard, live log ingestion, 
and the vsftpd log showing the exploit connection locally.

## Lessons Learned

The most valuable takeaway from this project wasn't the exploit itself — it 
was discovering that a SIEM is only as good as the logs it's configured to 
receive. Default syslog forwarding covers OS-level activity, but 
application-specific logs (like vsftpd's) require explicit configuration to 
be visible at all. This is a common real-world misconfiguration and a good 
reminder that "detection" isn't automatic just because a SIEM is running.

## Possible Future Improvements

- Properly forward vsftpd's dedicated log file to Splunk (e.g., via a file 
  monitor input instead of netcat)
- Build a Splunk search/alert for suspicious FTP connection patterns
- Extend log forwarding to other Metasploitable services (Apache, MySQL)
