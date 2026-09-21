# Assignment 6 — Capstone: Deploy Book Review App (Three-Tier Architecture) on Azure

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a production-ready, best-practice-compliant three-tier architecture on Azure: separated presentation, application, and database tiers, least-privilege network access, a controlled public entry point, protected secrets, and availability/monitoring evidence.

---

# Task 1 — Design the Azure Three-Tier Architecture

## Goal

Create an architecture diagram and implementation plan identifying the presentation, application, and database components, the chosen Azure services, the public entry point, and the internal traffic paths.

### Evidence

#### Screenshot 1 — Architecture diagram showing the public entry point, three tiers, network boundaries, and traffic flow

![](screenshots/W7A6T1S1.png)

---

#### Screenshot 2 — Written architecture assumptions and selected Azure services

Architecture Assumptions
Three-tier separation is enforced at the network level, not just logically. Web, application, and database each get their own subnet, NSG, and route table — no tier can be skipped or reached directly from a lower layer.
Only the web tier is internet-facing. The app and database subnets have no public IPs; all inbound access to them is mediated by the web tier (for app traffic) or exists only within the VNet (for the database).
The app tier needs outbound internet access but not inbound. It must reach GitHub and package registries during deployment, but nothing outside the VNet should be able to initiate a connection to it — hence NAT Gateway instead of a public IP.
The database is fully private. It's reachable only from the app subnet's IP range, over an encrypted (SSL) connection, and never has SSH or any other access opened to it.
Application code drives networking decisions, not the reverse. The backend's actual listening port, the database's port and SSL requirement, and the lack of an npm start script were all confirmed by reading server.js, db.js, and package.json before any Azure resource was created — so ports in NSGs, health probes, and load-balancer rules match what the code really does, not a guessed default.
Administrative access is minimized. SSH is restricted to a single known IP on the web tier and reachable on the app tier only via jump-host hop through the web VM — never opened broadly.
State and persistence are externalized. The application itself is stateless (no local session storage assumed); all persistent data lives in the managed MySQL service, and process survival (across reboots/crashes) is handled by PM2 rather than application code.
Single-instance per tier for this deployment, with the load balancers and backend pools structured so additional VMs could be added later without re-architecting.
Selected Azure Services
Layer	Service	Why
Networking	Virtual Network (VNet) + 3 subnets	Isolates web, app, and DB tiers into separate address spaces
Networking	NAT Gateway	Gives the private app subnet outbound-only internet access without a public IP
Networking	Route tables (3)	Control per-tier egress: internet route for web, NAT-backed route for app, no route for DB
Security	Network Security Groups (3)	Enforce least-privilege inbound rules per tier (web: 80/443/22; app: app port from web subnet only; DB: 3306 from app subnet only)
Compute	Virtual Machines (Ubuntu 24.04 LTS)	Host the Nginx/Next.js web tier and the Node/Express app tier
Compute	PM2 (on both VMs)	Keeps Node processes alive across SSH disconnects and reboots
Load balancing	Public Load Balancer (Standard)	Internet-facing entry point, health-checks and distributes traffic to the web tier
Load balancing	Internal Load Balancer (Standard)	Private entry point from web tier to app tier, decouples Nginx from any single app VM's IP
Database	Azure Database for MySQL Flexible Server	Managed MySQL with VNet-integrated private access, matching the app's Sequelize/mysql2/SSL requirements
Reverse proxy	Nginx (on the web VM)	Serves the frontend and forwards /api/* to the internal load balancer, keeping the app tier's address out of the browser entirely

---

# Task 2 — Create the Azure Network Foundation

## Goal

Create a dedicated Resource Group and VNet with separate subnets for the web, application, and database tiers, keeping the application and database tiers without direct public access.

### Evidence

#### Screenshot 3 — Resource Group overview showing the assignment resources

![](screenshots/W7A6T2S3.png)

---

#### Screenshot 4 — VNet overview showing the address space and all required subnets

![](screenshots/W7A6T2S4.png)

---

#### Screenshot 5 — Route-table or Private DNS evidence where applicable

![](screenshots/W7A6T2S5.png)

---

# Task 3 — Configure Security and Secret Management

## Goal

Apply least-privilege NSG rules so traffic flows Internet → public entry point → web tier → application tier → database tier, and store credentials in Azure Key Vault or another approved secure mechanism.

### Evidence

#### Screenshot 6 — NSG rules proving least-privilege access between the tiers

![](screenshots/W7A6T3S6.png)

---

#### Screenshot 7 — Key Vault or approved secret-management configuration (without displaying secret values)

![](screenshots/W7A6T3S7.png)

---

# Task 4 — Deploy the Presentation (Web) Tier

## Goal

Deploy the Book Review App presentation layer on the approved web-tier compute service, configured to route requests to the internal application-tier endpoint, and not directly exposed except through the public entry service.

### Evidence

#### Screenshot 8 — Web-tier compute overview showing subnet and availability configuration

![](screenshots/W7A6T4S8.png)

---

#### Screenshot 9 — Terminal or service output proving the presentation layer is running

![](screenshots/W7A6T4S9.png)

---

# Task 5 — Deploy the Business (Application) Tier

## Goal

Deploy the Book Review App backend privately in the application subnet, configured to use the private database endpoint and secured environment values, reachable only through its internal endpoint.

### Evidence

#### Screenshot 10 — Application-tier compute overview showing private subnet placement

![](screenshots/W7A6T5S10.png)

---

#### Screenshot 11 — Backend process, service, or listening-port evidence

![](screenshots/W7A6T5S11.png)

---

#### Screenshot 12 — Internal health-check or API response (without exposing secrets)

![](screenshots/W7A6T5S12.png)

---

# Task 6 — Deploy the Managed Database Tier

## Goal

Create a private Azure managed database (public access disabled), with availability/backup/retention settings, the Book Review App schema imported, and access restricted to the application tier only.

### Evidence

#### Screenshot 13 — Database overview showing private connectivity and public access disabled

![](screenshots/W7A6T6S13.png)

---

#### Screenshot 14 — Availability, backup, and retention configuration

![](screenshots/W7A6T6S14.png)

---

#### Screenshot 15 — Successful schema or connectivity verification (without exposing credentials)

![](screenshots/W7A6T6S15.png)

---

# Task 7 — Configure Traffic Management, Availability, and Monitoring

## Goal

Configure the approved public entry service with health probes and backend pools, internal routing for the application tier where required, and enable Azure Monitor/diagnostics/logs/alerts for the key resources.

### Evidence

#### Screenshot 16 — Public entry service showing listener, frontend endpoint, and healthy web targets

![](screenshots/W7A6T7S16.png)

---

#### Screenshot 17 — Internal application-tier load-balancing or routing configuration where applicable

![](screenshots/W7A6T7S17.png)

---

#### Screenshot 18 — Azure Monitor, diagnostic settings, logs, metrics, or alert evidence

![](screenshots/W7A6T7S18.png)

---

# Task 8 — Validate the Production-Style Deployment

## Goal

Confirm the Book Review App works end to end through the public endpoint, with at least one database read and one write, confirm private tiers are not internet-reachable, and complete a safe availability test.

### Evidence

#### Screenshot 19 — Browser showing the Book Review App through the public endpoint

![](screenshots/W7A6T8S19.png)

---

#### Screenshot 20 — Proof of successful database-backed read and write operations

![](screenshots/W7A6T8S20.png)

---

#### Screenshot 21 — Evidence that private tiers are not publicly accessible

![](screenshots/W7A6T8S21.png)

---

#### Screenshot 22 — Availability-test and healthy-target evidence

![](screenshots/W7A6T8S22.png)

---

#### Public Endpoint

Paste your public endpoint URL here:

`http://20.215.103.3/`

---

### Notes

Summarize what worked, issues encountered and how they were fixed, and the availability/security/secrets/monitoring/backup choices made.

Deployment Summary

What worked: The three-tier network design (Web/App/DB subnets, each with its own NSG) correctly enforced Internet → Public LB → Web tier → Internal LB → App tier → DB tier, with no tier reachable out of order. Once configured, the Load Balancers, MySQL Flexible Server, and PM2-managed processes all ran reliably, and full end-to-end testing (register, login, submit a review) succeeded with real data persisting to the database.

Issues fixed: A leftover AWS-style NAT route on the App route table was removed (Azure uses subnet association instead). SSH key path confusion when jumping between VMs was resolved by correctly scp-ing keys over. A .env typo (book_review_dbb vs book_review_db) caused a connection failure. PM2 crash-looped due to daemon conflicts and port collisions (EADDRINUSE) from processes started manually alongside systemd — fixed by killing stray processes/daemons and standardizing on systemctl for process management. The frontend showed no data due to a doubled /api/api/books path, fixed by removing a redundant prefix. Registration failed with a 500 error due to a CORS origin mismatch, fixed by adding the correct IP to ALLOWED_ORIGINS.

Key choices: Load Balancers with health probes for availability; tightly scoped NSGs and a fully private database for security; Azure Key Vault for secrets instead of hardcoding credentials; PM2/systemd status and LB health checks for monitoring; automated backup retention on the MySQL server for recovery.

---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, keys, connection strings, or subscription IDs

---

# Completion Checklist

- [✅ Completed] Task 1: Architecture diagram and assumptions documented (Screenshots 1–2)
- [✅ Completed] Task 2: Network foundation created with isolated tiers (Screenshots 3–5)
- [✅ Completed] Task 3: Least-privilege security and secret management configured (Screenshots 6–7)
- [✅ Completed] Task 4: Presentation tier deployed (Screenshots 8–9)
- [✅ Completed] Task 5: Application tier deployed privately (Screenshots 10–12)
- [✅ Completed] Task 6: Managed database tier deployed privately (Screenshots 13–15)
- [✅ Completed] Task 7: Public entry, internal routing, and monitoring configured (Screenshots 16–18)
- [✅ Completed] Task 8: End-to-end validation and availability test completed (Screenshots 19–22, Public Endpoint, Notes)
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
