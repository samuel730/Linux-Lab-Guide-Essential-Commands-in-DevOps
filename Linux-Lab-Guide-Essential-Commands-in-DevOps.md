# Linux Lab Guide: Essential Commands in DevOps

## Project Overview

This project demonstrates fundamental Linux and DevOps skills using a virtual Ubuntu environment. It focuses on automation, system administration, scheduling tasks, and performance monitoring using Bash scripting and native Linux tools.

The lab simulates real-world DevOps workflows such as server setup automation, scheduled tasks, and continuous system monitoring.

---

## Author

**Name:** Samuel Adesanya  
**Course:** DevOps Fundamentals  
**Environment:** Ubuntu Virtual Machine (Vagrant / VirtualBox)

---

## Objectives

The main objectives of this lab are to:

- Compare Linux and Windows for DevOps usage
- Automate system configuration using Bash scripts
- Schedule tasks using cron jobs
- Monitor system performance in real time
- Log CPU and memory usage
- Understand background process management

---

## Tools & Technologies Used

- :contentReference[oaicite:0]{index=0}  
- Bash Shell Scripting  
- cron (job scheduler)  
- ufw firewall utility  
- htop system monitoring tool  
- nohup (background process execution)  
- Linux commands: top, ps, free, grep, awk  

---

## Project Structure
```
linux-devops-lab/
│
├── setup.sh # Automates system setup
├── monitor.sh # Monitors system performance
├── system_metrics.log # Generated log file
└── README.md # Project documentation
```

---

## Task 1: Linux vs Windows for DevOps

### Objective
Compare Linux and Windows operating systems for DevOps automation.

### Key Findings

#### 1. Scripting
- Linux: Bash scripting (powerful and widely used)
- Windows: PowerShell (less common in cloud environments)

#### 2. Package Management
- Linux: apt, yum, dnf
- Windows: winget, Chocolatey

#### 3. Container Support

Linux supports container tools natively such as:

- :contentReference[oaicite:1]{index=1}  
- :contentReference[oaicite:2]{index=2}  

Windows requires additional configuration for containers.

#### 4. Automation
- Linux uses cron jobs and shell scripts
- Windows uses Task Scheduler

### Conclusion
Linux is the preferred operating system for DevOps due to its flexibility, lightweight design, strong automation support, and cloud-native compatibility.
```
## Task 1: Choosing an OS for DevOps Automation

### Linux vs Windows for DevOps Automation

DevOps automation depends heavily on scripting flexibility, efficient package management, and robust container support. Both Linux and Windows can be used, but they differ significantly in how well they support modern automation workflows.

---

### 1. Scripting Abilities

Linux (especially distributions like Ubuntu, CentOS, and Debian) is built around powerful command-line tools and scripting languages such as Bash, Python, and Perl. Bash scripting is native and deeply integrated into the system, making it ideal for automating system tasks, deployments, and CI/CD pipelines.

Windows, on the other hand, relies on PowerShell for automation. PowerShell is powerful and object-oriented, but many DevOps tools and scripts in the industry are originally designed for Unix-like environments. While Windows Subsystem for Linux (WSL) improves compatibility, it still adds an extra layer rather than native support.

**Advantage: Linux**

---

### 2. Package Management

Linux has mature and efficient package managers such as `apt` (Debian/Ubuntu), `yum/dnf` (RHEL-based systems), and `pacman` (Arch Linux). These tools allow fast, scriptable installation and updates of software, making automation straightforward in CI/CD pipelines.

Windows uses tools like Chocolatey and Winget, but they are less standardized across environments. Dependency handling and scripting integration are improving but still not as seamless or widely adopted in enterprise DevOps workflows.

**Advantage: Linux**

---

### 3. Container Compatibility

Modern DevOps relies heavily on containerization technologies such as Docker and orchestration tools like Kubernetes.

Linux is the native environment for Docker containers. Most container images are Linux-based, and container performance is optimized on Linux kernels. Kubernetes clusters are also most commonly deployed on Linux nodes.

Windows supports containers as well, but primarily for Windows-based applications. It also depends on Linux VMs to run most container workloads, which adds overhead.

**Advantage: Linux**

---

### 4. Overall System Integration

Linux is designed as a multi-user, server-oriented operating system with strong networking, permissions control, and automation-friendly architecture. It integrates naturally into cloud environments (AWS, Azure, GCP) and is the dominant OS in server infrastructure.

Windows (Microsoft Windows), while strong in desktop and enterprise GUI environments, is less optimized for headless server automation without additional configuration layers.

---

### Conclusion

Overall, Linux is the better operating system for DevOps automation. It provides:

* Native and efficient scripting support (especially Bash and Python)
* Mature and consistent package management systems
* First-class support for Docker and Kubernetes ecosystems
* Strong compatibility with cloud and server environments

Windows is improving, especially with PowerShell and WSL, but it is still more complex and less standardized for automation-heavy DevOps workflows.

For these reasons, Linux remains the industry standard and preferred OS for DevOps engineers focused on automation, scalability, and containerized systems.
```
---

## Task 2: System Automation with Bash Script

### Objective
Automate system setup tasks using a Bash script.

### Script: `setup.sh`

```bash
#!/bin/bash

echo "Updating system packages..."
sudo apt-get update -y

echo "Installing firewall..."
sudo apt-get install -y ufw

echo "Enabling firewall..."
sudo ufw enable

echo "System setup completed successfully."
```

#### How to Run
```
chmod +x setup.sh
./setup.sh
```
#### Automate with Cron

Open cron editor:
```
crontab -e
```
Add:
```
0 0 * * * /home/vagrant/setup.sh
```
This schedules the script to run daily at midnight.

<img width="1477" height="893" alt="Screenshot 2026-05-25 122605" src="https://github.com/user-attachments/assets/81df207c-5ecf-4a08-89c3-074244e4689b" />

<img width="2560" height="1372" alt="Screenshot 2026-05-25 121704" src="https://github.com/user-attachments/assets/29809dd5-d213-4ac6-8d90-909cc524434f" />

<img width="1737" height="1306" alt="Screenshot 2026-05-25 121613" src="https://github.com/user-attachments/assets/d64fc6c1-419e-4e74-bfc6-c68a1747af58" />

<img width="1565" height="703" alt="Screenshot 2026-05-25 121523" src="https://github.com/user-attachments/assets/d37edeb3-c9bb-4ca6-9738-2a11b24a4dcd" />

<img width="2502" height="1367" alt="Screenshot 2026-05-25 121356" src="https://github.com/user-attachments/assets/de2cf0d7-7fb4-4de6-acad-edd9f27759c4" />

<img width="695" height="112" alt="Screenshot 2026-05-25 121316" src="https://github.com/user-attachments/assets/ea0d879f-348e-45d8-b1b8-eff998ef98f1" />

## Task 3: System Monitoring & Logging
### Objective

Monitor system performance and log CPU and memory usage.

Install Monitoring Tool
```
sudo apt-get install htop -y
```
Run:
```
htop
```
Script: monitor.sh
```
#!/bin/bash

while true; do
    echo "$(date) - CPU: $(top -bn1 | grep 'Cpu(s)' | awk '{print $2 + $4}')% | MEM: $(free | awk '/Mem/{printf "%.2f", $3/$2 * 100.0}')" >> system_metrics.log

    sleep 60
done
```
Run in Background
```
chmod +x monitor.sh
nohup ./monitor.sh > /dev/null 2>&1 &
```
View Logs
```
cat system_metrics.log
```
Live monitoring:
```
tail -f system_metrics.log
```
Sample Output
```
Mon May 25 10:07:08 UTC - CPU: 14.8% | MEM: 16.57
Mon May 25 10:08:08 UTC - CPU: 12.8% | MEM: 16.60
Mon May 25 10:09:08 UTC - CPU: 11.2% | MEM: 16.55
```

<img width="1464" height="77" alt="Screenshot 2026-05-25 122927" src="https://github.com/user-attachments/assets/b8de84cd-5efe-4657-bca1-a856c223c878" />

<img width="1856" height="126" alt="Screenshot 2026-05-25 122911" src="https://github.com/user-attachments/assets/d4a98781-fbb1-45e5-88e6-330fc5afcc1d" />

<img width="1129" height="88" alt="Screenshot 2026-05-25 122852" src="https://github.com/user-attachments/assets/285de8cf-126a-4f85-9a8c-ad49581dae81" />

<img width="1346" height="394" alt="Screenshot 2026-05-25 122837" src="https://github.com/user-attachments/assets/d2b8235a-c19a-4a9a-abe6-073308084dfa" />

<img width="1895" height="272" alt="Screenshot 2026-05-25 122825" src="https://github.com/user-attachments/assets/5baceefc-f2d7-4c50-a9a6-305d96ea4b24" />

<img width="945" height="78" alt="Screenshot 2026-05-25 122813" src="https://github.com/user-attachments/assets/e5259124-db93-48cd-a23b-c7b03fba8655" />

<img width="741" height="42" alt="Screenshot 2026-05-25 122759" src="https://github.com/user-attachments/assets/aae63d2a-874b-49eb-a713-5c3a6bc3fe14" />

<img width="2557" height="764" alt="Screenshot 2026-05-25 122738" src="https://github.com/user-attachments/assets/bd32c191-6d32-4c4f-a89b-d4142584db6a" />

<img width="795" height="62" alt="Screenshot 2026-05-25 122725" src="https://github.com/user-attachments/assets/388489c4-ec30-49a1-9530-0454a19a4014" />

<img width="2549" height="1365" alt="Screenshot 2026-05-25 122654" src="https://github.com/user-attachments/assets/7e931d58-2e23-44f1-aa8a-acf8fe715f8a" />

<img width="525" height="35" alt="Screenshot 2026-05-25 122635" src="https://github.com/user-attachments/assets/3150b61d-74b7-4bd5-94bf-c9bd1ddc5263" />

<img width="1295" height="268" alt="Screenshot 2026-05-25 122620" src="https://github.com/user-attachments/assets/0a59df10-1acc-43eb-803d-ea02dcc19ecf" />

## Key Learnings
- Linux is highly efficient for DevOps automation
- Bash scripting simplifies system administration
- Cron enables scheduled automation
- System monitoring tools provide real-time insights
- Background processes allow continuous execution

## Challenges Faced
- Understanding cron syntax
- Managing background processes
- Extracting CPU and memory metrics
- Debugging script execution issues

All challenges were resolved through testing and debugging.

## Conclusion

This project demonstrates essential DevOps practices using Linux. It shows how system tasks can be automated, scheduled, and monitored using native Linux tools.

Linux is preferred in DevOps environments due to:

- Strong automation capabilities
- Lightweight system performance
- Cloud-native compatibility
- Native support for containers and orchestration tools
