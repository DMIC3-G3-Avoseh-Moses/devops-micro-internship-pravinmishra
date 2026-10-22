# Assignment 1 — Onboarding: Workstation Setup, Standards & AI (Go Beyond)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will set up a production-ready Ansible development workstation following real-world team standards: an isolated Python environment, VS Code tooling, SSH readiness, Git hygiene, pre-commit hooks, and reproducible onboarding documentation.

---

# Task 1 — Environment & Ansible Install

## Goal

Create and activate an isolated `.venv` inside `ansible-onboarding/`, install `ansible`, `ansible-lint`, and `yamllint`, and save dependencies to `requirements.txt`.

### Evidence

#### Screenshot 1 — Terminal showing the activated `.venv` and successful `ansible --version` output

![](screenshots/W9A1T1S1.png)

---

#### Screenshot 2 — Terminal showing successful `ansible-lint --version` output and `requirements.txt`

![](screenshots/W9A1T1S2.png)

---

# Task 2 — VS Code Setup (Tooling That Teams Expect)

## Goal

Install the Ansible, YAML, and Python VS Code extensions, and create `.vscode/settings.json` and `.editorconfig` with the team-standard settings.

### Evidence

#### Screenshot 3 — VS Code Extensions panel showing Ansible, YAML, and Python installed

![](screenshots/W9A1T2S3.png)

---

#### Screenshot 4 — VS Code showing `.vscode/settings.json` and `.editorconfig`

![](screenshots/W9A1T2S4.png)

---

# Task 3 — Baseline ansible.cfg (Team-Friendly Defaults)

## Goal

Create `ansible.cfg` in the project root with the team-friendly defaults and SSH connection settings, and confirm Ansible reads it.

### Evidence

#### Screenshot 5 — VS Code or terminal showing `ansible.cfg` in the project root with the supplied settings

![](screenshots/W9A1T3S5.png)

---

# Task 4 — SSH Readiness (Enterprise Basics)

## Goal

Generate or use an Ed25519 SSH key, load it into `ssh-agent`, and configure `~/.ssh/config` with safe defaults.

### Evidence

#### Screenshot 6 — Terminal showing `ssh-add -l` with the key loaded (do not expose private-key contents)

![](screenshots/W9A1T4S6.png)

---

# Task 5 — Git Identity, Signing & Hooks

## Goal

Configure Git identity and the `main` default branch, install `pre-commit`, add `.pre-commit-config.yaml` with `yamllint` and `ansible-lint` hooks, and run the hooks as a smoke test.

### Evidence

#### Screenshot 7 — Terminal showing `pre-commit install` output

![](screenshots/W9A1T5S7.png)

---

#### Screenshot 8 — Terminal showing `pre-commit run --all-files` passing

![](screenshots/W9A1T5S8.png)

---

# Task 6 — README + Checklist

## Goal

Document the workstation setup in `README.md`, including a "New Machine? Do This" checklist with 10–12 bullet points.

### Evidence

#### Screenshot 9 — Repository tree showing the required files

![](screenshots/W9A1T6S9.png)

---

#### Screenshot 10 — `README.md` showing machine details and the "New Machine? Do This" checklist

![](screenshots/W9A1T6S10.png)

---

### Notes

State one thing that makes this setup team-friendly, and one pitfall you avoided (e.g. global pip, missing SSH agent). Note any corporate proxy or CA certificate steps, if applicable.

### Notes

**Team-friendly feature:** Pre-commit hooks (yamllint and ansible-lint) run automatically before every commit, and all tools are installed in a project-local .venv with versions saved in requirements.txt. Every engineer gets the same environment and the same checks, so commits stay clean and consistent.

**Pitfall avoided:** I avoided installing Ansible globally with sudo/apt. Ubuntu suggested "sudo apt install ansible" when the .venv wasn't active, but I created the virtual environment and installed everything inside it, so the system Python stays untouched. I also kept the SSH private key in ~/.ssh (outside the project) and excluded it through .gitignore.

**Deviation (Python version):** The official ansible-lint pre-commit hook needs Python 3.14 to build its own environment, but Ubuntu 22.04 only has Python 3.10. I replaced it with a local hook that runs the ansible-lint installed in .venv. Both hooks pass in pre-commit run --all-files.

**Environment:** Ubuntu 22.04.5 LTS on WSL2 (Windows), edited through VS Code Remote - WSL. Ubuntu was imported manually from the official rootfs image because the standard WSL installer hung.

**Proxy / CA certificates:** Not applicable. No corporate proxy or custom CA certificate steps were needed.

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not commit the `.venv` directory, SSH private keys, passwords, tokens, or other secrets

---

# Completion Checklist

- [✅ Completed] Task 1: Isolated environment created, Ansible and lint tools installed (Screenshots 1–2)
- [✅ Completed] Task 2: VS Code extensions and workspace settings configured (Screenshots 3–4)
- [✅ Completed] Task 3: `ansible.cfg` created with team defaults (Screenshot 5)
- [✅ Completed] Task 4: SSH key generated and loaded into agent (Screenshot 6)
- [✅ Completed] Task 5: Git identity configured and pre-commit hooks passing (Screenshots 7–8)
- [✅ Completed] Task 6: README and checklist completed (Screenshots 9–10)
- [✅ Completed] Team-friendly choice / pitfall notes written (Notes)
- [✅ Completed] No private keys or secrets exposed

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
