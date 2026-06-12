# DVWA Ubuntu Server Hardening Checklist

## Project Information

| Item | Details |
|---|---|
| Project Name | DVWA Ubuntu Server Hardening |
| Server Hostname | UbunServDVWA-TeamA |
| Server IP Address | 10.10.10.223/16 |
| Platform | Proxmox Virtual Environment |
| Server Role | DVWA Ubuntu web application server |
| Lab Type | Cybersecurity / SOC Capstone Lab |
| Purpose | Secure, monitor, and document an intentionally vulnerable Ubuntu DVWA server in an authorized lab environment |

---

## 1. System Baseline

- [ ] Confirm current user with `whoami`
- [ ] Confirm hostname with `hostname`
- [ ] Review host details with `hostnamectl`
- [ ] Review operating system information with `cat /etc/os-release`
- [ ] Review kernel version with `uname -a`
- [ ] Confirm system date and time with `date`
- [ ] Check uptime with `uptime`
- [ ] Review logged-in users with `who`
- [ ] Review login history with `last`
- [ ] Review IP configuration with `ip a`
- [ ] Review routing table with `ip route`
- [ ] Review DNS resolver status with `resolvectl status`
- [ ] Review listening ports with `sudo ss -tulnp`
- [ ] Review open network connections with `sudo lsof -i`
- [ ] Review running services with `systemctl --type=service --state=running`
- [ ] Review failed services with `systemctl --failed`

**Evidence to save:**

- [ ] `hostnamectl.txt`
- [ ] `ip-addresses.txt`
- [ ] `ip-route.txt`
- [ ] `listening-ports.txt`
- [ ] `running-services.txt`
- [ ] `failed-services.txt`

---

## 2. Package Updates and Maintenance

- [ ] Run `sudo apt update`
- [ ] Run `sudo apt upgrade -y`
- [ ] Run `sudo apt autoremove -y`
- [ ] Run `sudo apt autoclean`
- [ ] Review available updates with `apt list --upgradable`

**Validation:**

- [ ] System packages updated
- [ ] Unnecessary packages removed
- [ ] No unexpected update errors observed

---

## 3. User and Account Review

- [ ] Review local users with `cat /etc/passwd`
- [ ] Review login shell users with `awk -F: '$7 ~ /(bash|sh)$/ {print $1 " " $7}' /etc/passwd`
- [ ] Review local groups with `cat /etc/group`
- [ ] Review sudo group membership with `getent group sudo`
- [ ] Review password aging with `chage -l $USER`
- [ ] Check root account status with `sudo passwd -S root`
- [ ] Lock root account with `sudo passwd -l root`
- [ ] Confirm root account is locked with `sudo passwd -S root`

**Validation:**

- [ ] Root account is locked
- [ ] Sudo access reviewed
- [ ] No unauthorized local accounts identified

---

## 4. SSH Hardening

- [ ] Back up SSH configuration with `sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak`
- [ ] Open SSH configuration with `sudo nano /etc/ssh/sshd_config`
- [ ] Verify or configure `PermitRootLogin no`
- [ ] Verify or configure `PasswordAuthentication no`, if using SSH keys
- [ ] Verify or configure `PubkeyAuthentication yes`
- [ ] Verify or configure `MaxAuthTries 3`
- [ ] Verify or configure `ClientAliveInterval 300`
- [ ] Verify or configure `ClientAliveCountMax 2`
- [ ] Verify or configure `LoginGraceTime 60`
- [ ] Verify or configure `X11Forwarding no`
- [ ] Verify or configure `AllowTcpForwarding no`
- [ ] Verify or configure `Banner /etc/issue.net`
- [ ] Create or update `/etc/issue.net`
- [ ] Test SSH syntax with `sudo sshd -t`
- [ ] Restart SSH with `sudo systemctl restart ssh`
- [ ] Confirm SSH is active with `sudo systemctl status ssh`
- [ ] Review SSH logs with `sudo journalctl -u ssh`
- [ ] Review authentication logs with `sudo tail -n 100 /var/log/auth.log`

**Validation:**

- [ ] SSH configuration syntax passed
- [ ] SSH service restarted successfully
- [ ] Root SSH login disabled
- [ ] Warning banner configured
- [ ] Remote access confirmed after changes

---

## 5. UFW Firewall Configuration

- [ ] Install UFW with `sudo apt install ufw -y`
- [ ] Check UFW status with `sudo ufw status verbose`
- [ ] Set default incoming policy with `sudo ufw default deny incoming`
- [ ] Set default outgoing policy with `sudo ufw default allow outgoing`
- [ ] Allow SSH with `sudo ufw allow 22/tcp`
- [ ] Allow HTTP with `sudo ufw allow 80/tcp`, only if DVWA web testing is required
- [ ] Deny HTTP with `sudo ufw deny 80/tcp`, if DVWA does not need to be reachable
- [ ] Enable UFW with `sudo ufw enable`
- [ ] Reload UFW with `sudo ufw reload`
- [ ] Review numbered rules with `sudo ufw status numbered`
- [ ] Save firewall evidence with `sudo ufw status verbose > ~/DVWA-Hardening-Evidence/ufw-status.txt`

**Validation:**

- [ ] UFW is active
- [ ] Default incoming traffic is denied
- [ ] SSH is allowed
- [ ] HTTP rule matches the current lab objective

---

## 6. Fail2Ban SSH Protection

- [ ] Install Fail2Ban with `sudo apt install fail2ban -y`
- [ ] Check service status with `sudo systemctl status fail2ban`
- [ ] Enable Fail2Ban with `sudo systemctl enable fail2ban`
- [ ] Start Fail2Ban with `sudo systemctl start fail2ban`
- [ ] Create or edit jail file with `sudo nano /etc/fail2ban/jail.local`
- [ ] Configure SSH jail
- [ ] Restart Fail2Ban with `sudo systemctl restart fail2ban`
- [ ] Check Fail2Ban status with `sudo fail2ban-client status`
- [ ] Check SSH jail with `sudo fail2ban-client status sshd`
- [ ] Review logs with `sudo tail -n 100 /var/log/fail2ban.log`
- [ ] Save Fail2Ban status evidence

**Recommended SSH jail settings:**

```ini
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 1h
findtime = 10m
```

**Validation:**

- [ ] Fail2Ban is active
- [ ] SSH jail is enabled
- [ ] Failed SSH attempts are monitored
- [ ] Ban and unban commands tested in the lab, if needed

---

## 7. AIDE File Integrity Monitoring

- [ ] Install AIDE with `sudo apt install aide aide-common -y`
- [ ] Initialize AIDE with `sudo aideinit`
- [ ] Copy database with `sudo cp /var/lib/aide/aide.db.new /var/lib/aide/aide.db`
- [ ] Run integrity check with `sudo aide --check`
- [ ] Update database after approved changes with `sudo aide --update`
- [ ] Replace old database with updated database when appropriate

**Validation:**

- [ ] AIDE database created
- [ ] AIDE check runs successfully
- [ ] File integrity baseline established

---

## 8. Lynis Security Audit

- [ ] Install Lynis with `sudo apt install lynis -y`
- [ ] Run audit with `sudo lynis audit system`
- [ ] Review report with `sudo less /var/log/lynis-report.dat`
- [ ] Review log with `sudo less /var/log/lynis.log`
- [ ] Search warnings with `sudo grep -i warning /var/log/lynis-report.dat`
- [ ] Search suggestions with `sudo grep -i suggestion /var/log/lynis-report.dat`
- [ ] Review hardening index with `sudo grep -i hardening_index /var/log/lynis-report.dat`

**Validation:**

- [ ] Lynis audit completed
- [ ] Warnings reviewed
- [ ] Suggestions reviewed
- [ ] Hardening index recorded

---

## 9. Chrony Time Synchronization

- [ ] Install Chrony with `sudo apt install chrony -y`
- [ ] Check service with `sudo systemctl status chrony`
- [ ] Enable service with `sudo systemctl enable chrony`
- [ ] Review configuration with `sudo nano /etc/chrony/chrony.conf`
- [ ] Restart service with `sudo systemctl restart chrony`
- [ ] Check tracking with `chronyc tracking`
- [ ] Check sources with `chronyc sources -v`
- [ ] Check system time with `timedatectl status`
- [ ] Save Chrony evidence with `chronyc tracking > ~/DVWA-Hardening-Evidence/chrony-tracking.txt`

**Validation:**

- [ ] Chrony service is active
- [ ] Time synchronization confirmed
- [ ] Logs can be trusted for event correlation

---

## 10. Disable Unneeded Services

- [ ] Check Avahi with `systemctl status avahi-daemon`
- [ ] Disable Avahi if not needed with `sudo systemctl disable --now avahi-daemon`
- [ ] Check CUPS with `systemctl status cups`
- [ ] Disable CUPS if not needed with `sudo systemctl disable --now cups`
- [ ] Check Whoopsie with `systemctl status whoopsie`
- [ ] Disable Whoopsie if not needed with `sudo systemctl disable --now whoopsie`
- [ ] Check LightDM with `systemctl status lightdm`
- [ ] Disable LightDM if server GUI is not needed with `sudo systemctl disable --now lightdm`
- [ ] Confirm failed services with `systemctl --failed`

**Validation:**

- [ ] Unneeded services reviewed
- [ ] Attack surface reduced
- [ ] No required services accidentally disabled

---

## 11. Sysctl Network Hardening

- [ ] Back up sysctl configuration with `sudo cp /etc/sysctl.conf /etc/sysctl.conf.bak`
- [ ] Edit sysctl configuration with `sudo nano /etc/sysctl.conf`
- [ ] Apply changes with `sudo sysctl -p`
- [ ] Verify reverse path filtering with `sysctl net.ipv4.conf.all.rp_filter`
- [ ] Verify redirect settings with `sysctl net.ipv4.conf.all.accept_redirects`
- [ ] Verify send redirect settings with `sysctl net.ipv4.conf.all.send_redirects`

**Recommended settings:**

```conf
net.ipv4.conf.all.rp_filter = 2
net.ipv4.conf.default.rp_filter = 2
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.tcp_syncookies = 1
```

**Validation:**

- [ ] Kernel hardening settings applied
- [ ] Redirects disabled
- [ ] Reverse path filtering enabled
- [ ] TCP SYN cookies enabled

---

## 12. Log Review and Monitoring

- [ ] Review system logs with `sudo journalctl`
- [ ] Review recent system logs with `sudo journalctl -n 100`
- [ ] Follow logs with `sudo journalctl -f`
- [ ] Review authentication logs with `sudo less /var/log/auth.log`
- [ ] Search failed SSH attempts
- [ ] Search accepted SSH logins
- [ ] Search sudo activity
- [ ] Review login history with `last`
- [ ] Review failed login attempts with `lastb`, if available

**Validation:**

- [ ] Failed SSH attempts reviewed
- [ ] Successful logins reviewed
- [ ] Sudo activity reviewed
- [ ] Suspicious activity documented

---

## 13. Network Testing and Traffic Capture

- [ ] Test gateway connectivity with `ping -c 4 <GATEWAY_IP>`
- [ ] Test target connectivity with `ping -c 4 <TARGET_IP>`
- [ ] Review ARP table with `ip neigh`
- [ ] Run authorized Nmap scan from Kali/client machine
- [ ] Capture traffic with `sudo tcpdump -i <INTERFACE>`
- [ ] Save packet capture with `sudo tcpdump -i <INTERFACE> -w dvwa-capture.pcap`
- [ ] Review packet capture with `tcpdump -r dvwa-capture.pcap`
- [ ] Review capture in Wireshark if needed

**Validation:**

- [ ] Connectivity confirmed
- [ ] Open ports documented
- [ ] Packet capture evidence saved

---

## 14. Apache and DVWA Service Checks

- [ ] Check Apache status with `sudo systemctl status apache2`
- [ ] Restart Apache with `sudo systemctl restart apache2`
- [ ] Enable Apache with `sudo systemctl enable apache2`
- [ ] Check listening ports with `sudo ss -tulnp | grep apache`
- [ ] Test local HTTP response with `curl -I http://localhost`
- [ ] Review Apache access logs
- [ ] Review Apache error logs

**Validation:**

- [ ] Apache is running when DVWA testing is required
- [ ] DVWA web access works as intended
- [ ] Apache logs reviewed

---

## 15. MySQL Service Checks

- [ ] Check MySQL status with `sudo systemctl status mysql`
- [ ] Restart MySQL with `sudo systemctl restart mysql`
- [ ] Enable MySQL with `sudo systemctl enable mysql`
- [ ] Check listening ports with `sudo ss -tulnp | grep mysql`
- [ ] Access MySQL console with `sudo mysql`

**Validation:**

- [ ] MySQL service is active
- [ ] DVWA database support is available
- [ ] MySQL exposure reviewed

---

## 16. Evidence Collection

- [ ] Create evidence folder with `mkdir -p ~/DVWA-Hardening-Evidence`
- [ ] Save hostname output
- [ ] Save IP address output
- [ ] Save routing output
- [ ] Save listening ports output
- [ ] Save UFW status
- [ ] Save Fail2Ban status
- [ ] Save Chrony status
- [ ] Save running services
- [ ] Save failed services
- [ ] Compress evidence folder with `tar -czvf DVWA-Hardening-Evidence.tar.gz ~/DVWA-Hardening-Evidence`

**Validation:**

- [ ] Evidence folder created
- [ ] Required output files saved
- [ ] Evidence archive created
- [ ] Screenshots saved to repository

---

## 17. Final Validation

- [ ] Run `sudo sshd -t`
- [ ] Confirm SSH status
- [ ] Confirm UFW status
- [ ] Confirm Fail2Ban status
- [ ] Confirm AIDE database exists
- [ ] Confirm Chrony tracking
- [ ] Confirm listening ports
- [ ] Confirm no failed services
- [ ] Run final Lynis audit
- [ ] Save screenshots
- [ ] Update `README.md`
- [ ] Upload `commands.txt`
- [ ] Upload this checklist
- [ ] Upload final lab report

---

## Final Checklist Status

| Control Area | Status |
|---|---|
| System baseline reviewed | Complete |
| Packages updated | Complete |
| User accounts reviewed | Complete |
| Root account locked | Complete |
| SSH hardened | Complete |
| Firewall configured | Complete |
| Fail2Ban configured | Complete |
| AIDE initialized | Complete |
| Lynis audit completed | Complete |
| Chrony validated | Complete |
| Unneeded services reviewed | Complete |
| Sysctl settings reviewed | Complete |
| Logs reviewed | Complete |
| Evidence collected | Complete |
| Final report created | Complete |

---

## Notes

This checklist is intended for an authorized lab environment. DVWA is intentionally vulnerable, so the hardening process should balance security controls with the need to keep the application usable for controlled cybersecurity testing.
