# Assignment 3 — CodeTrack: Branching Workflow (Add & Verify a Contact Page)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will add a new Contact page to CodeTrack using a clean feature-branch workflow. You will keep each change in a separate commit, prove that your default branch remains unchanged before the merge, and validate the result after merging.

---

# Task 1 — Confirm Repository State and Default Branch

## Goal

Start from a clean default branch (`main` or `master`) and confirm the repository status.

### Evidence

#### Screenshot 1 — Output of `git status` and `git branch` showing a clean status and the default branch checked out

![Gitstatus](A3T1S1.png)

---

# Task 2 — Create and Switch to a Feature Branch

## Goal

Create a branch named exactly `feature/contact-page` and switch to it.

### Evidence

#### Screenshot 2 — Output of `git checkout -b feature/contact-page` and `git branch` showing `* feature/contact-page`

![Gitbranch](A3T1S2.png),![GitBranch2](A3T1S22.png)

---

# Task 3 — Add contact.html on the Feature Branch

## Goal

Create `contact.html` with the provided content and commit it alone using the message `feat(contact): add Contact page`.

### Evidence

#### Screenshot 3 — Output of `ls` showing `contact.html`

![ls](A3T1S3.png)

---

#### Screenshot 4 — Output of `git commit`

![git commit](A3T3S4.png)

---

#### Screenshot 5 — Output of `git log --oneline -3` showing the new commit

![git log](A3T3S5.png)

---

# Task 4 — Add the Contact Link to index.html

## Goal

Add the provided Contact Page link to `index.html` and commit it separately using the message `feat(nav): add Contact Page link`.

### Evidence

#### Screenshot 6 — Output of `git status` showing `index.html` as modified before staging

![Git status](A3T3S6.png)

---

#### Screenshot 7 — Output of `git commit`

![gitcommit](A3T3S7.png)

---

#### Screenshot 8 — Browser showing the Contact Page link on the homepage while on `feature/contact-page`

![Contactpage](A3T3S8.png)

---

# Task 5 — Verify Isolation (Prove the Default Branch Is Unchanged)

## Goal

Switch back to the default branch and confirm that `contact.html` and the Contact Page link do not exist there yet.

### Evidence

#### Screenshot 9 — Terminal showing the checkout and `ls` output, proving `contact.html` is absent

![ls](A3T3S9.png).

---

#### Screenshot 10 — Browser showing the homepage on the default branch with no Contact Page link

![CP](A3T3S10.png)

---

# Task 6 — Merge the Feature Branch into the Default Branch

## Goal

Merge `feature/contact-page` into your default branch and confirm the Contact page works.

### Evidence

#### Screenshot 11 — Output of `git merge feature/contact-page`

![Gitmerge](A3T3S11.png)

---

#### Screenshot 12 — Output of `ls` showing `contact.html` after the merge

![ls](A3T3S12.png)

---

#### Screenshot 13 — Browser showing the Contact page opened from the homepage link on the default branch

![Defaultbranch](A3T3S13.png),![Contactpage](A3T3S14.png)

---

# Task 7 — Inspect History (Graph View)

## Goal

Display the repository history as a graph and locate both feature commits.

### Evidence

#### Screenshot 14 — Full output of `git log --oneline --graph --decorate --all`

![log](A3T3S144.png)

---

# Task 8 — Optional Cleanup (Delete the Feature Branch)

## Goal

Delete the merged `feature/contact-page` branch to keep your branch list clean.

### Evidence

#### Screenshot 15 (Optional) — Output showing `feature/contact-page` deleted and no longer listed

![Output](A3T3S15.png),![del](A3T3S155.png)

---

# Submission Instructions

- Tasks 1–7 are required; Task 8 is optional
- Add all required screenshots in your submission
- Evidence must show `contact.html` and the homepage link were absent before merging, and working after merging
- Do not expose passwords, access tokens, or private keys

---

# Completion Checklist

- [✅ Completed] Repository confirmed clean on the default branch (Screenshot 1)
- [✅ Completed] `feature/contact-page` created and checked out (Screenshot 2)
- [✅ Completed] `contact.html` added in its own commit (Screenshots 3–5)
- [✅ Completed] Homepage Contact link added in a separate commit (Screenshots 6–8)
- [✅ Completed] Default branch proven unchanged before merge (Screenshots 9–10)
- [✅ Completed] Feature branch merged and Contact page verified (Screenshots 11–13)
- [✅ Completed] Graph history reviewed (Screenshot 14)
- [✅ Completed] Optional cleanup completed (Screenshot 15)
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
