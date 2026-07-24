# Assignment 6 — Building an AI-Assisted Git Safety Net (PR Ready Check)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In Week 2 you built Claude Code hooks that block a dangerous action *before* it happens (`PreToolUse`), and a restricted skill that could look but not touch (`allowed-tools` without `Write`). In this assignment you will discover that Git has the exact same idea, decades older: a **pre-commit hook** that blocks a commit before it's created.

You will build both halves of a real "PR Ready" workflow:

1. A **Git hook that follows fixed rules** — scans staged changes for hardcoded secrets and oversized files and refuses the commit. No AI involved, no guessing, just a rule that gives the same answer every time.
2. A **restricted Claude Code skill** (`/pr-ready`) that reads your staged diff and drafts a Pull Request title, description, and a short list of things worth a second look — the kind of judgment a fixed rule can't make (mixed changes, missing context, unclear intent). The skill never commits, pushes, or opens the PR. You do that yourself, using its draft as a starting point.

This mirrors the Agentic Loop from Week 3's Linux triage assignment: **Gather → Analyze → Human Act → Verify**. The hook and the skill both gather and analyze; only you act.

---

# Task 0 — Confirm Your Fork and Create a Feature Branch

## Goal

Confirm you are working in your own fork, then create a dedicated branch for this assignment.

### Evidence

#### Screenshot 1 — Output of git remote -v and git branch showing the new branch

![git remote](A6T0S1.png),![git branch](A6T0S2.png)

---

### Notes

**1. Why create a dedicated branch instead of doing this work on main?**

Creating a separate branch allows you to work independently without affecting the main branch. It provides a safe space to develop, test, and refine your changes before merging them. It also ensures that your Pull Request contains only the changes relevant to the assignment, making the review process clearer and more organized.

---

# Task 1 — Stage a Change With Realistic Risk

## Goal

On your own fork of this repository (the one you've been submitting your DMI work in since onboarding), create a new branch and stage a change that a real reviewer should catch: a hardcoded-looking secret and a leftover debug statement.

### Evidence

#### Screenshot 1 — Output of  `git status` showing the staged file on feature/ai-pr-ready

![git status](A6T1S1.png)

---

### Notes

**1. Why does this assignment use an obviously fake key instead of a real one?**

The assignment uses an obviously fake key to demonstrate the concept without exposing any real credentials. Using a placeholder key keeps the exercise safe because real API keys or secrets should never be shared, committed to a repository, or included in training materials. The focus is on learning how authentication works and how to handle secrets properly, not on using an actual credential.

---

# Task 2 — Write a Real Git Pre-Commit Hook

## Goal

Create a tracked, shareable pre-commit hook that blocks a commit containing secret-like patterns or files over 1MB.

### Evidence

#### Screenshot 2 — `hooks/pre-commit` open in VS Code showing the full script

![hooks](A6T2S3.png)

---

#### Screenshot 3 — Output of `git config core.hooksPath` confirming it points to `hooks`

![githooks](A6T2S4.png)

---

### Notes

**1. Why is `hooks/pre-commit` tracked in the repo instead of living only in `.git/hooks/`?**

hooks/pre-commit is tracked in the repository so that all contributors can use the same pre-commit checks.
Files inside .git/hooks/ are local only and are not shared when the repository is cloned.
Tracking the hook ensures consistent code quality, formatting, and validation across the project.
It also makes the development workflow easier to set up and maintain for everyone.

---

**2. Compare this to `PreToolUse` from Week 2 Assignment 6. What does each one intercept, and what do they have in common?**

hooks/pre-commit intercepts Git commit actions before changes are saved to the repository. It checks staged files for issues like exposed secrets or files that are too large.

PreToolUse intercepts tool calls before they are executed, allowing checks or restrictions to be applied before an action happens.

Both act as preventive safeguards that run before an important operation occurs. They help enforce rules, improve security, and stop unsafe actions before they can cause problems.

---

# Task 3 — Prove the Hook Blocks the Risky Commit

## Goal

Attempt to commit the staged file from Task 1 and show the hook rejecting it.

### Evidence

#### Screenshot 4 — Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file

![rejected](A6T3S4.png)

---

### Notes

**1. Which line in `hooks/pre-commit` matched your fake key, and why did it match?**

if git diff --cached -- "$file" | grep -qE 'AKIA[0-9A-Z]{16}|-----BEGIN (RSA|OPENSSH|PRIVATE KEY)-----'; then
It matched because the fake key in scripts/notify.sh followed the AWS access key pattern checked by the regular expression: AKIA followed by 16 uppercase letters or numbers. The grep -qE command searched the staged file contents and detected this pattern, so the script marked it as a possible secret, set blocked=1, and rejected the commit.

---

**2. Could this hook have caught a poorly-named variable that stores a secret without the `AKIA` prefix? What does that tell you about the limits of a fixed rule like this?**

No, this hook would not catch a poorly named variable that stores a secret if the value does not match the patterns defined in the script.

The hook only looks for specific patterns, such as an AWS key beginning with AKIA or private key headers like -----BEGIN PRIVATE KEY-----.

This shows the limitation of fixed rules: they can detect known secret formats but may miss unknown, disguised, or custom secrets. More advanced tools or broader secret-scanning methods are needed for stronger protection.

---

# Task 4 — Build the `/pr-ready` Skill

## Goal

Create a manually invoked Claude Code skill that reads your staged changes and produces a PR-readiness report and a draft PR description — without writing, committing, or pushing anything itself.

### Evidence

#### Screenshot 5 — `SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`

![skillmd](A6T4S5.png)

---

#### Screenshot 6 — `/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement

![pr-ready](A6T4S6.png)

---

### Notes

**1. Why does `/pr-ready` have `Bash` and `Read` but not `Write`?**

/pr-ready has Bash and Read but not Write because it is designed only to inspect and review changes, not modify files.

Bash is needed to run commands like git diff --cached and git status to see what is staged.
Read is needed to examine files and understand the changes being reviewed.
No Write permission prevents the tool from editing files, automatically fixing issues, or changing the user's code.

This keeps the workflow safe because /pr-ready only creates a PR draft and risk report. A human must review the findings and make any required changes manually.

---

**2. The pre-commit hook and `/pr-ready` both looked at the same staged diff. Did they flag the same things? What did one catch that the other didn't?**

No, the pre-commit hook and /pr-ready did not flag exactly the same things.

The pre-commit hook only checked for specific technical patterns, such as AWS-style keys (AKIA...) and oversized files. It caught the credential-shaped string because the fake key matched the AKIA[0-9A-Z]{16} pattern and blocked the commit.

The /pr-ready review caught additional issues that the pre-commit hook did not detect:

It flagged the debug statement because the script echoed a credential-shaped value (echo "DEBUG: token is $AWS_ACCESS_KEY_ID").
It flagged the lack of accompanying notes/documentation explaining the purpose of notify.sh.

This shows that the pre-commit hook provides a simple automated safety check, while /pr-ready performs a broader code review that can identify quality, documentation, and maintainability issues beyond fixed patterns.

---

# Task 5 — Fix the Issues and Re-Verify

## Goal

Remove the secret and debug statement, then prove both gates now pass clean.

### Evidence

#### Screenshot 7 — `git commit` succeeding after the fix (no BLOCKED message)

![git commit](A6T5S7.png)

---

#### Screenshot 8 — Second `/pr-ready` run showing a clean risk report and a drafted PR title + description

![second PR](A6T5S8.png)

---

### Notes

**1. What exactly did you change to satisfy the pre-commit hook?**

To satisfy the pre-commit hook, I removed content that matched secret-detection patterns and eliminated unnecessary debug statements. I also ensured that all staged files were under the 1 MB size limit. After making these changes, I staged the updated files again. The commit then passed successfully because the hook detected no violations.

---

# Task 6 — Push and Open a Pull Request Using the AI Draft

## Goal

Push your branch and open a real Pull Request, using `/pr-ready`'s drafted title and description as your starting point — read it critically and edit before you use it.

**Important:** Open this Pull Request with base repository set to **your own fork** — not the shared upstream `pravinmishraaws/devops-micro-internship-pravinmishra` repository. This assignment's hook and skill files are your own practice work, not a change meant for the shared class repo.

### Evidence

#### Screenshot 9 — Your Pull Request showing the base repository is your own fork, plus the title and description, with the `/pr-ready` draft visible for comparison (paste it in the PR conversation or your notes below)

![PR](A6T5S9.png)

---

#### PR Link

https://github.com/pravinmishraaws/devops-micro-internship-pravinmishra/pull/74

---

### Notes

**1. What, if anything, did you edit in the AI's drafted PR description before using it? Why?**

I reviewed the AI-generated PR description and edited it to make sure it accurately described the changes I made, including the pre-commit hook, the /pr-ready skill, and the security checks. I removed or adjusted any wording that was unclear or did not match the actual implementation.

---

**2. If you had blindly copy-pasted the AI's draft without reading it, what could go wrong?**

The description could contain incorrect details, claim features that were not implemented, or miss important risks. This could mislead reviewers and reduce trust because AI output should be reviewed by a human before being used.

---

**3. Why does this PR need to target your own fork instead of the shared upstream repository?**

The PR should target my own fork because this assignment is for submitting my work, not contributing changes directly to the original upstream repository. This keeps my work isolated and prevents unintended changes from being submitted to the shared project.

---

# Task 7 — Map the Workflow to the Agentic Loop

## Goal

Explain this assignment's workflow using the same Gather → Analyze → Human Act → Verify structure from Week 3.

### Notes

**1. Which step(s) represent Gather?**

The Gather step is when the pre-commit hook and /pr-ready collect information from the staged changes using Git commands like git diff --cached and git status. They gather facts about files, possible secrets, and code changes.

---

**2. Which step(s) represent Analyze?**

The Analyze step is when the pre-commit hook checks fixed rules for secrets and large files, while /pr-ready reviews the changes and identifies risks like debug code, mixed changes, or missing context.

---

**3. Which step is Human Act, and why must a human — not Claude — run `git commit`, `git push`, and open the PR?**

Human Act is when the engineer fixes the issues, runs the Git commands, pushes the branch, and creates the Pull Request. A human must perform these actions because they change the shared repository, and the engineer must stay responsible for reviewing and approving changes.

---

**4. Which step is Verify?**

The Verify step is when the engineer confirms the commit succeeds, reruns /pr-ready, checks that risks are resolved, and reviews the Pull Request before submission.

---

**5. In one or two sentences: why do you need *both* the fixed-rule pre-commit hook and the AI skill? Isn't one enough?**

Both are needed because they solve different problems. The pre-commit hook provides fast and reliable rule-based protection, while the AI skill provides human-like review and context analysis that fixed rules cannot detect.

---

# Task 8 — LinkedIn Post

## Goal

Publish a LinkedIn post summarizing what you built and what you learned about combining fixed-rule safety checks with AI-assisted review.

### Evidence

#### LinkedIn Post URL

https://www.linkedin.com/posts/moses-avoseh_building-a-safer-git-workflow-with-ai-assisted-share-7486384114342850560-cJ9t/?utm_source=share&utm_medium=member_desktop&rcm=ACoAACZiz20BSL2chCMaU_0WK_2_7qktttgciMQ
---

## Key Learnings

Add 3-5 bullet points on what you learned this week.

-Learned how to build safety checks into the development workflow using Git hooks, and understood how fixed-rule automation can prevent security issues before code reaches a shared repository.
-Learned how to create AI-assisted workflows using Claude Code skills, including controlling permissions with allowed-tools and ensuring AI provides recommendations without directly modifying or pushing code.
-Improved my understanding of the Agentic Loop: Gather → Analyze → Human Act → Verify, and how this approach helps combine automation, AI assistance, and human decision-making in DevOps processes.

---

# Submission Instructions

- Ensure `hooks/pre-commit` and `.claude/skills/pr-ready/SKILL.md` are committed to your GitHub repository
- Add all required screenshots to your submission
- All written answers must be in your own words
- Do not use a real secret or credential anywhere in your submission — the fake key in Task 1 is intentional and must stay clearly fake
- Open your Pull Request against your own fork, not the shared upstream repository
- Push your final changes to your forked repository
- Include your PR link and LinkedIn post URL

---

## GitHub Repository URL

Paste your forked repository URL here:

`Add your URL here`

https://github.com/DMIC3-G3-Avoseh-Moses/devops-micro-internship-interviews.git

# Completion Checklist

- [✅ Completed] Branch `feature/ai-pr-ready` created with a staged file containing a fake secret and a debug statement
- [✅ Completed] `hooks/pre-commit` created and tracked in the repo (not only in `.git/hooks/`)
- [✅ Completed] `core.hooksPath` configured to point at `hooks/`
- [✅ Completed] Pre-commit hook shown blocking the risky commit
- [✅ Completed] `.claude/skills/pr-ready/SKILL.md` created with correct `allowed-tools` (no `Write`) and `disable-model-invocation: true`
- [✅ Completed] `/pr-ready` run against the risky diff and shown flagging issues
- [✅ Completed] Risky file fixed; `git commit` succeeds cleanly
- [✅ Completed] `/pr-ready` re-run showing a clean report and drafted PR title/description
- [ ] Pull Request opened using the AI draft as a starting point, with your own fork as the base repository (not upstream), PR link included
- [✅ Completed] Agentic Loop mapping (Task 7) completed in your own words
- [✅ Completed] LinkedIn post published and URL submitted
- [✅ Completed] All required screenshots added
- [✅ Completed] GitHub repository URL provided

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://pravinmishra.com/dmi  
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
