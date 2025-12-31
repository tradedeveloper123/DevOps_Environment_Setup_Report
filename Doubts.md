
````markdown
# DevOps Environment Setup & Management

## Project Overview
This project demonstrates the implementation of a secure, monitored, and well-maintained Linux development environment as per organizational DevOps standards.

The setup includes:
- System monitoring and logging
- User management with access control
- Password security enforcement
- Automated backup configuration for web servers

### User Mapping
> **Sarah → kailash (Apache Administrator)**  
> **Mike → boogeyman (Nginx Administrator)**

---

## System Details
- **Operating System:** RHEL / Amazon Linux (YUM-based)
- **Monitoring Tools:** htop, df, du, ps
- **Security:** PAM, libpwquality
- **Backup Automation:** Cron, tar
- **Shell:** Bash

---

# Task 1: System Monitoring Setup

## Objective
To monitor CPU, memory, disk usage, and running processes in order to detect performance issues and support capacity planning.

 <img width="1920" height="826" alt="image" src="https://github.com/user-attachments/assets/9783ecd6-19db-4414-a637-6ffb31a9044a" />
<img width="1918" height="817" alt="image" src="https://github.com/user-attachments/assets/d40205d7-b42e-4ef9-9cc7-01e06c9c2295" />

---

## 1.1 Install Monitoring Tool (htop)

### Step 1: Update System Packages
```bash
sudo yum update -y
````

**Purpose:**
Ensures the system is running the latest security patches and dependencies.
 <img width="1920" height="826" alt="image" src="https://github.com/user-attachments/assets/9783ecd6-19db-4414-a637-6ffb31a9044a" />
<img width="1918" height="817" alt="image" src="https://github.com/user-attachments/assets/d40205d7-b42e-4ef9-9cc7-01e06c9c2295" />
<img width="1919" height="680" alt="image" src="https://github.com/user-attachments/assets/61132330-e907-4233-80eb-c1a2dcbdb129" />

---

### Step 2: Install htop

```bash
sudo yum install htop -y
```

**Purpose:**
Installs `htop`, an interactive process viewer used for real-time system monitoring.

<img width="1918" height="495" alt="image" src="https://github.com/user-attachments/assets/9f5f42d6-f39e-4a3f-b959-e4153426ecae" />
<img width="1920" height="712" alt="image" src="https://github.com/user-attachments/assets/997b07eb-0b7e-4cc7-b582-da94a898f718" />

### htop Provides Visibility Into:

* CPU usage
* Memory consumption
* Running processes
* Load average

---

### Step 3: Launch htop

```bash
htop
```

<img width="1919" height="402" alt="image" src="https://github.com/user-attachments/assets/578e326b-21cd-4bd4-8deb-e84a68510e7a" />
<img width="1920" height="791" alt="image" src="https://github.com/user-attachments/assets/71e8224b-7239-4657-9082-da24afe7333b" />

---

## 1.2 Disk Usage Monitoring

### Check Overall Disk Usage

```bash
df -h
```

**Purpose:**
Displays total, used, and available disk space in a human-readable format.

<img width="1920" height="476" alt="image" src="https://github.com/user-attachments/assets/d31cc672-4125-456f-beee-4ef2b0b95bf5" />
<img width="1917" height="409" alt="image" src="https://github.com/user-attachments/assets/5ddb58fe-70c4-46c3-a818-4fe396fa230f" />

---

### Check Directory-wise Disk Usage

```bash
du -sh /var/*
```

**Purpose:**
Identifies directories consuming the most disk space.

<img width="1920" height="342" alt="image" src="https://github.com/user-attachments/assets/82f1db43-2625-42bc-9419-b9918303fd44" />
<img width="1920" height="572" alt="image" src="https://github.com/user-attachments/assets/e77cf2ae-d638-48f2-8c59-5bc0b0f87903" />

---

## 1.3 Identify Resource-Intensive Processes

```bash
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head
```

**Purpose:**
Lists the top CPU-consuming processes to help identify performance bottlenecks.

<img width="1920" height="425" alt="image" src="https://github.com/user-attachments/assets/a30ee582-bb15-4f69-982a-c035098920ef" />
<img width="1920" height="496" alt="image" src="https://github.com/user-attachments/assets/95943fe5-548e-4ec8-bca3-6c7012c4056d" />

---

## 1.4 Logging System Metrics

### Create Log Directory

```bash
sudo mkdir -p /var/log/system-monitor
```

<img width="1920" height="304" alt="image" src="https://github.com/user-attachments/assets/9f6fccc5-0633-4030-b472-75001956cc54" />
<img width="1920" height="713" alt="image" src="https://github.com/user-attachments/assets/c2a418c6-e4ec-4b35-9c62-d87197b6fdd9" />

---

### Create Monitoring Script

```bash
sudo vi /usr/local/bin/system_monitor.sh
```

```bash
#!/bin/bash
echo "===== $(date) =====" >> /var/log/system-monitor/monitor.log
df -h >> /var/log/system-monitor/monitor.log
ps -eo pid,cmd,%cpu,%mem --sort=-%cpu | head >> /var/log/system-monitor/monitor.log
```

**Purpose:**
Logs disk usage and top resource-consuming processes for auditing and troubleshooting.

<img width="1920" height="280" alt="image" src="https://github.com/user-attachments/assets/32a6049a-d9a3-4b38-b691-aa4b7618aef3" />
<img width="1919" height="312" alt="image" src="https://github.com/user-attachments/assets/0fe929f8-56e6-4891-8c01-1a64fd877f93" />

---

### Make Script Executable

```bash
sudo chmod +x /usr/local/bin/system_monitor.sh
```

<img width="1920" height="353" alt="image" src="https://github.com/user-attachments/assets/18e3969a-8398-4000-963b-1975084e0157" />

---

### Schedule Logging Every Hour

```bash
crontab -e
```

```bash
0 * * * * /usr/local/bin/system_monitor.sh
```

<img width="1920" height="252" alt="image" src="https://github.com/user-attachments/assets/39f04d11-2898-4233-bd71-221af124adaf" />
<img width="1920" height="389" alt="image" src="https://github.com/user-attachments/assets/c2194504-9f4e-4c57-a3b6-9e41a3e73812" />
<img width="1918" height="276" alt="image" src="https://github.com/user-attachments/assets/1751c6ba-8896-4edb-9b80-cd5a5a1d0649" />

---

# Task 2: User Management and Access Control

## Objective

To create secure user accounts with isolated workspaces and enforce strong password policies.

---

## 2.1 Create User Accounts

```bash
sudo useradd kailash
sudo useradd boogeyman
```

```bash
sudo passwd kailash
sudo passwd boogeyman
```

**Purpose:**
Creates local user accounts and assigns secure passwords.

<img width="1920" height="324" alt="image" src="https://github.com/user-attachments/assets/2a455a85-58a4-4d1c-ae29-7fe21a8d86eb" />
<img width="1920" height="426" alt="image" src="https://github.com/user-attachments/assets/43c435eb-13d1-443d-a32e-e0e153a87bff" />

---

## 2.2 Create Isolated Workspaces

```bash
sudo mkdir -p /home/kailash/workspace
sudo mkdir -p /home/boogeyman/workspace
```

```bash
sudo chown -R kailash:kailash /home/kailash/workspace
sudo chown -R boogeyman:boogeyman /home/boogeyman/workspace
```

```bash
sudo chmod 700 /home/kailash/workspace
sudo chmod 700 /home/boogeyman/workspace
```

**Purpose:**
Ensures only the respective user can access their workspace.

---

## 2.3 Enforce Password Policy

### Install Password Quality Module

```bash
sudo yum install libpwquality -y
```

```bash
rpm -qa | grep pwquality
```

---

### Configure Password Complexity

```bash
sudo vi /etc/security/pwquality.conf
```

```ini
minlen = 8
dcredit = -1
ucredit = -1
lcredit = -1
ocredit = -1
```

---

### Enforce Strict PAM Rules

```bash
sudo vi /etc/pam.d/system-auth
sudo vi /etc/pam.d/password-auth
```

```text
password requisite pam_pwquality.so try_first_pass local_users_only retry=3 enforce_for_root
```

---

## 2.4 Enforce Password Expiry

```bash
sudo chage -M 30 kailash
sudo chage -M 30 boogeyman
```

```bash
chage -l kailash
chage -l boogeyman
```

**Purpose:**
Forces password change every 30 days.

---

# Task 3: Backup Configuration for Web Servers

## Objective

To automate weekly backups for Apache and Nginx servers.

---

## 3.1 Create Backup Directory

```bash
sudo mkdir /backups
sudo chmod 755 /backups
```

---

## 3.2 Apache Backup Script (kailash)

```bash
sudo vi /usr/local/bin/apache_backup.sh
```

```bash
#!/bin/bash
DATE=$(date +%F)
tar -czf /backups/apache_backup_$DATE.tar.gz /etc/httpd /var/www/html
tar -tzf /backups/apache_backup_$DATE.tar.gz >> /backups/apache_verify.log
```

```bash
sudo chmod +x /usr/local/bin/apache_backup.sh
```

---

## 3.3 Nginx Backup Script (boogeyman)

```bash
sudo vi /usr/local/bin/nginx_backup.sh
```

```bash
#!/bin/bash
DATE=$(date +%F)
tar -czf /backups/nginx_backup_$DATE.tar.gz /etc/nginx /usr/share/nginx/html
tar -tzf /backups/nginx_backup_$DATE.tar.gz >> /backups/nginx_verify.log
```

```bash
sudo chmod +x /usr/local/bin/nginx_backup.sh
```

---

## 3.4 Schedule Cron Jobs

```bash
sudo crontab -e
```

```bash
0 0 * * 2 /usr/local/bin/apache_backup.sh
0 0 * * 2 /usr/local/bin/nginx_backup.sh
```

**Note:**
Cron jobs were temporarily modified to run every minute for testing purposes.

```bash
* * * * * /usr/local/bin/apache_backup.sh
```

---

## Conclusion

This project successfully implements:

* Real-time system monitoring
* Secure user isolation
* Enterprise-grade password policies
* Automated and verified backups


---






