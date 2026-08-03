# Assignment 2 — CodeTrack: Tracking, Staging, Committing + Deploy to EC2

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will track and stage project files, create two meaningful Git commits in `CodeTrack`, verify your commit history, and deploy the CodeTrack static website to an EC2 instance using Nginx. This connects local version-control practice with a basic manual deployment workflow used in real DevOps environments.

---

# Task 1 — Verify Git Setup and Enter the Repository

## Goal

Confirm that Git works and that you are inside the correct `CodeTrack` repository.

### Evidence

#### Screenshot 1 — Output of `pwd` showing you're inside `CodeTrack`

![Pwd](<Assignment 2, Task 1 pwd.png>)

---

#### Screenshot 2 — Output of `git status` showing no "not a git repository" error

![Gitstatus](<Assignment 2 Task 1 Screenshot 2.png>)

---

# Task 2 — Create index.html and style.css

## Goal

Create the two starter UI files inside `CodeTrack`.

### Evidence

#### Screenshot 3 — Output of `ls` showing `index.html` and `style.css`

![ls](<Assignment 2 Task 2 screenshot.png>)

---

# Task 3 — Add Starter Content

## Goal

Copy the provided starter HTML and CSS content into your local `index.html` and `style.css` files.

### Evidence

#### Screenshot 4 — Your editor showing the contents of `index.html` and `style.css`

![index and style](<Assignment 2 Task 3.png>)

---

# Task 4 — Track and Stage Files Correctly

## Goal

Confirm both files show as untracked, then stage them individually with `git add`.

### Evidence

#### Screenshot 5 — Output of `git status` showing both files as untracked

![Git Stat](<Assignment 2 Task 4.png>)

---

#### Screenshot 6 — Output of `git status` showing both files staged under "Changes to be committed"

![git stat](<Assignment 2 Task 4 screenshot 6.png>)

---

# Task 5 — Create the First Commit (Clean Initial Commit)

## Goal

Commit the staged starter files using the message `Initial UI scaffold: add index.html and style.css`, then check the log.

### Evidence

#### Screenshot 7 — Output of `git commit`

![Gitcommit](<Assignment 2 Task 5 Screenshot 7.png>)

---

#### Screenshot 8 — Output of `git log --oneline` showing the first commit

![gitlog](<Assignment 2 task 5 screenshot 8.png>)

---

# Task 6 — Modify index.html and Create a Second Commit

## Goal

Follow the instruction comment inside `index.html` to update the Student Name and Group Name, then commit that change separately using the message `Update homepage content: heading, tagline, CTA button`.

### Evidence

#### Screenshot 9 — Browser showing the updated page with your Student Name and Group Name visible

![Update](<Assignment 2 Task 6 Screenshot 9.png>)

---

#### Screenshot 10 — Output of `git status` showing `index.html` as modified

![Git status](<Assignment 2 Task 1 Screenshot 10.png>)

---

#### Screenshot 11 — Output of `git commit`

![Git commit](<Assignment 2 Task 1 Screenshot 11.png>)

---

#### Screenshot 12 — Output of `git log --oneline` showing two commits

![git log](<Assignment 2 Task 1 Screenshot 12.png>)

---

# Task 7 — Deploy to EC2 with Nginx (Static Website)

## Goal

Install and start Nginx on your EC2 instance, then copy `index.html` and `style.css` into the Nginx web root.

### Evidence

#### Screenshot 13 — Output of `systemctl status nginx --no-pager` showing Nginx `active (running)`

![Ngnix active](<Assignment 2 Task 7 Screenshot 13.png>)

---

#### Screenshot 14 — Output of `curl -I http://localhost` showing `HTTP/1.1 200 OK`

![Curl](<Assignment 2 Task 7 Screenshot 14.png>)

---

#### Screenshot 15 — Browser showing the CodeTrack site loaded at `http://<EC2_PUBLIC_IP>`, with your Full Name and Group Name visible

![Http](<Assignment 2 Task 7 Screenshot 15.png>)

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/posts/moses-avoseh_git-github-aws-share-7485707646281363456-xA7D/?utm_source=share&utm_medium=member_desktop&rcm=ACoAACZiz20BSL2chCMaU_0WK_2_7qktttgciMQ`

---

#### Screenshot — LinkedIn post showing the deployed CodeTrack application

![Linkedlnpost](<Linkedln post.png>)

---

# Submission Instructions

- Add all required screenshots in your submission
- Full Name and Group Name must be visible in the deployed application evidence
- `git log --oneline` output must show at least two meaningful commits
- Do not expose AWS access keys, passwords, private key contents, or other sensitive information

---

# Completion Checklist

- [✅ Completed] `CodeTrack` repository verified with `git status` (Screenshots 1–2)
- [✅ Completed] `index.html` and `style.css` created and populated (Screenshots 3–4)
- [✅ Completed] Starter files staged and committed in the first commit (Screenshots 5–8)
- [✅ Completed] Student Name and Group Name updated in `index.html` (Screenshot 9)
- [✅ Completed] Second controlled commit created (Screenshots 10–12)
- [✅ Completed] Nginx active on the EC2 instance and CodeTrack reachable via its public IP (Screenshots 13–15)
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
