Task 1: System Monitoring Setup
To monitor CPU, memory, disk usage, and running processes in order to detect performance issues and support capacity planning
<img width="1920" height="826" alt="image" src="https://github.com/user-attachments/assets/9783ecd6-19db-4414-a637-6ffb31a9044a" />
<img width="1918" height="817" alt="image" src="https://github.com/user-attachments/assets/d40205d7-b42e-4ef9-9cc7-01e06c9c2295" />

***Install Monitoring Tools (htop)
Step 1: Update system packages
sudo yum update -y
<img width="1919" height="680" alt="image" src="https://github.com/user-attachments/assets/61132330-e907-4233-80eb-c1a2dcbdb129" />
Step 2: Install htop
sudo yum install htop -y
<img width="1918" height="495" alt="image" src="https://github.com/user-attachments/assets/9f5f42d6-f39e-4a3f-b959-e4153426ecae" />
<img width="1920" height="712" alt="image" src="https://github.com/user-attachments/assets/997b07eb-0b7e-4cc7-b582-da94a898f718" />
htop provides a real-time view of:
CPU usage
Memory consumption
Running processes
Load average
Step 3: Launch htop
htop

<img width="1919" height="402" alt="image" src="https://github.com/user-attachments/assets/578e326b-21cd-4bd4-8deb-e84a68510e7a" />
<img width="1920" height="791" alt="image" src="https://github.com/user-attachments/assets/71e8224b-7239-4657-9082-da24afe7333b" />
1.2 Disk Usage Monitoring
Check overall disk usage
df -h

<img width="1920" height="476" alt="image" src="https://github.com/user-attachments/assets/d31cc672-4125-456f-beee-4ef2b0b95bf5" />
<img width="1917" height="409" alt="image" src="https://github.com/user-attachments/assets/5ddb58fe-70c4-46c3-a818-4fe396fa230f" />

Check directory-wise usage
du -sh /var/*

<img width="1920" height="342" alt="image" src="https://github.com/user-attachments/assets/82f1db43-2625-42bc-9419-b9918303fd44" />
<img width="1920" height="572" alt="image" src="https://github.com/user-attachments/assets/e77cf2ae-d638-48f2-8c59-5bc0b0f87903" />

1.3 Identify Resource-Intensive Processes
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head
<img width="1920" height="425" alt="image" src="https://github.com/user-attachments/assets/a30ee582-bb15-4f69-982a-c035098920ef" />

<img width="1920" height="496" alt="image" src="https://github.com/user-attachments/assets/95943fe5-548e-4ec8-bca3-6c7012c4056d" />

Logging System Metrics
 1.Create log directory
  sudo mkdir -p /var/log/system-monitor
<img width="1920" height="304" alt="image" src="https://github.com/user-attachments/assets/9f6fccc5-0633-4030-b472-75001956cc54" />
<img width="1920" height="713" alt="image" src="https://github.com/user-attachments/assets/c2a418c6-e4ec-4b35-9c62-d87197b6fdd9" />

Create monitoring script
sudo vi /usr/local/bin/system_monitor.sh
<img width="1913" height="697" alt="image" src="https://github.com/user-attachments/assets/ea5228cd-6164-4f7a-8228-80e59d54f98f" />
#!/bin/bash
echo "===== $(date) =====" >> /var/log/system-monitor/monitor.log
df -h >> /var/log/system-monitor/monitor.log
ps -eo pid,cmd,%cpu,%mem --sort=-%cpu | head >> /var/log/system-monitor/monitor.log

<img width="1920" height="280" alt="image" src="https://github.com/user-attachments/assets/32a6049a-d9a3-4b38-b691-aa4b7618aef3" />
<img width="1919" height="312" alt="image" src="https://github.com/user-attachments/assets/0fe929f8-56e6-4891-8c01-1a64fd877f93" />
Make script executable
sudo chmod +x /usr/local/bin/system_monitor.sh
<img width="1920" height="353" alt="image" src="https://github.com/user-attachments/assets/18e3969a-8398-4000-963b-1975084e0157" />

Schedule logging every hour
crontab -e
<img width="1920" height="252" alt="image" src="https://github.com/user-attachments/assets/39f04d11-2898-4233-bd71-221af124adaf" />
<img width="1920" height="389" alt="image" src="https://github.com/user-attachments/assets/c2194504-9f4e-4c57-a3b6-9e41a3e73812" />
<img width="1918" height="276" alt="image" src="https://github.com/user-attachments/assets/1751c6ba-8896-4edb-9b80-cd5a5a1d0649" />


Task 2: User Management and Access Control
To create secure user accounts with proper access isolation and password policies
2.1 Create User Accounts
sudo useradd kailash
<img width="1920" height="324" alt="image" src="https://github.com/user-attachments/assets/2a455a85-58a4-4d1c-ae29-7fe21a8d86eb" />
sudo useradd boogeyman
<img width="1920" height="426" alt="image" src="https://github.com/user-attachments/assets/43c435eb-13d1-443d-a32e-e0e153a87bff" />
Set passwords
sudo passwd kailash
<img width="1920" height="365" alt="image" src="https://github.com/user-attachments/assets/4dce51b9-3c73-410d-9adc-eb1926a776cc" />
<img width="1920" height="431" alt="image" src="https://github.com/user-attachments/assets/5a486b41-6e2d-46d2-a686-cdfb40e80594" />

sudo passwd boogeyman
<img width="1920" height="386" alt="image" src="https://github.com/user-attachments/assets/a9c50eb9-2332-4b4c-85b8-1e51570717d0" />
<img width="1920" height="410" alt="image" src="https://github.com/user-attachments/assets/8ac56a26-eb07-4f62-b578-eed9cd5390d8" />

2.2 Create Isolated Working Directories
sudo mkdir -p /home/kailash/workspace
<img width="1920" height="344" alt="image" src="https://github.com/user-attachments/assets/cd8d47fe-c15c-4dd5-a2e0-010320277190" />
sudo mkdir -p /home/boogeyman/workspace
<img width="1920" height="454" alt="image" src="https://github.com/user-attachments/assets/5aedd211-f985-458c-8cb0-c4b9bd1e7fb4" />
Assign ownership





















