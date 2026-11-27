
## 📝 Project Overview
Implemented a centralized Nagios Core monitoring solution for OS infrastructure using NRPE-based remote monitoring. 
The solution provides proactive monitoring of servers, services, resources, and system health through configurable alerts and dashboards.
This configuration is based on Linux OS 

---
## 📌 Business Problem

Organizations require continuous monitoring of OS servers to ensure system availability, resource utilization visibility, and early detection of failures. 
Manual checks are time-consuming and reactive, often resulting in delayed incident response. 
This project implemented centralized monitoring to improve operational visibility and reduce downtime.

This approach leads to:

- Increased troubleshooting effort
- Delayed incident resolution
- Inconsistent health assessments
- Risk of missing performance bottlenecks
- Reduced operational efficiency
---

## 🎯 Objectives

The project aims to:

- Centralized Linux monitoring
- CPU monitoring
- Memory monitoring
- Disk monitoring
- Service monitoring
- NRPE integration
- Alerting and notifications
- Dashboard visibility
---

## 🏗️ Architecture
The architecture utilises Nagios Core, plugins, NRPE agents, and the web interface to monitor remote Linux systems

             +------------------+
             |   Nagios Core    |
             +------------------+
                      |
             +--------+--------+
             |                 |
        NRPE Agent        NRPE Agent
             |                 |
      Linux Server 1    Linux Server 2

---

## ⚙️ Features
- Nagios Core Installation
- NRPE Integration
- Linux Resource Monitoring
- CPU Checks
- Memory Checks
- Swap Checks
- Disk Checks
- SSH Monitoring
- Rsyslog Monitoring
- Chronyd Monitoring
- Host Groups
- Service Groups
- Dashboard Monitoring

The project includes checks for CPU, memory, swap, disk, Chrony, NRPE, Rsyslog, and SSH services

---

## ⚠️ <sub>Professional Disclaimer</sub>
<sub><i>This repository presents a sanitized and independently documented implementation of a Linux infrastructure monitoring solution created for portfolio and demonstration purposes.</i></sub>
<sub><i>All customer identifiers, hostnames, IP addresses, business information, and sensitive configuration details have been anonymized or replaced with sample values.</i></sub>
<sub><i>No proprietary information, confidential customer data, or intellectual property has been disclosed..</i></sub>

---


## 👤 Author
**Bhagyesh Prakash Vanarse**  
Senior Infrastructure Consultant | Linux Lead | DevOps  
🔗 [LinkedIn] - https://www.linkedin.com/in/bhagyeshv-devopslinux/

