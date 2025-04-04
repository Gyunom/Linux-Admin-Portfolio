# 🛠️ System Administration & Maintenance Exercises  
**By [Gyunom Maigida]**

This document walks you through how I completed key Linux system administration exercises related to user management, permissions, automation with cron, and system resource monitoring. I documented my process step-by-step with explanations, commands, and scripts. Each task was executed and tested on a Linux environment (Ubuntu 22.04 LTS).

---

## 1. Managing Users, Groups, and Permissions

To begin, I created a group called `projectteam`, added three users to the system (`nene`, `nom`, and `dums`), and added them to the group.

```bash
sudo groupadd projectteam

for user in nene nom dums; do
  sudo useradd -m -G projectteam $user
  echo "$user:password" | sudo chpasswd
done
```

I then created a shared folder `/srv/projectteam`, set the group ownership, and applied the `setgid` bit to ensure new files inherit the group:

```bash
sudo mkdir -p /srv/projectteam
sudo chown root:projectteam /srv/projectteam
sudo chmod 2775 /srv/projectteam
```

---

## 2. Using ACLs for Granular Permissions

Next, I created a `securegroup` and restricted access to `/data/secure`. I also used ACL to give `dave` read-only access to a specific file inside the directory without adding him to the group.

```bash
sudo groupadd securegroup
sudo mkdir -p /data/secure
sudo chown root:securegroup /data/secure
sudo chmod 2770 /data/secure
sudo chmod g+s /data/secure

sudo touch /data/secure/important.txt
sudo setfacl -m u:dave:r-- /data/secure/important.txt
```

---

## 3. Automating Backups with Cron

I created a backup script that archives the `/home/projectteam` directory daily and stores the archive in `/backups` with a timestamp. It logs success/failure messages.

**Script: `/usr/local/bin/backup_script.sh`**
```bash
#!/bin/bash
DATE=$(date +"%F")
BACKUP_DIR="/backups"
SOURCE_DIR="/home/projectteam"
DEST="$BACKUP_DIR/projectteam_$DATE.tar.gz"
LOGFILE="/var/log/projectteam_backup.log"

tar -czf $DEST $SOURCE_DIR && \
echo "$DATE Backup successful." >> $LOGFILE || \
echo "$DATE Backup failed." >> $LOGFILE
```

I made the script executable and scheduled it to run every day at 2 AM using cron:

```bash
sudo chmod +x /usr/local/bin/backup_script.sh
crontab -e
# Add:
0 2 * * * /usr/local/bin/backup_script.sh
```

---

## 4. Emailing Weekly Disk Usage Reports

To monitor storage usage, I wrote a script that emails a summary of disk usage and `/home` directory sizes every Monday at 8 AM.

**Script: `/usr/local/bin/disk_report.sh`**
```bash
#!/bin/bash
REPORT=$(mktemp)
echo "Disk Usage Report - $(date)" > $REPORT
df -h >> $REPORT
echo -e "\nHome Directory Sizes:" >> $REPORT
du -sh /home/* >> $REPORT
mail -s "Weekly Disk Report" admin@example.com < $REPORT
```

Scheduled via cron:
```bash
crontab -e
# Add:
0 8 * * 1 /usr/local/bin/disk_report.sh
```

---

## 5. Monitoring System Health

I created a system health monitor script that logs high CPU (>80%), memory (>75%), or disk usage (>90%) to `/var/log/sys_alerts.log`.

**Script: `/usr/local/bin/sys_health.sh`**
```bash
#!/bin/bash
LOGFILE="/var/log/sys_alerts.log"
DATE=$(date)
CPU=$(top -bn1 | grep "Cpu(s)" | awk '{print $2 + $4}')
MEM=$(free | awk '/Mem:/ {print $3/$2 * 100.0}')
DISK=$(df -h | awk '$5+0 > 90 {print $0}')

if (( $(echo "$CPU > 80" | bc -l) )); then
  echo "$DATE High CPU usage: $CPU%" >> $LOGFILE
fi

if (( $(echo "$MEM > 75" | bc -l) )); then
  echo "$DATE High Memory usage: $MEM%" >> $LOGFILE
fi

if [ ! -z "$DISK" ]; then
  echo -e "$DATE High Disk usage:\n$DISK" >> $LOGFILE
fi
```

Optional cron job:
```bash
*/30 * * * * /usr/local/bin/sys_health.sh
```

---



## 6. Generating a Daily System Report

Finally, I created a script that logs top processes, logged-in users, and uptime to a report file.

**Script: `/usr/local/bin/system_report.sh`**
```bash
#!/bin/bash
DATE=$(date +%F)
REPORT_FILE="/var/reports/daily_status_$DATE.txt"

{
  echo "System Report for $DATE"
  echo "============================="
  echo "\nTop 5 Memory-consuming Processes:"
  ps aux --sort=-%mem | head -6
  echo "\nTop 5 CPU-consuming Processes:"
  ps aux --sort=-%cpu | head -6
  echo "\nLogged-in Users:"
  who
  echo "\nSystem Uptime:"
  uptime
} > $REPORT_FILE
```

Scheduled at 7 AM:
```bash
sudo mkdir -p /var/reports
0 7 * * * /usr/local/bin/system_report.sh
```

---

## Summary

These exercises helped reinforce my understanding of user/group permissions, scheduled task automation, and system health monitoring in a real-world Linux environment. Each task was designed to simulate common administrative responsibilities, and I ensured every script was tested, scheduled with cron, and logged properly.


