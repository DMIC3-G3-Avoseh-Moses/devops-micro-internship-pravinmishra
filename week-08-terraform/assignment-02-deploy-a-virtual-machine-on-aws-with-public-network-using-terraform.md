# Assignment 2 — Deploy a Virtual Machine on AWS with Public Network Using Terraform

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will use Terraform to create a custom AWS network (VPC, public and private subnets, Internet Gateway, route table) and launch an EC2 instance into the public subnet with a public IP, a Security Group allowing SSH and HTTP, and Nginx installed for validation.

---

# Task 1 — Create a New Terraform Project

## Goal

Create a `terraform-aws-vm` project directory for the AWS Terraform configuration.

### Evidence

#### Screenshot 1 — File Explorer, VS Code, or terminal showing the `terraform-aws-vm` project directory

![](screenshots/W8A2T1S1.png)

---

# Task 2 — Create main.tf with the Required AWS Resources

## Goal

Define the AWS provider, a VPC (10.0.0.0/16) with a public subnet (10.0.1.0/24) and private subnet (10.0.2.0/24), an Internet Gateway with public routing, a Security Group (SSH 22, HTTP 80), an EC2 instance in the public subnet with a public IP, and a public IP output.

### Evidence

#### Screenshot 2 (optional) — `main.tf` showing the VPC and EC2 resource blocks

![](screenshots/W8A2T2S2.png)

---

# Task 3 — Initialize Terraform

## Goal

Run `terraform init` and confirm the working directory initializes successfully.

### Evidence

#### Screenshot 3 — Terminal showing successful `terraform init` output

![](screenshots/W8A2T3S3.png)

---

# Task 4 — Plan and Apply the Configuration

## Goal

Review `terraform plan`, run `terraform apply`, and record the EC2 instance's public IP from the Terraform output.

### Evidence

#### Screenshot 4 — Terraform apply output showing successful completion

![](screenshots/W8A2T4S4.png)

---

#### Screenshot 5 — Terraform output showing the EC2 public IP

![](screenshots/W8A2T5S5.png)

---

# Task 5 — Verify the Deployment

## Goal

Confirm the EC2 instance is running in the public subnet with a public IP, install Nginx, and confirm it is accessible by browser or SSH.

### Evidence

#### Screenshot 6 — EC2 instance running in the AWS Console, with the subnet and public IP visible

![](screenshots/W8A2T6S6.png)

---

#### Screenshot 7 — Browser showing the Nginx page through the EC2 public IP, or terminal showing a successful SSH connection

![](screenshots/W8A2T7S7.png)

---

# Task 6 — Destroy Resources

## Goal

Run `terraform destroy` to remove the Terraform-managed AWS resources after testing.

### Evidence

#### Screenshot 8 — Terminal showing successful `terraform destroy` completion

![](screenshots/W8A2T8S8.png)

---

### Notes

Write a short paragraph about any challenges you faced and how you solved them.

The main challenges in this assignment were around setup rather than the Terraform code itself. I authenticated the AWS CLI with the root user’s access key. I first tried creating a dedicated IAM user, but attaching a policy failed with a quota error because I had selected the wrong policy (AWSAuditManagerAdministratorAccess instead of AdministratorAccess). I also had to create a new access key because the secret key is only shown once, then run aws configure so the CLI and Terraform could authenticate. The assignment’s example used another student’s file path and the eu-north-1 region, so I generated my own SSH key pair, used the Windows-style path in the file() function, and changed the provider region to us-east-2 so everything matched. Running terraform init, validate and plan before apply helped me confirm the nine resources were correct before anything was created. Finally, I ran terraform destroy to remove everything and avoid unnecessary charges.

---

# Submission Instructions

- Add all required screenshots in your submission
- Include the EC2 public IP
- Do not expose AWS credentials, private keys, or account IDs

---

# Completion Checklist

- [✅ Completed] Task 1: `terraform-aws-vm` project created (Screenshot 1)
- [✅ Completed] Task 2: `main.tf` defines VPC, subnets, IGW, Security Group, and EC2 (Screenshot 2, optional)
- [✅ Completed] Task 3: `terraform init` completed successfully (Screenshot 3)
- [ ] Task 4: Plan reviewed and `terraform apply` completed, public IP recorded (Screenshots 4–5)
- [✅ Completed] Task 5: EC2 instance verified running and accessible (Screenshots 6–7)
- [✅ Completed] Task 6: `terraform destroy` completed successfully (Screenshot 8)
- [✅ Completed] Challenges/solutions paragraph written (Notes)
- [✅ Completed] No sensitive information exposed

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
