# Assignment 3 — Docker Networking

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will explore Docker network types and deploy applications using different networking modes: the default bridge network, a custom bridge network for microservices, multiple networks for a multi-tier architecture, and host networking.

---

# Task 1 — Deploy a Standalone Application Using Docker Bridge Network

## Goal

List Docker networks, verify/pull the Nginx image, run an Nginx container (`myweb`) on the default bridge network with port 80 mapped, and verify it's reachable from a browser via the VM's public IP.

### Evidence

#### Screenshot 1 — Output of `docker network ls`

![](screenshots/W11A3T1S1.png)

---

#### Screenshot 2 — Output of `docker images`

![](screenshots/W11A3T1S2.png)

---

#### Screenshot 3 — Output of `docker search nginx`

![](screenshots/W11A3T1S3.png)

---

#### Screenshot 4 — Successful `docker pull nginx` (if applicable)

![](screenshots/W11A3T1S4.png)

---

#### Screenshot 5 — Output of `docker ps` showing the running `myweb` container

![](screenshots/W11A3T1S5.png)

---

#### Screenshot 6 — Browser displaying the Nginx Welcome Page using the Public IP address

![](screenshots/W11A3T1S6.png)

---

# Task 2 — Connect Multiple Containers Using a Custom Bridge Network

## Goal

Create a custom bridge network `mynetwork`, build and run a Node/Express `frontend` container and an Nginx `backend` container on it, and verify they can communicate by container name.

### Evidence

#### Screenshot 1 — Output of `docker network create mynetwork`

![](screenshots/W11A3T2S1.png)

---

#### Screenshot 2 — Output of `docker network ls`

![](screenshots/W11A3T2S2.png)

---

#### Screenshot 3 — Frontend Dockerfile

![](screenshots/W11A3T2S3.png)

---

#### Screenshot 4 — Successful `docker build` for the frontend

![](screenshots/W11A3T2S4.png)

---

#### Screenshot 5 — Output of `docker ps` showing the frontend container

![](screenshots/W11A3T2S5.png)

---

#### Screenshot 6 — Output of `docker ps` showing both frontend and backend containers

![](screenshots/W11A3T2S6.png)

---

#### Screenshot 7 — Output of `docker network inspect mynetwork`

![](screenshots/W11A3T2S7.png)

---

#### Screenshot 8 — Successful `curl http://<Public-IP>` showing "Hello from Frontend"

![](screenshots/W11A3T2S8.png)

---

#### Screenshot 9 — Successful `curl backend` output from the frontend container showing the Nginx Welcome Page

![](screenshots/W11A3T2S9.png)

---

# Task 3 — Deploy a Multi-Tier Application Using Multiple Docker Networks

## Goal

Build a three-tier app (frontend, backend, MongoDB) across `backend-network` (backend ↔ database) and `frontend-network` (frontend ↔ backend), and verify data flows end to end.

### Evidence

#### Screenshot 1 — Creation of `backend-network`

![](screenshots/W11A3T3S1.png)

---

#### Screenshot 2 — Creation of `frontend-network`

![](screenshots/W11A3T3S2.png)

---

#### Screenshot 3 — Project folder structure

![](screenshots/W11A3T3S3.png)

---

#### Screenshot 4 — Database Dockerfile

![](screenshots/W11A3T3S4.png)

---

#### Screenshot 5 — Backend Dockerfile

![](screenshots/W11A3T3S5.png)

---

#### Screenshot 6 — Frontend Dockerfile

![](screenshots/W11A3T3S6.png)

---

#### Screenshot 7 — Successful Docker image builds (database, backend, frontend)

![](screenshots/W11A3T3S7.png)

---

#### Screenshot 8 — Running containers (`docker ps`)

![](screenshots/W11A3T3S8.png)

---

#### Screenshot 9 — Output of `docker network inspect backend-network`

![](screenshots/W11A3T3S9.png)

---

#### Screenshot 10 — Output of `docker network inspect frontend-network`

![](screenshots/W11A3T3S10.png)

---

#### Screenshot 11 — Browser showing the frontend application

![](screenshots/W11A3T3S11.png)

---

#### Screenshot 12 — Successful `curl api` from the frontend container

![](screenshots/W11A3T3S12.png)

---

#### Screenshot 13 — MongoDB connection using `mongosh`

![](screenshots/W11A3T3S13.png)

---

#### Screenshot 14 — Successful document insertion

![](screenshots/W11A3T3S14.png)

---

#### Screenshot 15 — Successful retrieval of the inserted document

![](screenshots/W11A3T3S15.png)

---

# Task 4 — Deploy an Application Using Docker Host Network Mode

## Goal

Deploy an Nginx container (`fastapp`) using Host Network Mode and verify it's reachable without explicit port mapping, then confirm the `NetworkMode` and clean up.

### Evidence

#### Screenshot 1 — Output of `docker run --network host`

![](screenshots/W11A3T4S1.png)

---

#### Screenshot 2 — Output of `docker ps` showing the running `fastapp` container

![Add your screenshot here.](screenshots/W11A3T4S1.png)

---

#### Screenshot 3 — Browser or terminal displaying the Nginx Welcome Page

![](screenshots/W11A3T4S3.png)

---

#### Screenshot 4 — Output of `docker inspect fastapp | grep "NetworkMode"`

![](screenshots/W11A3T4S4.png)

---

#### Screenshot 5 — Successful cleanup showing `docker stop fastapp` and `docker rm fastapp`

![](screenshots/W11A3T4S5.png)

---

# LinkedIn Post (Optional)

## Goal

Create a LinkedIn post covering the assignment objective, networking modes explored, key learning outcomes, and a short reflection on Docker networking concepts.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/posts/moses-avoseh_docker-networking-hands-on-from-default-share-7522846179748159488-IPT4/?utm_source=share&utm_medium=member_desktop&rcm=ACoAACZiz20BSL2chCMaU_0WK_2_7qktttgciMQ`

---

#### Screenshot — Published LinkedIn post

![](screenshots/W11A3T4S6.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- Do not expose sensitive information

---

# Completion Checklist

- [✅] Task 1: Standalone app on default bridge network deployed and verified (Screenshots 1–6)
- [✅] Task 2: Custom bridge network with frontend/backend communication verified (Screenshots 1–9)
- [✅] Task 3: Multi-tier app across two networks deployed and verified end to end (Screenshots 1–15)
- [✅] Task 4: Host network mode deployment verified and cleaned up (Screenshots 1–5)
- [✅] No sensitive information exposed

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
