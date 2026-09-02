# Assignment 5 — Deploy a Highly Available Two-Tier Application on AWS

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will design and deploy a highly available two-tier web application on AWS: highly available networking across two Availability Zones, an Application Load Balancer, an Auto Scaling Group for the web tier, and a private Multi-AZ RDS database. You must prove high availability with real failure tests.

---

# Task 1 — Create HA Networking (VPC + 4 Subnets + IGW + NAT + Route Tables)

## Goal

Build a VPC (10.0.0.0/16) with two public and two private subnets across two Availability Zones, an Internet Gateway, a NAT Gateway, and the matching public/private route tables.

### Evidence

#### Screenshot 1 — VPC details showing CIDR 10.0.0.0/16

![](screenshots/W6A5T1S1.png)

---

#### Screenshot 2 — Subnets list showing four subnets and their Availability Zones

![](screenshots/W6A5T1S2.png)

---

#### Screenshot 3 — Public route table showing the Internet Gateway route and both public-subnet associations

![](screenshots/W6A5T1S3.png)

---

#### Screenshot 4 — Private route table showing the NAT Gateway route and both private-subnet associations

![](screenshots/W6A5T1S4.png)

---

#### Screenshot 5 — NAT Gateway status showing Available and the Elastic IP

![](screenshots/W6A5T1S5.png)

---

# Task 2 — Create Security Groups (ALB, EC2, RDS) with Least Privilege

## Goal

Create `ha-alb-sg` (HTTP public), `ha-web-sg` (HTTP only from `ha-alb-sg`, SSH from your IP), and `ha-db-sg` (database port only from `ha-web-sg`).

### Evidence

#### Screenshot 6 — ALB Security Group inbound rules

![](screenshots/W6A5T2S6.png)

---

#### Screenshot 7 — EC2 Security Group inbound rules showing the ALB Security Group reference and SSH from your IP

![](screenshots/W6A5T2S7.png)

---

#### Screenshot 8 — RDS Security Group inbound rule showing the database port allowed only from the EC2 Security Group

![](screenshots/W6A5T2S8.png)

---

# Task 3 — Deploy Database Tier (RDS Multi-AZ in Private Subnets)

## Goal

Launch a private, Multi-AZ RDS database (MySQL or PostgreSQL) using the private DB Subnet Group and `ha-db-sg`.

### Evidence

#### Screenshot 9 — RDS summary showing Multi-AZ = Yes and Publicly accessible = No

![](screenshots/W6A5T3S9.png)

---

#### Screenshot 10 — RDS connectivity section showing the DB Subnet Group and Security Group

![](screenshots/W6A5T3S10.png)

---

# Task 4 — Build a Launch Template (User Data Installs App + Connects to DB)

## Goal

Create a Launch Template whose user data installs the web-server runtime, deploys the application, configures the database connection, and starts the required services.

### Evidence

#### Screenshot 11 — Launch Template details showing that user data exists, including a visible snippet

![](screenshots/W6A5T4S11.png)

---

#### Screenshot 12 — A running instance created from the template showing the application responds on port 80

![](screenshots/W6A5T4S12.png)

---

# Task 5 — Create an Application Load Balancer (ALB) Across 2 Public Subnets

## Goal

Create an internet-facing ALB across both public subnets with an HTTP listener and a healthy instance target group.

### Evidence

#### Screenshot 13 — ALB details showing two public subnets in two Availability Zones

![](screenshots/W6A5T5S13.png)

---

#### Screenshot 14 — Target group showing at least one healthy target

![](screenshots/W6A5T5S14.png)

---

# Task 6 — Create Auto Scaling Group (ASG) in 2 Public Subnets

## Goal

Create an Auto Scaling Group from the Launch Template across both public subnets, with desired capacity 2, minimum 2, and maximum 4, registered to the ALB target group.

### Evidence

#### Screenshot 15 — Auto Scaling Group showing desired, minimum, and maximum capacity and the selected subnet Availability Zones

![](screenshots/W6A5T6S15.png)

---

#### Screenshot 16 — EC2 instances list showing two running instances in different Availability Zones

![](screenshots/W6A5T6S16.png)

---

# Task 7 — Configure App to Use RDS + Validate Read/Write

## Goal

Confirm the application communicates with the RDS database through the ALB DNS name with at least one read and one write operation.

### Evidence

#### Screenshot 17 — Browser showing the application loaded through the ALB DNS name with the URL visible

![](screenshots/W6A5T7S17.png)

---

#### Screenshot 18 — Proof of a database write through a UI message or database query output

![](screenshots/W6A5T7S18.png)

---

# Task 8 — High Availability Tests (Must Do Both)

## Goal

Test A: terminate one web instance and confirm the Auto Scaling Group replaces it automatically without interrupting the ALB. Test B: simulate an Availability Zone impact (stop, detach, or reduce desired capacity in one AZ) and confirm the application stays available.

### Evidence

#### Screenshot 19 — EC2 showing the terminated instance and the newly launched instance

![](screenshots/W6A5T8S19.png)

---

#### Screenshot 20 — Target group showing healthy targets after replacement

![](screenshots/W6A5T8S20.png)

---

#### Screenshot 21 — Evidence that an instance was removed, detached, placed in Standby, or stopped in one Availability Zone

![](screenshots/W6A5T8S21.png)

---

#### Screenshot 22 — Browser showing that the ALB DNS endpoint still works during the change

![](screenshots/W6A5T8S22.png)

---

# Task 9 — Architecture and Test-Results Summary

## Goal

Summarize the VPC/subnet layout, the ALB and Auto Scaling Group setup, the private Multi-AZ RDS setup, and the results of both high-availability tests.

### Evidence

#### Screenshot 23 — A simple architecture diagram (hand-drawn is fine), or an AWS console overview showing the components

![](screenshots/W6A5T9S23.png)

---

### Notes

Write a short summary covering the network, ALB/ASG setup, RDS setup, and the results of Test A and Test B.

Network
The environment is built on a custom VPC (ha-vpc) spanning two Availability Zones (us-east-2a and us-east-2b) in us-east-2, each with a dedicated public subnet. Both subnets route outbound traffic through a single attached Internet Gateway (ha-igw), and share a common route table (public-rt) with a 0.0.0.0/0 → igw route. Security is enforced at two layers: ha-alb-sg allows public HTTP (80) traffic into the load balancer, while ha-web-sg is locked down to only accept HTTP from the ALB's security group (plus SSH from an admin IP for troubleshooting) — enforcing a strict internet → ALB → EC2 → RDS access chain.

ALB / ASG
An Application Load Balancer (ha-alb) is deployed internet-facing across both public subnets, listening on HTTP:80 and forwarding to a target group (ha-web-tg) using instance targets and a / health check path. Behind it, an Auto Scaling Group (ha-asg) — built from a Launch Template (HA-WEB-Launch-Template) — maintains a desired capacity of 2 instances split across both AZs. The launch template's user data script fully self-configures each new instance: installing Apache/PHP, deploying WordPress, and wiring up the RDS connection automatically on boot, so replacement instances require no manual setup.

RDS
A MySQL database instance (ha-db) hosts the WordPress schema. All web tier instances share this single database, meaning application state (posts, settings, users) is centralized and persists independently of any individual EC2 instance's lifecycle — a core requirement for statelessness at the web tier.


Test A — Instance Failure
One running EC2 instance was manually terminated to simulate an unexpected failure. The ALB immediately stopped routing traffic to the terminated instance and shifted all requests to the remaining healthy instance, so the site stayed continuously reachable throughout. Within roughly 1–2 minutes, the Auto Scaling Group detected the capacity shortfall and automatically launched a replacement instance from the launch template, which came up healthy and rejoined the target group without any manual intervention.

Test B — Availability Zone Impact
Desired/minimum capacity was temporarily reduced to 1, removing all running capacity from one Availability Zone. The application remained fully available throughout, with the ALB routing all traffic to the single surviving instance in the other AZ — confirming the architecture tolerates the loss of an entire AZ, not just a single instance. Capacity was then restored to 2, and the ASG automatically relaunched a second instance to re-establish multi-AZ redundancy.

Result: Both tests confirmed the deployment meets its high-availability goal — the application survives both individual instance failure and full AZ impact without manual recovery, with the ALB and ASG working together to maintain availability automatically.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about the high-availability build, including the ALB URL (or a redacted screenshot), three to five lines on what you built and how you tested high availability, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/posts/moses-avoseh_just-finished-building-a-fully-self-healing-activity-7499504757858828288-YQvP?utm_source=share&utm_medium=member_desktop&rcm=ACoAACZiz20BSL2chCMaU_0WK_2_7qktttgciMQ`

---

#### Screenshot — Published LinkedIn post

![](<screenshots/Linkedln post.png>)

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose passwords, connection strings, private keys, or account IDs

---

# Completion Checklist

- [✅ Completed] Task 1: VPC, four subnets, IGW, NAT Gateway, and route tables created (Screenshots 1–5)
- [✅ Completed] Task 2: Least-privilege ALB, EC2, and RDS security groups created (Screenshots 6–8)
- [✅ Completed] Task 3: Private Multi-AZ RDS created (Screenshots 9–10)
- [✅ Completed] Task 4: Self-configuring Launch Template created and tested (Screenshots 11–12)
- [✅ Completed] Task 5: ALB created across both public subnets (Screenshots 13–14)
- [✅ Completed] Task 6: Auto Scaling Group running two instances across two AZs (Screenshots 15–16)
- [✅ Completed] Task 7: Application verified through the ALB with a database read and write (Screenshots 17–18)
- [✅ Completed] Task 8: Both high-availability tests completed (Screenshots 19–22)
- [✅ Completed] Task 9: Architecture and test-results summary completed (Screenshot 23 & Notes)
- [✅ Completed] LinkedIn post published and URL submitted
- [✅ Completed] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
