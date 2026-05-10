# Day 01 - Linux For DevOps

Today I started my DevOps learning journey with Linux fundamentals.  
The main focus was to understand how Linux works, why it is important in DevOps, and how basic commands are used in real server environments.

## Topics Covered

### 1. Operating System Basics
- Learned what an Operating System is.
- Understood how OS manages hardware, software, memory, files, and processes.
- Learned that OS acts as a bridge between user and computer hardware.

### 2. Client OS vs Server OS
- Client OS is used by normal users for daily tasks.
- Server OS is used to run services, applications, websites, databases, and production workloads.
- In DevOps, Linux Server OS is very important because most cloud servers run on Linux.

### 3. Linux OS
- Learned why Linux is widely used in DevOps.
- Linux is stable, secure, lightweight, and powerful for servers.
- Most DevOps tools work very well with Linux-based systems.

### 4. Linux Filesystem
- Learned that everything in Linux is treated as a file or directory.
- Explored important directories like `/`, `/etc`, `/bin`, `/sbin`, `/mnt`, and `/var`.
- Understood that `/var/www/html` is commonly used to store default web files for NGINX.

### 5. Basic Linux Commands
Practiced basic Linux commands:

- `ls` - list files and directories
- `cd` - change directory
- `rmdir` - remove empty directory
- `rm -r` - remove directory with files
- `sudo` - run command with admin/root permissions
- `apt update` - update package information
- `apt upgrade` - upgrade installed packages
- `systemctl` - manage system services

### 6. Vim Basics
- Learned that Vim is a terminal-based text editor.
- Used `i` to enter insert mode.
- Used `Esc` to exit insert mode.
- Used `:wq` to save and quit.

### 7. NGINX Basics
- Learned that NGINX is a web server and reverse proxy server.
- Checked NGINX service using `systemctl`.
- Edited the default NGINX web page using Vim.

## Commands Practiced

```bash
ls
cd /var
cd /var/www/html
rmdir foldername
rm -r foldername
sudo apt update
sudo apt upgrade
sudo systemctl status nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo vim index.html