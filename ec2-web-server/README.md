# AWS Application Load Balancer with Auto Scaling

![AWS](https://img.shields.io/badge/AWS-Cloud-orange)
![EC2](https://img.shields.io/badge/AWS-EC2-blue)
![LoadBalancer](https://img.shields.io/badge/AWS-ALB-red)
![AutoScaling](https://img.shields.io/badge/AWS-AutoScaling-green)

---

# Project Overview

This project demonstrates how to build a **highly available and scalable web architecture on AWS** using:

* Amazon EC2
* Application Load Balancer
* Target Groups
* Auto Scaling Groups

The architecture distributes incoming traffic across multiple EC2 instances and ensures high availability.

---

# Architecture Diagram

```
                Internet
                    │
                    ▼
        Application Load Balancer (ALB)
                    │
                    ▼
               Target Group
                    │
         ┌──────────┴──────────┐
         ▼                     ▼
      EC2 Instance 1       EC2 Instance 2
   (Apache Web Server)  (Apache Web Server)
           │                    │
           └──── Auto Scaling Group ────┘
```

---

# AWS Services Used

| Service                   | Purpose                       |
| ------------------------- | ----------------------------- |
| EC2                       | Host web servers              |
| Application Load Balancer | Distribute traffic            |
| Target Group              | Manage EC2 instances          |
| Auto Scaling Group        | Automatically scale instances |
| Security Groups           | Control network access        |
| VPC                       | Networking                    |

---

# Step 1 – Launch EC2 Instance

EC2 instance was launched using Amazon Linux.

Install Apache Web Server

```
sudo dnf update -y
sudo dnf install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

Create test page

```
echo "<h1>Hello from Load Balancer Demo - Ayush</h1>" | sudo tee /var/www/html/index.html
```

---

# Step 2 – Create Target Group

Configuration:

```
Target type : Instance
Protocol : HTTP
Port : 80
Health Check Path : /
```

EC2 instances were registered to the target group.

---

# Step 3 – Create Application Load Balancer

Configuration:

```
Scheme : Internet Facing
Listener : HTTP (Port 80)
Forward to → Target Group
```

Load Balancer distributes traffic across EC2 instances.

---

# Step 4 – Configure Security Groups

Load Balancer Security Group

```
HTTP 80 → 0.0.0.0/0
```

EC2 Security Group

```
HTTP 80 → 0.0.0.0/0
SSH 22 → My IP
```

---

# Step 5 – Create Auto Scaling Group

Auto Scaling configuration:

```
Desired Capacity : 2
Minimum Capacity : 2
Maximum Capacity : 4
```

This ensures that the system automatically launches new instances if one fails.

---

# Troubleshooting Issues

## Issue 1 – Load Balancer DNS not working

Error:

```
ERR_CONNECTION_TIMED_OUT
```

Cause

Load Balancer security group did not allow HTTP traffic.

Fix

Added inbound rule

```
HTTP 80 → 0.0.0.0/0
```

---

## Issue 2 – Apache service inactive

Check status

```
sudo systemctl status httpd
```

Fix

```
sudo systemctl start httpd
```

---

## Issue 3 – SSH Permission Denied

Error

```
Permission denied (publickey)
```

Fix

```
ssh -i ai-key.pem ec2-user@public-ip
```

---

# Final Architecture

```
User
 │
Internet
 │
Application Load Balancer
 │
Target Group
 │
Auto Scaling Group
 │
EC2 Instances
 │
Apache Web Server
```

This architecture provides:

* High Availability
* Load Distribution
* Scalability
* Fault Tolerance

---

# Project Screenshots

## Load Balancer Created

![Load Balancer](screenshots/Load-balancer-DNS-working.png)

---

## Target Group Healthy

![Target Group](screenshots/Target-group-healthy.png)

---

## EC2 Instances Running

![EC2 Instances](screenshots/EC2-instances-running.png)

---

## Auto Scaling Group

![Auto Scaling](screenshots/Auto-scaling-group.png)

---

## Load Balancer Configuration

![Load Balancer Config](screenshots/Load-balancer-DNS-working.png)

---

# Author

Ayush Nath Motichur

Cloud & DevOps Enthusiast

