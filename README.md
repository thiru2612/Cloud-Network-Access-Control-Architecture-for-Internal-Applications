# Cloud Network & Access Control Architecture for Internal Applications (AWS)

This project demonstrates how to securely host an internal web application in AWS using proper network segmentation, firewall rules, and controlled administrative access.

The architecture simulates a real-world enterprise setup where:
- Employees exist inside an organization network
- The application is protected in a private subnet
- Administrative access is restricted and controlled
- Common security mistakes are demonstrated and corrected

---

## 📌 Problem Statement

Previously, systems were directly exposed to the internet without firewall rules, resulting in security incidents.  
The objective is to redesign the network using AWS best practices to balance **accessibility** and **security**.

---

## 🧠 Architecture Overview

```

Admin Laptop
|
| SSH
v
Public Subnet (Employees + Bastion)
|
| HTTP/HTTPS + SSH (private IP)
v
Private Subnet (Docker Web Application)

```

- Public Subnet → Simulates employee machines
- Private Subnet → Hosts internal web application
- Bastion Host → Used for secure admin access
- NAT Gateway → Allows private subnet outbound internet access
- Security Groups + NACL → Enforce firewall rules

---

## 🏗️ AWS Components Used

- VPC (10.0.0.0/16)
- Public Subnet (10.0.1.0/24)
- Private Subnet (10.0.2.0/24)
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- Network ACL
- EC2 Instances
- Docker & Docker Compose
- Nginx Web Server

---

## 🔒 Security Design

### Insecure Setup (Simulated)

A security group with:

| Type | Port | Source |
|------|------|--------|
| All Traffic | All | 0.0.0.0/0 |

This exposed the server to the entire internet.

---

### Secure Setup (Implemented)

#### Public EC2 (Employees / Bastion)

| Port | Source |
|------|--------|
| SSH (22) | Admin IP |
| All Traffic | 0.0.0.0/0 |

#### Private EC2 (Web App)

| Port | Source |
|------|--------|
| HTTP (80) | 10.0.1.0/24 |
| HTTPS (443) | 10.0.1.0/24 |
| SSH (22) | 10.0.1.0/24 |

---

## 🌐 Network Routing

### Public Route Table
```

0.0.0.0/0 → Internet Gateway

```

### Private Route Table
```

0.0.0.0/0 → NAT Gateway

```

Private instances have **outbound internet only**, no inbound access.

---

## 🐳 Web Application (Docker)

A simple Nginx web server is deployed inside the private EC2 using Docker Compose.

Employees from public subnet can access the application using:

```

curl [http://10.0.2.92](http://10.0.2.92)

```

Direct access from the internet is blocked.

---

## 🧪 Testing Performed

| Test | Result |
|------|--------|
| Laptop → Private EC2 | ❌ Blocked |
| Laptop → Public EC2 | ✅ SSH |
| Public EC2 → Private EC2 (HTTP) | ✅ Allowed |
| Internet → Private EC2 | ❌ Blocked |

---

## 🛠️ How to Reproduce

1. Create VPC and subnets
2. Attach Internet Gateway
3. Create NAT Gateway
4. Configure route tables
5. Launch EC2 instances
6. Configure Security Groups and NACL
7. SSH into private EC2 through public EC2
8. Install Docker and run Nginx container

---

## 📷 Architecture Diagram

Refer to the architecture diagram included in this repository.

---

## ✅ Security Best Practices Demonstrated

- Principle of Least Privilege
- Network Segmentation
- Bastion Host Access
- Defense in Depth (SG + NACL)
- Private Subnet Isolation
- Controlled Administrative Access
- Restricted Application Ports

---

## 📎 Key Learning Outcomes

- Why private subnets cannot access internet without NAT
- How bastion hosts are used in real environments
- Difference between Security Groups and NACL
- Importance of subnet segregation
- Real-world cloud network troubleshooting

---

## 👤 Author

Thiruppathi R
Cloud Network & Security college Project
