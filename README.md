<img src="https://github.com/user-attachments/assets/3c620b9b-91e0-4fc5-8553-d56427afcaf9" width="800" height="500">

## Project Overview

In this project, I have deployed a highly available and scalable multi-tier application using AWS. The architecture includes a Virtual Private Cloud (VPC), Application Load Balancer, Auto Scaling Groups, EC2 instances, NAT Gateway, Bastion Host, and RDS database.

This design follows a real-world production architecture where application components are distributed across multiple Availability Zones for high availability, security, and performance.

---

## Project Objectives

* Design and configure a VPC with public and private subnets
* Deploy EC2 instances using Auto Scaling
* Configure Application Load Balancer for traffic distribution
* Set up RDS database in private subnet
* Implement NAT Gateway and Bastion Host for secure access
* Achieve high availability using multiple Availability Zones

---

## Prerequisites

* AWS account (Free Tier recommended)
* Basic understanding of VPC, EC2, RDS
* Knowledge of networking (subnets, routing, security groups)

---

## Step 1: Create VPC

* CIDR Block: 10.0.0.0/16

### Subnets

* Public Subnet (AZ1): 10.0.1.0/24
* Public Subnet (AZ2): 10.0.2.0/24
* Private Subnet (App AZ1): 10.0.3.0/24
* Private Subnet (App AZ2): 10.0.4.0/24
* Private Subnet (DB AZ1): 10.0.5.0/24
* Private Subnet (DB AZ2): 10.0.6.0/24

---

## Step 2: Configure Internet Gateway

* Create Internet Gateway
* Attach to VPC
* Add route (0.0.0.0/0) to public route table

---

## Step 3: Configure NAT Gateway

* Create NAT Gateway in public subnet
* Attach Elastic IP
* Update private route table to route internet traffic via NAT

---

## Step 4: Launch Bastion Host

* Launch EC2 in public subnet
* Enable SSH (port 22)
* Used to access private instances securely

---

## Step 5: Create Application Load Balancer

* Type: Application Load Balancer
* Scheme: Internet-facing
* Subnets: Public subnets (AZ1 & AZ2)
* Listener: HTTP (80)

---

## Step 6: Create Launch Template

```
#!/bin/bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
echo "Welcome to Multi-Tier Auto Scaling App" > /var/www/html/index.html
```

---

## Step 7: Configure Auto Scaling Group

* Subnets: Private application subnets
* Desired Capacity: 2
* Min: 1
* Max: 3

### Scaling Policy

* Target CPU Utilization: 50%

---

## Step 8: Create RDS Database

* Engine: MySQL
* Deployment: Multi-AZ
* Subnets: Private DB subnets
* Security: Allow access only from EC2

---

## Step 9: Configure Security Groups

### EC2

* Allow HTTP (80) from Load Balancer
* Allow SSH (22) from Bastion Host

### RDS

* Allow MySQL (3306) from EC2 instances

---

## Step 10: Architecture Diagram

architecture.png

---

## Architecture Explanation

* Internet Gateway provides internet access
* Load Balancer distributes incoming traffic
* Auto Scaling Group manages EC2 instances dynamically
* EC2 instances are deployed in private subnets
* Bastion Host provides secure SSH access
* NAT Gateway allows outbound internet access
* RDS database is deployed in private subnet for security
* Multi-AZ setup ensures high availability

---

## Data Flow

1. User → Internet Gateway
2. → Application Load Balancer
3. → Auto Scaling EC2 instances
4. → Application processes request
5. → Connects to RDS database
6. → Response sent back to user

---

## Summary

* Designed a secure VPC architecture
* Deployed scalable EC2 instances using Auto Scaling
* Configured Load Balancer for traffic management
* Implemented RDS for backend storage
* Ensured high availability using Multi-AZ setup

---

## Conclusion

This project demonstrates a real-world cloud architecture that ensures scalability, security, and high availability. It reflects industry best practices for deploying production-ready applications on AWS.

---

## Note

This project was implemented using AWS Free Tier. Resources were terminated after testing to avoid additional charges.
