# 🖧 Home Lab: DHCP & DNS Server with Ubuntu

This project demonstrates the design and implementation of a small home lab network using **Ubuntu Server** as a:

* DHCP Server (dynamic IP assignment)
* DNS Server (BIND9)
* Router (NAT for internet access)

The lab simulates a real-world internal network with centralized network services.

---

## 📌 Architecture

<p align="center">
  <img src="topology.png" alt="Network Topology" width="800"/>
</p>

---

## 🌐 Network Design

| Component     | Interface | IP Address    | Role               |
| ------------- | --------- | ------------- | ------------------ |
| Ubuntu Server | ens33     | 172.20.10.x   | Internet (Bridged) |
| Ubuntu Server | ens37     | 192.168.129.2 | DHCP, DNS, Gateway |
| Client VM     | ens33     | 192.168.129.x | DHCP Client        |

---

## ⚙️ Features

* Dynamic IP allocation using ISC DHCP Server
* Internal DNS resolution using BIND9
* Internet access via NAT (iptables)
* Multi-interface routing configuration
* Isolated internal network (Host-Only)

---
## 🔄 NAT Flow Explanation

The client sends traffic using a private IP (192.168.129.x), which cannot be routed on the internet.  
The Ubuntu server performs NAT (MASQUERADE), replacing the source IP with its public IP (172.20.10.x).

Flow:
1. Client → Gateway (Ubuntu Server)
2. NAT translates private IP → public IP
3. Internet responds to server
4. Server translates back → Client

## 🧰 Technologies Used

* Ubuntu Server (Linux)
* ISC DHCP Server
* BIND9 DNS Server
* iptables (NAT)
* VMware Workstation

---

## 📂 Project Structure

```text
.
├── configs/
│   ├── dhcp/
│   │   └── dhcpd.conf
│   ├── dns/
│   │   ├── named.conf.options
│   │   ├── named.conf.local
│   │   └── db.home.lab
│   └── netplan/
│       └── 00-installer-config.yaml
├── topology.png
├── README.md
├── 01-dhcp-server-status.png
├── 02-client-ip.png
├── 03-dns-server-status.png
├── ping-test.png
├── dns-nslookup-test.png
├── internet-reachable.png
├── internal-DNS-working.png
└── external-DNS-working.png
```

---

## 🚀 Setup Overview

### 1. Install Required Packages

```bash
sudo apt update
sudo apt install isc-dhcp-server bind9 -y
```

### 2. Configure Network Interfaces

Netplan configuration is located in:

```
configs/netplan/00-installer-config.yaml
```

---

### 3. Configure DHCP Server

```
configs/dhcp/dhcpd.conf
```

Defines IP range, gateway, and DNS server.

---

### 4. Configure DNS Server (BIND9)

```
configs/dns/
```

Includes:

* Forwarders (Google DNS / Cloudflare)
* Local domain (`home.lab`)
* Zone records

---

### 5. Enable Routing & NAT

```bash
sudo sysctl -w net.ipv4.ip_forward=1
sudo iptables -t nat -A POSTROUTING -o ens33 -j MASQUERADE
```

---

## 🧪 Testing & Verification

### ✅ DHCP Server Status

![DHCP](01-dhcp-server-status.png)

---

### ✅ Client IP Assignment

![Client IP](02-client-ip.png)

---

### ✅ DNS Server Status

![DNS Status](03-dns-server-status.png)

---

### ✅ Internet Connectivity (NAT Working)

![Ping](ping-test.png)

---

### ✅ DNS Resolution

![DNS](dns-nslookup-test.png)

---

### ✅ External DNS Working

![External DNS](External-DNS-working.png)

---

### ✅ Internal Domain Resolution

![Internal DNS](Internal-DNS-working.png)

---

## 🧠 What I Learned

* Configured a DHCP server for dynamic IP allocation
* Implemented DNS resolution using BIND9
* Set up NAT and routing using iptables
* Designed a multi-network Linux server
* Troubleshot network connectivity and DNS issues

---

## 🔥 Key Skills Demonstrated

* Linux system administration
* Networking fundamentals (DHCP, DNS, NAT)
* Network troubleshooting
* Infrastructure setup in virtual environments

---
