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
- **Networking:** Both VMs configured on VirtualBox's Bridged Adapter mode, 
  allowing direct communication over the local network

## Work Completed

**1. Lab Environment Setup**
Installed and configured both VMs, set up networking between them, and 
verified connectivity via a successful ping test (0% packet loss).

**2. Reconnaissance**
Ran an Nmap scan against the target to enumerate open ports and running 
services. Identified 23 open ports, including FTP, SSH, Telnet, HTTP, and 
MySQL. Researched the purpose and security implications of each service, 
since raw port numbers are meaningless without understanding what's running 
behind them.

**3. Exploitation**
Identified that the target was running vsftpd 2.3.4, a version containing a 
publicly documented backdoor vulnerability (CVE-2011-2523) introduced after 
its official source was compromised in 2011. Used the Metasploit Framework 
to exploit this vulnerability, obtaining a Meterpreter session and confirming 
root-level access on the target via `whoami`.

**4. Detection — in progress**
Currently setting up Splunk to ingest logs from the target machine and build 
detection queries for the reconnaissance and exploitation activity performed 
above.

## Skills Demonstrated

- VM provisioning and network configuration
- Port and service enumeration using Nmap
- Vulnerability identification and exploitation using Metasploit
- Practical understanding of CVEs and real-world exploit mechanics
- (In progress) SIEM-based log analysis and detection engineering

## Screenshots

Evidence for each step is available in the `/screenshots` folder, including 
Nmap scan output and confirmation of root access post-exploitation.

## Next Steps

- Complete Splunk installation and configure log ingestion from the target
- Build a detection rule/alert for the vsftpd exploitation activity
- Document the complete attack-to-detection pipeline