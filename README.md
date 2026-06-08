# 🛡️ Personal Cybersecurity Lab — Task 2
### Maincrafts Technology Internship

![Kali Linux](https://img.shields.io/badge/Kali_Linux-2025.4-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![VMware](https://img.shields.io/badge/VMware-Workstation-607078?style=for-the-badge&logo=vmware&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Juice_Shop-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![OWASP](https://img.shields.io/badge/OWASP-Juice_Shop-orange?style=for-the-badge)

---

## 📌 Objective

Build a **safe and isolated cybersecurity practice environment** using virtualization, consisting of:

- 🖥️ **Attacker Machine** — Kali Linux 2025.4 (VMware)
- 🎯 **Target Application** — OWASP Juice Shop (Docker)
- 🔒 **Network Segmentation** — NAT + Host-Only adapters

---

## 🏗️ Lab Architecture

```
+--------------------------------------------------+
|           Your Computer (Host System)            |
|                                                  |
|   └── VMware Workstation                         |
|         ├── Kali Linux 2025.4  (Attacker)        |
|         │     eth0 → NAT       192.168.61.129    |
|         │     eth1 → Host-Only 192.168.85.128    |
|         │     docker0          172.17.0.1        |
|         │                                        |
|         └── OWASP Juice Shop   (Target)          |
|               Docker Container 172.17.0.2:3000   |
|                                                  |
|   Networks:                                      |
|     • NAT       → Internet (updates only)        |
|     • Host-Only → Isolated lab traffic           |
+--------------------------------------------------+
```

---

## ⚙️ Prerequisites

| Requirement | Specification |
|---|---|
| RAM | 8GB minimum (16GB recommended) |
| Disk Space | 40–60 GB free |
| CPU | Virtualization enabled (VT-x / AMD-V) |
| OS | Windows / Mac / Linux |
| Virtualization | VMware Workstation / VirtualBox |

---

## 🛠️ Tools Used

| Tool | Version | Purpose |
|---|---|---|
| VMware Workstation | Latest | Virtualization platform |
| Kali Linux | 2025.4 AMD64 | Attacker machine |
| Docker | Latest | Container engine |
| OWASP Juice Shop | v17+ | Vulnerable target application |
| Nmap | 7.95 | Network scanning & host discovery |
| Burp Suite Community | v2025.10.6 | HTTP traffic interception |
| Wireshark | 4.6.0 | Packet capture & analysis |

---

## 📋 Step-by-Step Setup

### Step 1 — Install VMware and Configure Networks
- Install VMware Workstation
- Configure VMnet1 (Host-Only) and VMnet8 (NAT) virtual networks

### Step 2 — Deploy Kali Linux (Attacker VM)
- Download Kali Linux VMware image from [kali.org](https://www.kali.org/get-kali/#kali-virtual-machines)
- Import into VMware, assign 2 CPUs and 3–4GB RAM
- Set Adapter 1 → NAT, Adapter 2 → Host-Only

```bash
# Fix Kali mirror if needed
echo "deb https://kali.download/kali kali-rolling main non-free contrib" | sudo tee /etc/apt/sources.list
sudo apt clean && sudo apt update

# Create working directories
mkdir -p ~/lab/{scans,notes,pcaps,screenshots}
```

### Step 3 — Deploy OWASP Juice Shop (Target)

```bash
# Install Docker
sudo apt install -y docker.io
sudo systemctl enable --now docker

# Pull and run Juice Shop
sudo docker pull bkimminich/juice-shop
sudo docker run -d -p 3000:3000 --name juice bkimminich/juice-shop
```

Access Juice Shop at: `http://localhost:3000`

### Step 4 — Verify Connectivity

```bash
# Host discovery scan
nmap -sn 192.168.85.0/24

# Service detection on localhost
nmap -sV localhost

# Ping validation
ping -c 4 192.168.85.1
ping -c 4 172.17.0.2
```

### Step 5 — Tool Validation

```bash
# Launch Burp Suite
burpsuite &

# Launch Wireshark (capture on docker0)
sudo -E wireshark
```

---

## 📸 Evidence

| # | Deliverable | Description |
|---|---|---|
| 1 | VM List | VMware showing Kali Linux VM running |
| 2 | IP Configuration | `ip addr` output — eth0 (NAT) + eth1 (Host-Only) |
| 3 | Ping Validation | Successful ping to gateway and Docker container |
| 4 | Nmap Scan | Host discovery + service detection output |
| 5 | Burp Suite Intercept | HTTP request intercepted from Juice Shop |
| 6 | Burp HTTP History | Full request/response log |
| 7 | Wireshark Capture | HTTP packets on docker0 interface |

> 📁 All screenshots are in the `/Evidence` folder

---

## 📁 Repository Structure

```
📦 cybersecurity-lab-task2/
├── 📄 README.md
├── 📄 CybersecurityLab_Task2_Report.pdf
└── 📁 Evidence/
    ├── 1_VM_List.png
    ├── 2_IP_Configuration.png
    ├── 3_Ping_Validation.png
    ├── 4_Nmap_Scan.png
    ├── 5_Burp_Intercept.png
    ├── 6_Burp_HTTP_History.png
    └── 7_Wireshark_Capture.png
```

---

## 🔍 Key Findings

- Successfully deployed isolated dual-network lab environment
- OWASP Juice Shop running on Docker container at `172.17.0.2:3000`
- Nmap discovered 3 active hosts on the Host-Only subnet (`192.168.85.0/24`)
- Burp Suite successfully intercepted HTTP traffic from Juice Shop
- Wireshark captured 327 packets (100 HTTP) on the `docker0` interface
- Kali mirror issue resolved by switching to `kali.download` CDN

---

## 📚 Resources

| Topic | Link |
|---|---|
| Kali Linux VM Download | https://www.kali.org/get-kali/#kali-virtual-machines |
| OWASP Juice Shop | https://github.com/juice-shop/juice-shop |
| DVWA (Alternative Target) | https://github.com/digininja/DVWA |
| Burp Suite Docs | https://portswigger.net/burp/documentation/desktop |
| Wireshark Docs | https://www.wireshark.org/docs/ |
| Nmap Reference | https://nmap.org/book/man.html |

---

## ✅ Deliverables Checklist

- [x] Kali Linux VM deployed and configured
- [x] Dual network adapters (NAT + Host-Only) verified
- [x] OWASP Juice Shop running via Docker
- [x] Nmap scan completed
- [x] Ping validation completed
- [x] Burp Suite traffic interception demonstrated
- [x] Wireshark packet capture completed
- [x] Full Lab Build PDF Report created
- [x] Evidence folder with all screenshots

---

<div align="center">

**Maincrafts Technology Internship — Cybersecurity & Ethical Hacking**

*Built with 🔒 for learning purposes only. All testing performed in an isolated lab environment.*

</div>
