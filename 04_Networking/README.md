# 🌐 My Personal Linux Networking Documentation

This stage of my Linux Admin portfolio focused on mastering basic networking concepts and commands in Linux. I wanted to not just learn the syntax, but understand **why** each command is important and **what** I was trying to achieve. Here's a detailed walkthrough of everything I did.

---

## 1. 🔍 Basic TCP/IP Networking and Troubleshooting

### ➤ `ping`
```bash
ping 8.8.8.8
ping google.com
```
I used `ping` to test whether my system could reach the internet and resolve domain names. Pinging `8.8.8.8` (Google DNS) helped me check raw network connectivity, while `ping google.com` tested DNS resolution. If the domain ping fails but the IP works, I know it's a DNS issue.

### ➤ `traceroute`
```bash
traceroute google.com
```
This command let me see the path my packets take to reach Google's servers. It helped me understand where delays or failures might be occurring in the network — like spotting a slow router or dead hop.

### ➤ `ss` and `netstat`
```bash
ss -tulnp
ss -s
```
I used `ss` to see which services were listening on which ports. It replaced the older `netstat` command and showed me details like process IDs and socket states. `ss -s` gave me a useful summary of all connections.

### ➤ IP and Routing Info
```bash
ip a
ip route
```
These commands showed my system’s IP addresses and routing table. `ip a` helped me confirm what IP was assigned, while `ip route` showed the default gateway and route priorities. Very useful for troubleshooting local network config.

### ➤ `dig`
```bash
dig google.com
dig +short google.com
```
I used `dig` to perform DNS lookups and verify that domain names were resolving properly. `+short` gave a concise result. This tool is essential when diagnosing DNS issues or verifying changes to domain configs.

---

## 2. 🔐 Securing SSH Access

SSH is the backbone of remote server access, so I spent time learning how to secure it properly.

### ➤ Generate SSH Keys and Enable Key-Based Login
```bash
ssh-keygen -t rsa
ssh-copy-id user@remotehost
```
I generated an RSA key pair and copied the public key to the remote server. This allowed me to log in securely without a password, reducing the risk of brute-force attacks and making automated access safer.

### ➤ Harden SSH Daemon Settings
Edited `/etc/ssh/sshd_config`:
```
Port 2222
PasswordAuthentication no
AllowUsers myusername
```
I changed the SSH port to a non-standard one to reduce automated attacks, disabled password-based logins, and restricted SSH access to just my user. These steps hardened the system against common intrusion methods.

### ➤ Restart SSH Safely
```bash
sudo systemctl restart sshd
```
I restarted the SSH service to apply my changes. I made sure to keep another terminal open during testing to avoid being locked out by mistake.

---

## 3. 🔥 UFW and IPTables Firewall Configuration

After securing remote access, I moved on to configuring firewalls to control traffic flow and protect the system.

### ➤ Set Up UFW Rules
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2222/tcp
sudo ufw enable
```
I configured UFW (Uncomplicated Firewall) to block all incoming traffic by default, while allowing outbound connections. I then allowed traffic on port 2222 (my custom SSH port). Finally, I enabled the firewall. This created a strong default security posture.

### ➤ View and Adjust UFW Rules
```bash
sudo ufw status verbose
sudo ufw deny from 192.168.1.5
```
I checked the current firewall rules in detail, then blocked a specific IP to simulate managing bad actors or suspicious traffic sources.

### ➤ Explore IPTables (Advanced)
```bash
sudo iptables -L -v -n
sudo iptables -A INPUT -p tcp --dport 2222 -j ACCEPT
sudo iptables -A INPUT -s 192.168.1.100 -j DROP
sudo iptables-save > /etc/iptables/rules.v4
```
To go deeper, I experimented with IPTables. I listed current rules, allowed SSH on my custom port, blocked a test IP, and saved the rules so they persist after reboot. This helped me understand packet filtering on a more granular level.

---

## ✅ What I Gained from This Phase

- I can now confidently troubleshoot basic network issues using tools like `ping`, `traceroute`, and `dig`.
- I understand how to inspect and manage network interfaces, ports, and routes using `ip` and `ss`.
- I hardened SSH by disabling password login and restricting access.
- I set up and managed a firewall using both UFW (easy) and IPTables (advanced).
- This phase built a strong foundation for managing servers in real-world environments.

Next, I’ll be diving into DNS hosting, VPNs, and more advanced networking tasks as I continue building out my portfolio.
