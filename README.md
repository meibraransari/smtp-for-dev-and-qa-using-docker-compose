# 📬 Self-Host Your SMTP Server for DEV and QA Using Docker Compose

Easily **self-host your own SMTP testing server** for **development (DEV)** and **quality assurance (QA)** using [Mailpit](https://github.com/axllent/mailpit).

Mailpit lets you capture, inspect, and debug outgoing emails from your applications — all in a clean, web-based UI.
This repository contains **three example configurations** showing different levels of security and complexity using **Docker Compose**.

It introduces all three examples, explains when to use each, and provides quick links, structure, and setup tips.
You can place this file at the root of your project, alongside the `mail1/`, `mail2/`, and `mail3/` folders.



## 🧭 Overview

| Folder | Description | Auth Type | Recommended For |
|--------|------------|-----------|----------------|
| [`mail1/`](./mail1) | Basic setup — no authentication | ❌ None | Local DEV / personal testing |
| [`mail2/`](./mail2) | Single-user authentication | 🔑 Username + Password | Individual developer testing |
| [`mail3/`](./mail3) | Multi-user authentication (file-based) | 👥 Multiple accounts | Shared QA or team environments |


## ⚙️ What You’ll Learn

* 🐳 How to run **Mailpit** in Docker
* 🔐 How to secure SMTP and Web UI with authentication
* 👥 How to manage multiple users via config files
* 💌 How to test email sending from apps or scripts
* 🧪 How to set up isolated mail servers for DEV & QA



## 🧱 Folder Structure

```
project-root/
├── mail1/
│   ├── docker-compose.yml
│   └── README.md          # No-auth Mailpit setup
│
├── mail2/
│   ├── docker-compose.yml
│   └── README.md          # Single-user authenticated Mailpit
│
├── mail3/
│   ├── docker-compose.yml
│   ├── config/
│   │   ├── smtp-auth.txt
│   │   └── ui-auth.txt
│   └── README.md          # Multi-user Mailpit setup
│
└── README.md              # ← Main overview file (this one)
```



## 🧩 Mailpit Flow (Simplified Diagram)

```
           ┌──────────────────────────────┐
           │        Your Application      │
           │  (Laravel, Django, Node.js)  │
           └──────────────┬───────────────┘
                          │
                  SMTP (port 1025)
                          │
                 ┌────────────────┐
                 │    Mailpit     │
                 │  (Dockerized)  │
                 │ SMTP + Web UI  │
                 └──────┬─────────┘
                        │
                  Web UI (port 8025)
                        │
                        ▼
             ┌──────────────────────────┐
             │  Browser / Developer UI  │
             │  Inspect, Search, Debug  │
             └──────────────────────────┘
```



## 🚀 Quick Start

### 1️⃣ Choose a setup

Pick one of the examples based on your needs:

* **[`mail1/`](./mail1)** → simplest setup (no auth)
* **[`mail2/`](./mail2)** → single-user (auth via env vars)
* **[`mail3/`](./mail3)** → multi-user (auth via config files)

### 2️⃣ Run the container

```bash
git clone https://github.com/meibraransari/SMTP-For-DEV-And-QA-Using-Docker-Compose.git
cd SMTP-For-DEV-And-QA-Using-Docker-Compose
cd mail1   # or mail2, mail3
docker compose up -d
```

### 3️⃣ Access the Web UI

Open your browser:

👉 [http://localhost:8025](http://localhost:8025)

*(or use your local IP if on a LAN, e.g., `http://192.168.1.100:8025`)*

### 4️⃣ Send a Test Email

You can test with `curl` (or from your app):

```bash
curl --url 'smtp://localhost:1025' \
  --mail-from 'test@example.com' \
  --mail-rcpt 'recipient@example.com' \
  -T - << EOF
Subject: Test
From: test@example.com
To: recipient@example.com

Hello from Mailpit!
EOF
```

If you’re using authentication (mail2 or mail3), include the `--user` flag:

```bash
--user 'username:password'
```



## 🔐 Authentication Options Summary

| Setup     | SMTP Auth                   | Web UI Auth      | Multiple Users | Notes                          |
| :-- | :-- | :-- | :-- | :-- |
| **Mail1** | Disabled                    | None             | ❌              | Quick & easy for local testing |
| **Mail2** | Single user (env vars)      | Same credentials | ❌              | Simple secure testing          |
| **Mail3** | File-based (from `/config`) | File-based       | ✅              | Ideal for shared DEV/QA        |



## 🧠 Recommended Usage Scenarios

| Environment           | Recommended Setup | Description                                           |
| :-- | :- | :- |
| **Local DEV**         | `mail1`           | Lightweight, no password, for quick testing.          |
| **Staging / QA**      | `mail2`           | Auth-protected SMTP & Web UI for controlled access.   |
| **Shared DEV Server** | `mail3`           | Multi-user setup for team testing with unique logins. |



## 🧹 Maintenance

To stop and remove containers:

```bash
docker compose down
```

To clear all messages (optional):

```bash
rm -rf mailpit-data/
```



## 💡 Pro Tips

* 🧪 Use Mailpit’s **Web UI search & filters** to debug easily.
* 🔒 Use **bcrypt hashed passwords** in `smtp-auth.txt` for better security.
* 📦 Persist data with Docker volumes (`mailpit-data`).
* ⚙️ Integrate Mailpit with CI/CD pipelines for automated email checks.



## 🧾 References

* 📘 [Mailpit Documentation](https://github.com/axllent/mailpit)
* 🐳 [Docker Hub: axllent/mailpit](https://hub.docker.com/r/axllent/mailpit)
* 💌 [SMTP Protocol Basics (MDN)](https://developer.mozilla.org/en-US/docs/Glossary/SMTP)



## 🎉 Done!

You’re now ready to **self-host your own SMTP testing environment** for both **development** and **QA** stages — safely, locally, and fully containerized! 🚀

Each example (`mail1`, `mail2`, `mail3`) offers a different balance of simplicity and security.
Pick the one that fits your team’s workflow and start catching those test emails 🧑‍💻💌


## 🧠 About This Project

This project is maintained by **DevOps In Action**, mainly focusing on practical, hands-on DevOps setups for CI/CD automation, containerization, and infrastructure management.

### 💼 Connect with Me 👇😊

* 🔥 [**YouTube**](https://www.youtube.com/@DevOpsinAction?sub_confirmation=1)
* ✍️ [**Blog**](https://ibraransari.blogspot.com/)
* 💼 [**LinkedIn**](https://www.linkedin.com/in/ansariibrar/)
* 👨‍💻 [**GitHub**](https://github.com/meibraransari?tab=repositories)
* 💬 [**Telegram**](https://t.me/DevOpsinActionTelegram)
* 🐳 [**Docker Hub**](https://hub.docker.com/u/ibraransaridocker)


### ⭐ If You Found This Helpful...

***Please star the repo and share it! Thanks a lot!*** 🌟


