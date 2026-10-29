# Assignment 2 — Deploy Mini Finance Project via Azure DevOps Pipeline

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build an Azure DevOps CI/CD pipeline that deploys the Mini Finance static website to an Ubuntu VM running Nginx: importing the repo into Azure Repos, provisioning the VM with Terraform and Ansible, connecting via an SSH Service Connection, and deploying on every commit to `main`.

---

# Task 1 — Import the Repository

## Goal

Import `https://github.com/pravinmishraaws/Azure-Static-Website` into Azure Repos and confirm `index.html` is present.

### Evidence

#### Screenshot 1 — Azure Repos showing the imported repository files with `index.html` visible

![](screenshots/W10A2T1S1.png).

---

# Task 2 — Prepare the Target VM

## Goal

Provision a Linux VM with Terraform (ports 22/80 open), then use Ansible to install and start Nginx and prepare `/var/www/html`.

### Evidence

#### Screenshot 2 — Terraform output or cloud console showing the running VM and public IP

![](screenshots/W10A2T2S2.png)

---

#### Screenshot 3 — Terminal showing Ansible completed successfully and Nginx is active

![](screenshots/W10A2T2S3.png)

---

# Task 3 — Create an SSH Service Connection

## Goal

Create the password-based SSH Service Connection `ubuntu-nginx-ssh` pointing to the VM, and validate it.

### Evidence

#### Screenshot 4 — SSH Service Connection configuration page showing the connection details and successful validation, with the password hidden

![](screenshots/W10A2T3S4.png)

---

# Task 4 — Author the YAML Pipeline

## Goal

Write a pipeline triggered on `main` that checks out the repo, copies files to `/var/www/html` via `CopyFilesOverSSH@0`, and verifies the deployment directory via an `SSH@0` task, using `ubuntu-nginx-ssh` and the self-hosted (or available Microsoft-hosted) pool.

### Evidence

#### Screenshot 5 — Pipeline YAML definition open in the Azure DevOps editor

![](screenshots/W10A2T4S5.png)

---

# Task 5 — Verify Deployment

## Goal

Confirm the pipeline run succeeded (checkout, SSH connection, file transfer, remote verification) and the Mini Finance website is live at the VM's public IP.

### Evidence

#### Screenshot 6 — Successful Azure DevOps pipeline run log summary

![](screenshots/W10A2T5S6.png)

---

#### Screenshot 7 — Browser showing the deployed website with the VM public IP visible

![](screenshots/W10A2T5S7.png)

---

### Notes

Include the VM public URL. Describe any issue you faced and how you fixed it (e.g. parallelism/agent-pool issues).

VM public URL: http://18.201.222.183

Setup: Terraform (AWS EC2, eu-west-1) provisioned the target VM; Ansible installed Nginx and set up the ubuntu user's password. The pipeline runs on my self-hosted agent (SelfHostedPool) and deploys on every push to main.

Issue 1: Ansible failed with "No module named 'passlib'" when setting the ubuntu password. I fixed it by running pip install passlib in my virtual environment and rerunning the playbook.

Issue 2: The first Terraform and Ansible runs did not work from Git Bash because Ansible was not found. I ran them from Ubuntu on WSL, where Ansible was installed.

No parallelism or agent-pool issues occurred. The self-hosted agent picked up the job immediately.

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not commit the VM password to the repository or write it directly in YAML

---

# Completion Checklist

- [c] Task 1: Repository imported into Azure Repos (Screenshot 1)
- [C] Task 2: VM provisioned and Nginx configured (Screenshots 2–3)
- [C] Task 3: SSH Service Connection created and validated (Screenshot 4)
- [C] Task 4: YAML pipeline authored (Screenshot 5)
- [C] Task 5: Pipeline run succeeded and site verified (Screenshots 6–7)
- [C] VM URL and issue notes written (Notes)
- [C] No passwords, tokens, or credentials exposed

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
