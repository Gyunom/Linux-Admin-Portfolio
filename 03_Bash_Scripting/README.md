# 🔄 Bash Scripting: Automated System Update & Cleanup  
**By [Gyunom Maigida]**

This script is part of my Linux Admin automation exercises. I wanted to replicate a basic but essential responsibility for sysadmins: keeping the system clean, updated, and running smoothly without manual intervention every time.

---

## 🎯 Objective

Write a Bash script that performs the following system maintenance tasks:

- Updates package repositories
- Upgrades all installed packages
- Removes unnecessary packages
- Cleans the APT cache
- Deletes old log files (older than 7 days) from `/var/log`

This helps reduce manual overhead and ensures the system stays lean and secure.

---

## 🛠️ Script: `update_and_cleanup.sh`

I created a shell script using `nano` and saved it to `/usr/local/bin/update_and_cleanup.sh`.

```bash
#!/bin/bash

# Update and Upgrade
echo "Updating package lists..."
sudo apt update -y

echo "Upgrading installed packages..."
sudo apt upgrade -y

# Autoremove unnecessary packages
echo "Removing unnecessary packages..."
sudo apt autoremove -y

# Clean package cache
echo "Cleaning apt cache..."
sudo apt clean

# Remove old logs
echo "Removing old log files (older than 7 days)..."
sudo find /var/log -type f -name "*.log" -mtime +7 -exec rm -f {} \;

echo "System update and cleanup complete."
```

### ✅ Making the Script Executable
```bash
sudo chmod +x /usr/local/bin/update_and_cleanup.sh
```

---

## ⏰ Automating the Script with Cron

To make the process truly hands-free, I scheduled the script to run automatically every Monday at 3:00 AM using cron:

```bash
crontab -e
```

Then I added the following line to my crontab:

```bash
0 3 * * 1 /usr/local/bin/update_and_cleanup.sh >> /var/log/sys_maintenance.log 2>&1
```

This ensures weekly maintenance is logged and performed during off-hours.

---

## 💡 Why This Script Matters

- It automates a real-world sysadmin task.
- Reinforces usage of tools like `apt`, `find`, and `cron`.
- Helps avoid log file bloating and disk space issues.
- Trains me in writing safe, repeatable scripts for production use.

---

## ✅ Result

The script was tested on Ubuntu 22.04. It successfully updated the system, cleaned unused files, and removed logs older than 7 days. Running it as a cron job ensures maintenance runs without needing my manual involvement.

Next step: Wrap this into a larger maintenance toolkit for multiple servers.

