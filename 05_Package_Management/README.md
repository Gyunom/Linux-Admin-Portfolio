# My Personal Linux Package Management Guide (APT-Based Systems)

>  By: Gyunom Maigida 
>  Journey: Hands-on learning as part of my Linux Admin portfolio  
>  Focus: Mastering real-world package and repository management on Ubuntu

---

##  1. Update and Upgrade

###  Why I Practiced This:
I wanted to make sure my system always runs the latest security and feature updates. This is the first habit of any good sysadmin.

###  Commands I Used:
```bash
sudo apt update
sudo apt upgrade -y
sudo apt full-upgrade -y
```

###  Explanation:
- `sudo`: Runs the command as a superuser (root). Required for system-level changes.
- `apt`: Advanced Package Tool – handles package installation, upgrades, and removal.
- `update`: Refreshes the list of available packages and their versions. It doesn't install anything.
- `upgrade`: Installs the newest versions of packages currently installed (without removing or replacing existing ones).
- `full-upgrade`: Like `upgrade`, but smarter – it can remove obsolete packages or install new dependencies if needed.
- `-y`: Automatically answers "yes" to any prompt, so the upgrade can run non-interactively.

I used these commands to keep my system up to date and to prepare it for new package installations without any version conflicts.

---

##  2. Search, Install, and Remove Packages

###  Why I Practiced This:
I needed to know how to quickly install, uninstall, and search for packages — especially when setting up new environments or cleaning up unused tools.

###  Commands I Used:
```bash
apt search htop
sudo apt install htop
sudo apt remove htop
sudo apt purge htop
```

###  Explanation:
- `search htop`: Looks for the term `htop` in the package index. Useful when I'm not sure of the exact package name.
- `install htop`: Downloads and installs `htop`, a resource monitor. Automatically pulls required dependencies.
- `remove htop`: Uninstalls the `htop` package but leaves behind its configuration files.
- `purge htop`: Uninstalls `htop` **and** deletes its config files, restoring the system to a clean state.

This process helped me understand how to manage software in Linux environments efficiently and cleanly.

---

##  3. Package Information

###  Why I Practiced This:
I didn’t want to install anything blindly. I needed a way to inspect what a package does, what it depends on, and who maintains it.

###  Commands I Used:
```bash
apt show curl
dpkg -s curl
apt list --installed
```

###  Explanation:
- `apt show curl`: Gives detailed info about the `curl` package — version, description, dependencies, source, maintainer, etc.
- `dpkg -s curl`: Shows installed package status, version, and description (from the dpkg database).
- `apt list --installed`: Lists every installed package and its version — useful for system audits.

 I used these commands to learn how to investigate package details before installing or troubleshooting.

---

## 🛠 4. Fix Broken Packages

###  Why I Practiced This:
At some point, I *will* break something. So I wanted to get comfortable with fixing broken dependencies or interrupted installs.

###  Commands I Used:
```bash
sudo apt install -f
sudo dpkg --configure -a
```

###  Explanation:
- `install -f`: The `-f` flag stands for **fix-broken** — it tries to install missing dependencies and correct broken package states.
- `dpkg --configure -a`: Reconfigures **all partially installed** packages. Useful if an install was interrupted or corrupted.

 These tools are critical for recovering from package errors without reinstalling the OS — something every admin should know.

---

##  5. Hold and Unhold Packages

###  Why I Practiced This:
Sometimes I don’t want a critical package (like a kernel module or nginx) to be upgraded and break my config. Freezing it makes sense.

###  Commands I Used:
```bash
sudo apt-mark hold htop
sudo apt-mark unhold htop
```

###  Explanation:
- `apt-mark hold nginx`: Marks the `htop` package to be **held back** from upgrades.
- `apt-mark unhold nginx`: Removes the hold, allowing `htop` to upgrade again.

 I used this when I had a custom nginx setup and didn’t want a future upgrade to overwrite my configs.

---

##  6. Clean Up the System

###  Why I Practiced This:
I like keeping my systems lean and clean. After installing and removing several packages, things can get cluttered fast.

###  Commands I Used:
```bash
sudo apt autoremove -y
sudo apt clean
```

###  Explanation:
- `autoremove`: Deletes orphaned packages that were installed as dependencies but are no longer needed.
- `clean`: Clears out the local package cache at `/var/cache/apt/archives/`, freeing disk space.

These commands helped me reduce unnecessary disk usage and maintain a tidy system.

---

##  7. My Bash Script: `update_system.sh`

```bash
#!/bin/bash

# ─── MY SYSTEM UPDATE SCRIPT ───
# Purpose: Automate routine updates on my machine
# Context: Used this script to speed up weekly maintenance

echo "Starting system update..."

sudo apt update -y
echo Package list updated."

sudo apt upgrade -y
echo "Packages upgraded."

sudo apt full-upgrade -y
echo "Full upgrade done."

sudo apt autoremove -y
echo " Removed unnecessary packages."

sudo apt clean
echo "Cleaned up package cache."

echo "System update complete!"
```

###  Reflection:
This script turned my manual update process into a one-liner. I run this weekly now as part of system hygiene. It’s fast, reliable, and reminds me how easy automation can be.

---

##  8. My Python Script: `update_system.py`

```python
#!/usr/bin/env python3

import subprocess

def run_command(command, description):
    print(f" {description}...")
    try:
        subprocess.run(command, check=True, shell=True)
        print(f" {description} completed.")
    except subprocess.CalledProcessError:
        print(f" Error during: {description}")

# ─── SYSTEM UPDATE FLOW ───
print("Starting system update...")

run_command("sudo apt update -y", "Updating package list")
run_command("sudo apt upgrade -y", "Upgrading installed packages")
run_command("sudo apt full-upgrade -y", "Performing full upgrade")
run_command("sudo apt autoremove -y", "Removing unnecessary packages")
run_command("sudo apt clean", "Cleaning package cache")

print("System update complete!")
```

###  Reflection:
I decided to try scripting with a different language to have a better understand and be flexible with my scripting skill when called upon. Writing this in Python gave me more control — especially with error handling and modularity. I’ll be building on this to include logging and maybe even email reports in the future.

---

##  Final Thoughts

This section of my portfolio gave me real-world, confidence-boosting experience. Now I can:
- Install, search, and manage packages.
- Troubleshoot broken installs.
- Freeze critical software.
- Automate some things with scripts.

Mastering this step made me feel like a true Linux Admin in the making. On to the next phase — repository management and deeper automation! 💪

---
