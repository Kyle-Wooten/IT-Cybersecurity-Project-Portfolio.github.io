# Network Troubleshooting

## Objective

The objective of this project was to document and demonstrate a structured network troubleshooting process using hands-on lab work completed throughout the Kyle School project. The project focused on identifying, isolating, testing, and resolving common network connectivity issues across virtualized systems, Cisco Packet Tracer networks, Windows clients, Ubuntu/Kali Linux systems, Proxmox infrastructure, DNS configurations, routing, and web-based services.

This project showcases the ability to troubleshoot real-world network problems using command-line tools, device configuration checks, connectivity testing, service validation, and professional documentation.

---

## Environment

- **Project Folder:** Networking / Network-Troubleshooting
- **Platform:** Proxmox Virtual Environment, Cisco Packet Tracer, Windows, Linux
- **Operating Systems:**
  - Windows 10 / Windows 11
  - Ubuntu Server
  - Kali Linux
  - Windows Server
  - Proxmox VE
- **Tools Used:**
  - `ping`
  - `ip a`
  - `ip route`
  - `ipconfig`
  - `nslookup`
  - `tracert`
  - `ssh`
  - `systemctl`
  - `journalctl`
  - `ss`
  - `lsof`
  - `tcpdump`
  - Wireshark
  - Cisco Packet Tracer
  - Cisco IOS commands
  - Proxmox web console
- **Network Ranges Used in Labs:**
  - 10.10.10.0/24
  - 10.10.10.0/16
  - 192.168.100.0/24
  - 192.168.200.0/24
- **Devices / VMs:**
  - Proxmox host
  - Ubuntu DVWA server
  - Kali Linux VM
  - Windows client VM
  - Windows Server domain controllers
  - Cisco 2911 routers
  - Cisco 2960 switches
  - Packet Tracer PCs, switches, routers, and wireless devices

---

## Project Scenario

Throughout the Kyle School labs, multiple network connectivity issues were encountered and resolved. These issues included loss of access to Proxmox, inability to reach a web interface, destination host unreachable messages, DNS failures, incorrect static IP configuration, routing issues, SSH connectivity problems, Packet Tracer VLAN and subinterface errors, firewall rule conflicts, and service availability problems.

This project documents those troubleshooting methods as a repeatable workflow that can be used in future IT support, network administration, cybersecurity lab, and SOC environments.

---

## Tasks Completed

### 1. Verified Physical and Virtual Connectivity

Checked whether the host machine, virtual machines, and network interfaces were connected to the correct virtual switch, bridge, or network adapter.

Actions included:

- Confirming VM network adapters were enabled
- Checking Proxmox bridge configuration
- Verifying Windows network adapter status
- Reviewing Linux interface names and IP assignments
- Confirming Cisco Packet Tracer cables and device links were active

---

### 2. Confirmed IP Addressing

Reviewed IP addresses, subnet masks, default gateways, and DNS settings on Windows, Linux, and Cisco devices.

Commands used included:

```bash
ip a
ip route
ipconfig /all
ping <gateway-ip>
ping <target-ip>
```

---

### 3. Tested Local and Remote Connectivity

Used ping and route testing to determine whether the issue was local to the host, between devices, or related to routing.

Testing included:

- Loopback testing
- Gateway testing
- Same-subnet testing
- Cross-subnet testing
- Internet/DNS testing when applicable
- Web interface reachability testing

---

### 4. Troubleshot DNS Issues

Investigated name resolution issues when systems could communicate by IP address but not by hostname or domain name.

Tools and commands included:

```bash
nslookup <hostname>
nslookup <domain-name>
ipconfig /flushdns
resolvectl status
cat /etc/resolv.conf
```

---

### 5. Troubleshot Routing and Gateway Issues

Reviewed routing tables and gateway settings to determine whether traffic had a valid path.

Tools and commands included:

```bash
ip route
route print
tracert <target-ip>
traceroute <target-ip>
show ip route
show running-config
```

---

### 6. Troubleshot Proxmox Access Issues

Worked through Proxmox access issues involving static IP addressing, browser access, ping testing, Windows client IP configuration, DNS, and local network reachability.

Example lab details included:

- Proxmox server: `team2.tech`
- Proxmox IP: `10.10.10.88/24`
- Windows client IP: `10.10.10.55/24`
- Issue: Web GUI and SSH access problems
- Resolution path: Verified client IP, subnet, gateway, DNS, ping results, and browser access to the Proxmox management interface

---

### 7. Troubleshot Linux SSH and Service Issues

Reviewed SSH and service availability on Linux systems.

Commands used included:

```bash
systemctl status ssh
sudo systemctl restart ssh
sudo ss -tulnp
sudo lsof -i
sudo journalctl -u ssh
```

---

### 8. Troubleshot Packet Tracer VLAN and Router Issues

Resolved network design and configuration issues in Packet Tracer labs involving Cisco 2911 routers and Cisco 2960 switches.

Troubleshooting areas included:

- VLAN creation
- Access port assignment
- Trunk port configuration
- Router-on-a-stick subinterfaces
- 802.1Q encapsulation
- Inter-VLAN routing
- DHCP configuration
- ACL testing
- OSPF route verification

Common Cisco commands included:

```text
show vlan brief
show interfaces trunk
show ip interface brief
show running-config
show ip route
show access-lists
ping
```

---

### 9. Captured and Reviewed Network Traffic

Used packet capture tools to validate whether traffic was reaching the expected interface and to support SOC-style investigation.

Tools included:

- `tcpdump`
- Wireshark

Example commands:

```bash
sudo tcpdump -i <interface>
sudo tcpdump -i <interface> port 22
sudo tcpdump -i <interface> port 80
sudo tcpdump -i <interface> -w troubleshooting-capture.pcap
```

---

### 10. Documented Issues and Resolutions

Created professional troubleshooting notes, lab reports, and project summaries to explain the issue, cause, testing process, resolution, and final outcome.

Documentation focused on:

- What was broken
- What commands were used
- What evidence was observed
- What change fixed the problem
- How the fix was validated

---

## Skills Demonstrated

- Network troubleshooting methodology
- IP addressing and subnetting
- Gateway and routing validation
- DNS troubleshooting
- Windows network configuration
- Linux network configuration
- Proxmox bridge troubleshooting
- SSH troubleshooting
- Cisco router and switch troubleshooting
- VLAN and trunk verification
- Packet Tracer troubleshooting
- Firewall and service validation
- Packet capture and traffic review
- Technical documentation
- Root cause analysis
- Lab evidence collection

---

## Issues Encountered

### Destination Host Unreachable

During lab work, some systems returned destination host unreachable errors. This indicated that the source machine did not have a valid path to the target system or that the local network configuration was incorrect.

**Resolution:**

- Verified IP address and subnet mask
- Confirmed the default gateway
- Checked whether both systems were on the same network
- Reviewed virtual switch and bridge configuration
- Retested connectivity using ping

---

### Proxmox Web GUI Not Reachable

The Proxmox server responded inconsistently during troubleshooting and was not always reachable through the browser.

**Resolution:**

- Verified the Proxmox IP address
- Set the Windows client to the correct static IP range
- Tested ping between the Windows client and Proxmox server
- Confirmed the correct browser URL and management port
- Reviewed DNS/name resolution behavior

---

### DNS Name Resolution Failure

Some systems were reachable by IP address but not by hostname or local domain name.

**Resolution:**

- Reviewed DNS settings
- Tested name resolution with `nslookup`
- Checked `/etc/resolv.conf` or `resolvectl status`
- Used direct IP access when DNS was not required
- Updated DNS settings where appropriate

---

### SSH Connectivity Problems

SSH access failed or became unavailable during configuration changes.

**Resolution:**

- Checked SSH service status
- Verified the server was listening on port 22
- Reviewed firewall rules
- Checked authentication logs
- Restarted SSH after validating configuration

---

### Packet Tracer Subinterface Configuration Error

Packet Tracer displayed an error when configuring IP routing on a LAN subinterface before proper VLAN encapsulation was configured.

**Resolution:**

- Configured the router subinterface with 802.1Q encapsulation first
- Applied the IP address after VLAN tagging was set
- Verified router-on-a-stick configuration
- Tested inter-VLAN routing with ping

---

## Final Outcome

This project resulted in a repeatable network troubleshooting workflow that can be applied across Windows, Linux, Proxmox, Cisco Packet Tracer, and cybersecurity lab environments.

The project successfully demonstrates the ability to:

- Identify network connectivity problems
- Isolate issues by layer
- Validate IP addressing and routing
- Troubleshoot DNS and gateway problems
- Restore access to server and web interfaces
- Troubleshoot Cisco VLAN and routing issues
- Use packet capture for validation
- Document problems and resolutions professionally

---

## Screenshots

Screenshots should be added to a `screenshots/` folder in this project directory.

Recommended screenshots include:

```markdown
![IP Configuration](screenshots/ip-configuration.png)
![Ping Test](screenshots/ping-test.png)
![Routing Table](screenshots/routing-table.png)
![Proxmox GUI Access](screenshots/proxmox-gui-access.png)
![Packet Tracer VLAN Verification](screenshots/packet-tracer-vlan-verification.png)
![Wireshark Capture](screenshots/wireshark-capture.png)
```

---

## Recommended Repository Structure

```text
Networking/
└── Network-Troubleshooting/
    ├── README.md
    ├── final-lab-report.pdf
    ├── commands.txt
    ├── notes.md
    ├── evidence/
    │   ├── ip-configuration.txt
    │   ├── routing-table.txt
    │   ├── ping-results.txt
    │   ├── dns-results.txt
    │   └── service-status.txt
    └── screenshots/
        ├── ip-configuration.png
        ├── ping-test.png
        ├── routing-table.png
        ├── proxmox-gui-access.png
        └── packet-tracer-verification.png
```

---

## Reflection

This project helped reinforce the importance of using a structured troubleshooting process instead of guessing. Many network issues appear similar on the surface, but the root cause may be IP addressing, DNS, routing, firewall rules, service status, virtual bridge configuration, or device misconfiguration.

The most important lesson from this project was to test one layer at a time: verify the local interface, confirm IP settings, test the gateway, test the destination by IP address, test DNS, review routes, check services, and then validate the fix.

This project also demonstrated the value of documentation. Recording commands, outputs, screenshots, and resolutions makes troubleshooting easier to explain, repeat, and showcase in a professional portfolio.

---

## Disclaimer

This project was completed in an authorized educational lab environment. All scanning, packet capture, troubleshooting, and configuration testing was performed on lab-owned or controlled systems.
