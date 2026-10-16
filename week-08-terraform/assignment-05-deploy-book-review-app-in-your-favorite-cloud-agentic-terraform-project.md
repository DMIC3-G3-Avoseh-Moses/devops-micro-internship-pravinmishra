# Assignment 5 — Deploy Book Review App in Your Favorite Cloud (Agentic Terraform Project)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

This is the most important assignment of the Terraform section. You will deploy the Book Review App in a production-style three-tier architecture using Terraform on your choice of AWS or Azure — six subnets across two Availability Zones, tier-specific security rules, public and internal load balancers, Next.js/Node.js on Ubuntu VMs, and a private managed MySQL database with a read replica. This assignment is agent-assisted: you may use Claude Code, ChatGPT, or another LLM tool to help design, generate, debug, and improve the infrastructure.

---

# Task 1 — VPC/VNet and Subnet Setup

## Goal

Create a custom VPC/VNet (10.0.0.0/16) with six subnets across two Availability Zones: two public Web Tier subnets, two private App Tier subnets, and two private Database Tier subnets, implemented with Terraform.

### Evidence

#### Screenshot 1 — VPC or VNet details showing 10.0.0.0/16

![](screenshots/W8A5T1S1.png)

---

#### Screenshot 2 — Subnet list showing all six subnets, their tiers, CIDR ranges, and Availability Zones

![](screenshots/W8A5T1S2.png)

---

#### Screenshot 3 — Terraform plan or cloud networking view showing the required routing and tier isolation

![](screenshots/W8A5T1S3.png)

---

# Task 2 — Security Groups/NSGs and Load Balancers

## Goal

Configure tier-specific Security Groups/NSGs (Web Tier HTTP 80, App Tier 3001 only from Web Tier, Database Tier 3306 only from App Tier), and create a public load balancer for the frontend and an internal load balancer for the backend, all with Terraform.

### Evidence

#### Screenshot 4 — Web, App, and Database Security Group or NSG rules

![](screenshots/W8A5T2S4.png)

---

#### Screenshot 5 — Public frontend load balancer configuration

![](screenshots/W8A5T2S5.png)

---

#### Screenshot 6 — Internal backend load balancer configuration

![](screenshots/W8A5T2S6.png)

---

#### Screenshot 7 — Healthy frontend and backend targets or backend pools

![](screenshots/W8A5T2S7.png)

---

# Task 3 — VMs and Application Deployment

## Goal

Deploy the Next.js Web Tier behind Nginx on port 80 in the public subnets, and the Node.js App Tier on port 3001 in the private subnets (no Elastic IPs/Public IPs on private VMs), with the frontend reaching the backend through the internal load balancer.

### Evidence

#### Screenshot 8 — EC2 or Azure VM dashboard showing the frontend and backend VMs

![](screenshots/W8A5T3S8.png)

---

#### Screenshot 9 — Nginx status or frontend response on the Web Tier

![](screenshots/W8A5T3S9.png)

---

#### Screenshot 10 — Backend API response through the permitted internal path

![](screenshots/W8A5T3S10.png)

---

# Task 4 — MySQL Database Setup

## Goal

Deploy a private managed MySQL database (Amazon RDS Multi-AZ or Azure Database for MySQL Flexible Server) with a read replica, restricted to the App Tier on port 3306, and validate the Book Review App homepage, login, review flow, backend API, and database integration through the public load balancer.

### Evidence

#### Screenshot 11 — Amazon RDS or Azure Database dashboard showing the primary database and read replica

![](screenshots/W8A5T4S11.png)

---

#### Screenshot 12 — Evidence of private database networking and permitted App Tier access

![](screenshots/W8A5T4S12.png)

---

#### Screenshot 13 — Functional Book Review App homepage and login flow

![](screenshots/W8A5T4S13.png)

---

#### Screenshot 14 — Functional review flow with working backend API and database integration

![](screenshots/W8A5T4S14.png)

---

#### Screenshot 15 (optional) — Application logs or terminal output

![](screenshots/W8A5T4S15.png)

---

### Notes

Report the cloud platform used (AWS or Azure), your Terraform code structure (`main.tf`, `variables.tf`, `outputs.tf`, and supporting files), a link/description of your architecture diagram, and the Public Load Balancer DNS used to access the frontend.

**Cloud platform:** AWS (region us-east-2, Ohio)

**Terraform code structure:**

terraform/
├── main.tf            (root module: wires the modules together)
├── variables.tf       (input variables)
├── outputs.tf         (non-sensitive outputs: ALB DNS names, IDs, DB hostnames)
├── terraform.tfvars   (non-secret values)
└── modules/
    ├── network/       (VPC 10.0.0.0/16, six subnets, IGW, NAT, route tables)
    ├── security/      (tier-specific security groups)
    ├── load-balancer/ (public and internal ALBs, listeners, target groups, health checks)
    ├── compute/       (Web and App EC2 instances, bootstrap scripts, IAM roles)
    └── database/      (RDS MySQL Multi-AZ, read replica, SSM parameters)

Modules communicate only through variables and outputs. The DB password and JWT secret are never stored in state, outputs or user data. They are written to SSM Parameter Store as SecureString.

**Architecture diagram:** ![<paste your diagram link or describe the image you embedded>]
https://docs.google.com/document/d/1WEABHkPD4vCmSCeH2khrJDv-fqE5lIcWeKe9JY-T0qM/edit?usp=sharing
(screenshots/W8A5T5S.png)

**Public Load Balancer DNS:**
http://book-review-dev-pub-alb-1086794438.us-east-2.elb.amazonaws.com

**Traffic flow:** Internet → public ALB (80) → Web tier (Nginx + Next.js) → internal ALB (3001) → App tier (Node.js/Express) → RDS MySQL (3306, App SG only).

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about what you achieved in this assignment, with public or "Anyone" visibility.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/ecrvEpCi`

---

#### Screenshot 16 — Published LinkedIn post showing the text and at least one image or proof

![](screenshots/W8A5T6S.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Include your architecture diagram and Public Load Balancer DNS
- Do not expose passwords, keys, tokens, database credentials, or Terraform state secrets

---

# Completion Checklist

- [✅ Completed] Task 1: Six-subnet VPC/VNet created across two AZs with Terraform (Screenshots 1–3)
- [✅ Completed] Task 2: Tier-specific security rules and load balancers configured (Screenshots 4–7)
- [✅ Completed] Task 3: Web and App Tier VMs deployed with correct public/private placement (Screenshots 8–10)
- [✅ Completed] Task 4: Private MySQL with read replica deployed and app validated end to end (Screenshots 11–15)
- [✅ Completed] Report completed: cloud platform, Terraform structure, diagram, LB DNS (Notes)
- [✅ Completed] LinkedIn post published and URL submitted (Screenshot 16)
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
