# 📬 Mailpit Setup Guide (File-Based Authentication)

This guide explains how to set up **Mailpit** in Docker with **multi-user authentication** for both SMTP and the Web UI.
It also includes a visual diagram showing how email flow works through Mailpit 🔐



## 🧱 1. Create the `docker-compose.yml` File

Create a file named **`docker-compose.yml`** and paste the following content:

```yaml
services:
  mailpit:
    image: axllent/mailpit:latest
    #image: ibraransaridocker/mailpit:latest
    container_name: mailpit
    restart: unless-stopped
    ports:
      - "8025:8025"  # Web UI
      - "1025:1025"  # SMTP
    environment:
      TZ: Europe/London
      MP_VERBOSE: true # Enable debug logging 
      # Database
      MP_MAX_MESSAGES: 5000 
      MP_DATABASE: /data   
      # SMTP Authentication (file-based - multiple users)
      MP_SMTP_AUTH_FILE: /configs/smtp-auth.txt
      MP_SMTP_AUTH_ACCEPT_ANY: 0  # Require authentication
      MP_SMTP_AUTH_ALLOW_INSECURE: 1
      
      # Web UI Authentication (file-based - multiple users)
      MP_UI_AUTH_FILE: /configs/ui-auth.txt
      
      # Optional: TLS Support
      # MP_SMTP_TLS_CERT: /certs/cert.pem
      # MP_SMTP_TLS_KEY: /certs/key.pem
      
    volumes:
      - ./mailpit-data:/data  # Separate database
      - ./configs:/configs:ro  # Mount configs directory as read-only
      # Optional: TLS certificates
      # - ./certs:/certs:ro
    healthcheck:
      test: ["CMD", "/mailpit", "readyz"]
      interval: 15s
      start_period: 10s
      timeout: 30s      # optional (default is 30s)
      retries: 3        # optional (default is 3)

```



## 📁 2. Create the configsuration Directory

Create a folder named **`configs`** in the same directory as your `docker-compose.yml`:

```bash
mkdir configs
```

Inside the `configs` folder, create the following two files 👇



### 📨 `configs/smtp-auth.txt`

Used for **SMTP authentication** — your apps or scripts use these credentials to send emails.

**Format:**

```
username:password
```

Example:

```
user1:user1123
user2:user2123
admin:admin123
```

> 💡 **Tip:** You can use **bcrypt hashed passwords** for stronger security:
>
> ```
> user1:$2a$10$abcdefghijklmnopqrstuvwxyz...
> ```



### 🌐 `configs/ui-auth.txt`

Used for **Web UI authentication** — you’ll need these credentials to log into Mailpit’s dashboard.

**Format:**

```
username:password
```

Example:

```
admin:admin123
user1:user1123
viewer:viewer123
```



## 🧠 3. Understanding Authentication

| 🔧 Setting                    | 💬 Description                                               |
| -- | -- |
| `MP_SMTP_AUTH_FILE`           | Points to the file listing all SMTP usernames and passwords. |
| `MP_UI_AUTH_FILE`             | Points to the file listing Web UI usernames and passwords.   |
| `MP_SMTP_AUTH_ACCEPT_ANY`     | `0` means authentication is required.                        |
| `MP_SMTP_AUTH_ALLOW_INSECURE` | Allows login over plain SMTP (for local use only).           |
| `MP_MAX_MESSAGES`             | Sets how many emails Mailpit will store.                     |

> 🧩 This setup allows **multiple users** to connect securely for both sending and viewing emails.



## ▶️ 4. Start Mailpit

Run the following command:

```bash
docker compose up -d
docker logs -f mailpit
```

✅ This will:

* Pull the Mailpit image
* Start the container
* Expose ports **8025 (Web)** and **1025 (SMTP)**



## 🌍 5. Access the Web UI

Open your browser and go to:

👉 [http://localhost:8025/](http://localhost:8025/)
(or your host IP, e.g., `http://192.168.1.100:8025/`)

Log in with one of the Web UI users (from `ui-auth.txt`):

* **Username:** `admin`
* **Password:** `admin123`

🎉 You’ll now see the Mailpit dashboard with incoming test emails!



## ✉️ 6. Send a Test Email

Use `curl` or your app’s SMTP configsuration to test sending an email:

```bash
curl --url 'smtp://localhost:1025' \
  --mail-from 'test@example.com' \
  --mail-rcpt 'recipient@example.com' \
  --user 'user2:user2123' \
  -T - << EOF
Subject: Test Email 2
From: test@example.com
To: recipient@example.com

Hello from user2!
EOF
```

✅ Check the Mailpit UI — your message should appear instantly.



## 🧹 7. Stop and Clean Up

Stop Mailpit safely:

```bash
docker compose down
```

Your stored messages and configs remain in:

* `./mailpit-data` → mail storage
* `./configs` → authentication files



## 🧾 8. Example Folder Structure

```
project-folder/
├── docker-compose.yml
├── mailpit-data/
└── configs/
    ├── smtp-auth.txt
    └── ui-auth.txt
```



## 🖼️ 9. Mailpit Flow Diagram

Here’s a simple visual of how Mailpit works in your local environment 👇

```
           ┌──────────────────────────────┐
           │        Your App / Tool       │
           │  (e.g., Laravel, Node.js)    │
           └──────────────┬───────────────┘
                          │
                 SMTP (port 1025)
                          │
            Auth → user2 : user2123
                          │
                 ┌────────────────┐
                 │    Mailpit     │
                 │  (Dockerized)  │
                 │  SMTP + WebUI  │
                 └──────┬─────────┘
                        │
                  Web UI (port 8025)
                        │
                        ▼
             ┌──────────────────────────┐
             │   Browser / Developer     │
             │ (Login via ui-auth.txt)   │
             │   admin / user / viewer   │
             └──────────────────────────┘
```

📩 **Flow Summary:**

1. Your application sends an email via SMTP on port **1025**.
2. Mailpit intercepts and stores the message locally (no external delivery).
3. Developers access the **Web UI (8025)** to view and test emails securely.



## 🎉 You’re Done!

You now have a **secure, multi-user Mailpit setup** running in Docker 🐳
Perfect for local email testing with authentication and optional TLS support.



### 💡 Pro Tips

* 🔒 Use **bcrypt-hashed passwords** in production-like environments.
* 🧪 Create multiple users for different roles (developer, tester, admin).
* 📦 Backup the `mailpit-data` folder regularly if you need to persist emails.
* 🧰 Works great with frameworks like Laravel, Symfony, Django, or Node.js apps.




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


