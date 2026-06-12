# DVWA Ubuntu Server Hardening

## Objective

The objective of this project was to build, secure, and document a vulnerable DVWA Ubuntu server in a controlled lab environment. The project focused on applying basic blue-team hardening practices to improve the security posture of the server while maintaining the ability to use DVWA for authorized cybersecurity training.

This project demonstrates hands-on experience with Linux server administration, service review, firewall configuration, SSH hardening, intrusion prevention, file integrity monitoring, vulnerability auditing, log analysis, and security documentation.

---

## Environment

- **Platform:** Proxmox Virtual Environment
- **Primary Server:** Ubuntu DVWA Server
- **Hostname:** UbunServDVWA-TeamA
- **Server IP Address:** 10.10.10.223/16
- **Operating Systems Used:**
  - Ubuntu Server
  - Kali Linux
  - Windows Client
- **Lab Type:** Cybersecurity / SOC Capstone Lab
- **Network Type:** Virtualized Proxmox lab network
- **Primary Role of Server:** Intentionally vulnerable DVWA web application server used for security testing and hardening practice

---

## Tools Used

- Ubuntu Server CLI
- Kali Linux
- DVWA
- Proxmox VE
- UFW
- Fail2Ban
- AIDE
- Lynis
- Chrony
- Nmap
- Hydra
- tcpdump
- Wireshark
- journalctl
- systemctl
- ss
- lsof
- iptables
- SSH
- Apache
- MySQL
- DISA STIG references
- RMF-style documentation
- NIST-aligned security practices

---

## Network Information

| Item | Details |
|---|---|
| Server Name | UbunServDVWA-TeamA |
| Server IP | 10.10.10.223/16 |
| Server Role | DVWA Ubuntu web server |
| Attacker/Test Machine | Kali Linux VM using DHCP |
| Client/Test Machine | Windows VM using DHCP |
| Virtualization Host | Proxmox |
| Lab Purpose | Authorized cybersecurity training and SOC capstone testing |

---

## Tasks Completed

### 1. System Baseline Review

Performed initial system checks to identify the server hostname, IP configuration, running services, open ports, active users, failed services, routing table, and operating system details.

Commands and tools used included:

- `hostnamectl`
- `ip a`
- `ip route`
- `ss -tulnp`
- `lsof -i`
- `systemctl`
- `journalctl`

---

### 2. Package Updates and Maintenance

Updated the Ubuntu server to ensure installed packages were current and unnecessary packages were removed.

Actions completed:

- Updated package lists
- Upgraded installed packages
- Removed unnecessary packages
- Cleaned package cache
- Reviewed available upgrades

---

### 3. User and Account Review

Reviewed local users, login shells, groups, sudo access, and root account status.

Actions completed:

- Reviewed `/etc/passwd`
- Reviewed `/etc/group`
- Checked sudo group membership
- Reviewed password aging information
- Locked the root account to reduce unauthorized administrative access risk

---

### 4. SSH Hardening

Hardened SSH access to reduce the risk of brute-force attacks and unauthorized remote login attempts.

Security controls reviewed or applied:

- Disabled root SSH login
- Reviewed password authentication settings
- Verified public key authentication settings
- Reduced maximum authentication attempts
- Configured idle session timeout settings
- Added an authorized access warning banner
- Tested SSH configuration syntax before restarting the service

---

### 5. Firewall Configuration

Configured UFW to control inbound and outbound traffic.

Actions completed:

- Set default incoming policy to deny
- Set default outgoing policy to allow
- Allowed SSH access
- Reviewed HTTP access requirements for DVWA testing
- Reviewed numbered firewall rules
- Reloaded and verified firewall status

---

### 6. Fail2Ban SSH Protection

Installed and configured Fail2Ban to monitor SSH authentication attempts and block repeated failed login activity.

Actions completed:

- Installed Fail2Ban
- Enabled the service at boot
- Created local jail configuration
- Configured SSH jail parameters
- Reviewed banned IP addresses
- Practiced manual ban and unban commands
- Reviewed Fail2Ban logs

---

### 7. AIDE File Integrity Monitoring

Installed and initialized AIDE to monitor file integrity and detect unauthorized changes to system files.

Actions completed:

- Installed AIDE
- Initialized the AIDE database
- Copied the new database into the active location
- Ran file integrity checks
- Reviewed database update process after approved system changes

---

### 8. Lynis Security Audit

Used Lynis to perform a Linux security audit and identify hardening suggestions.

Actions completed:

- Installed Lynis
- Ran a full system audit
- Reviewed warnings
- Reviewed suggestions
- Reviewed the hardening index
- Used audit results to guide additional security improvements

---

### 9. Chrony Time Synchronization

Configured and reviewed time synchronization using Chrony.

Actions completed:

- Installed Chrony
- Reviewed Chrony service status
- Verified time synchronization
- Reviewed time sources
- Confirmed system time status using `timedatectl`

Accurate time synchronization is important for logging, event correlation, incident response, and forensic review.

---

### 10. Service Reduction

Reviewed and disabled unnecessary services to reduce the attack surface of the server.

Services reviewed included:

- Avahi
- CUPS
- Whoopsie
- LightDM

Unneeded services were disabled where appropriate for a server environment.

---

### 11. Sysctl Network Hardening

Reviewed and applied Linux kernel network hardening settings.

Security settings included:

- Reverse path filtering
- Disabling ICMP redirects
- Disabling send redirects
- Ignoring bogus ICMP error responses
- Enabling TCP SYN cookies

These settings help reduce exposure to spoofing, redirection, and certain network-based attacks.

---

### 12. Log Review and Monitoring

Reviewed system and authentication logs to identify login activity, failed authentication attempts, sudo usage, service issues, and other security-relevant events.

Commands and logs reviewed included:

- `journalctl`
- `/var/log/auth.log`
- SSH failed password attempts
- SSH accepted logins
- sudo activity
- Fail2Ban logs

---

### 13. Network Testing and Traffic Capture

Performed authorized network testing and traffic capture to support blue-team and red-team lab activities.

Tools used included:

- `ping`
- `ip neigh`
- `nmap`
- `tcpdump`
- Wireshark

Traffic capture was used to support investigation, documentation, and SOC-style analysis.

---

### 14. Apache and DVWA Service Checks

Reviewed Apache and DVWA service availability to ensure the web application was operating as expected during authorized lab testing.

Actions completed:

- Checked Apache service status
- Restarted Apache when needed
- Verified listening ports
- Tested local HTTP response
- Reviewed Apache access logs
- Reviewed Apache error logs

---

### 15. MySQL Service Checks

Reviewed MySQL service status because DVWA depends on a database backend.

Actions completed:

- Checked MySQL service status
- Restarted MySQL when needed
- Verified MySQL listening ports
- Accessed the MySQL console for administrative review

---

### 16. Evidence Collection and Documentation

Created an evidence folder to save system state and hardening outputs for documentation.

Evidence collected included:

- Hostname information
- IP address information
- Routing table
- Listening ports
- Firewall status
- Fail2Ban status
- Chrony tracking
- Running services
- Failed services

This information supports project reporting, audit evidence, and GitHub portfolio documentation.

---

## Skills Demonstrated

- Linux server administration
- Ubuntu Server hardening
- SSH security configuration
- Firewall configuration with UFW
- Intrusion prevention using Fail2Ban
- File integrity monitoring with AIDE
- Vulnerability auditing with Lynis
- System logging and event review
- Network scanning and validation
- Traffic capture and packet analysis
- Service management with systemctl
- Apache and MySQL service review
- Linux kernel parameter hardening
- Cybersecurity documentation
- SOC-style investigation workflow
- Evidence collection
- Troubleshooting and problem solving
- GRC-style reporting and control mapping

---

## Issues Encountered

Several issues were encountered during the lab and were used as troubleshooting opportunities.

### AIDE Initialization Issues

AIDE required proper database initialization before file integrity checks could be performed. The database had to be initialized and then copied into the correct active database location before checks would run properly.

### Fail2Ban Configuration Issues

Fail2Ban required a properly configured local jail file before SSH protection could be validated. The SSH jail configuration had to be reviewed and restarted before the service status could be confirmed.

### SSH Hardening Considerations

SSH settings required careful review to avoid accidentally locking out administrative access. Configuration syntax was tested before restarting SSH.

### Service Review

Some services were not required for the server role and had to be reviewed before disabling. This helped avoid disabling services that could affect lab functionality.

### DVWA Accessibility Versus Hardening

Because DVWA is intentionally vulnerable and requires web access for training, firewall rules had to balance security with lab usability. HTTP access was reviewed based on whether DVWA needed to be reachable during testing.

### Time Synchronization Review

Chrony configuration had to be validated to make sure logs and security events had accurate timestamps for later analysis.

---

## Final Outcome

The DVWA Ubuntu server was successfully reviewed, hardened, and documented as part of a cybersecurity lab environment.

The project resulted in:

- A documented Ubuntu DVWA server baseline
- Hardened SSH configuration
- Active firewall rule review
- Fail2Ban SSH brute-force protection
- AIDE file integrity monitoring
- Lynis security audit results
- Chrony time synchronization validation
- Reduced unnecessary services
- Network hardening through sysctl settings
- Improved logging and monitoring workflow
- Evidence collection for reporting
- A repeatable command reference for future hardening checks

This project demonstrates the ability to secure and monitor a Linux-based web server while maintaining a controlled training environment for cybersecurity testing.

---

## Reflection

This project helped develop practical cybersecurity and system administration skills by turning a vulnerable Ubuntu DVWA server into a monitored and partially hardened lab asset.

The most important lesson from this project was learning how to balance security controls with operational requirements. Because DVWA is intentionally vulnerable, the server could not simply be locked down completely. Instead, hardening had to be applied in a way that protected remote access, improved visibility, reduced unnecessary exposure, and preserved the ability to conduct authorized lab testing.

This project also reinforced the importance of documentation. Commands, screenshots, logs, and evidence files are critical for proving what was done, explaining why it was done, and showing measurable improvement over time.

---

## Disclaimer

This project was completed in an authorized lab environment for educational and professional development purposes only. Any scanning, testing, or attack simulation referenced in this project was performed against systems owned or controlled within the lab environment.
