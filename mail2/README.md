# 📬 Mailpit Setup Guide (With Authentication)

Easily set up and test local email sending using **Mailpit** in Docker — now secured with **SMTP and Web UI authentication** 🔐

This guide will help you step-by-step 👇



## 🧱 1. Create the `docker-compose.yml` File

Create a file named **`docker-compose.yml`** and add the following content:

```yaml
services:
  mailpit:
    image: axllent/mailpit:latest
    # Alternative image:
    # image: ibraransaridocker/mailpit:latest
    container_name: mailpit
    ports:
      - "8025:8025" # 🌐 Web GUI
      - "1025:1025" # 📮 SMTP
    volumes:
      - ./mailpit-data:/data  # 💾 Persistent storage for messages
    environment:
      MP_SMTP_AUTH: "user2:pass2123"        # 👤 SMTP login credentials
      MP_UI_AUTH: "user2:pass2123"          # 🔑 Web UI login credentials
      MP_SMTP_AUTH_ALLOW_INSECURE: 1        # ⚠️ Allow auth without TLS (for local use only)
      MP_DATABASE: /data
      MP_MAX_MESSAGES: 5000                 # 💌 Max number of stored messages
      MP_VERBOSE: true                      # 🪵 Enable verbose logging
    healthcheck:
      test: ["CMD", "/mailpit", "readyz"]
      interval: 15s
      start_period: 10s
      timeout: 30s
      retries: 3
```



## 🔐 2. Understanding Username & Password

Mailpit can be secured so that only authorized users can access it:

| 🔧 Setting                    | 💬 Description                                                                       |
| -- | -- |
| `MP_SMTP_AUTH`                | Defines the **SMTP username and password** used by apps or scripts to send emails.   |
| `MP_UI_AUTH`                  | Defines the **Web UI username and password** to log in to the Mailpit web interface. |
| `MP_SMTP_AUTH_ALLOW_INSECURE` | Allows authentication over **non-TLS** connections (OK for local testing).           |

> 🧠 **Example credentials:**
>
> * **Username:** `user2`
> * **Password:** `pass2123`
>
> Use these for both sending emails and logging into the web interface.



## ▶️ 3. Start the Mailpit Container

Run this command in the same directory as your `docker-compose.yml`:

```bash
docker compose up -d
docker logs -f mailpit
```

✅ This will:

* Pull the Mailpit image
* Start the container
* Expose ports **8025 (Web)** and **1025 (SMTP)**



## 🌍 4. Access the Mailpit Web UI

Once the container is running, open your browser and go to:

👉 [http://localhost:8025/](http://localhost:8025/)
*(or replace `localhost` with your server IP, e.g. `192.168.1.100`)*

You’ll be prompted to log in:

* **Username:** `user2`
* **Password:** `pass2123`

🎉 After logging in, you can view, search, and inspect all test emails sent to Mailpit.



## ✉️ 5. Test Sending an Email via `curl`

You can test the SMTP connection using the credentials you configured:

```bash
curl --url 'smtp://localhost:1025' \
  --mail-rcpt 'recipient@example.com' \
  --user 'user2:pass2123' \
  --mail-from 'test@example.com' \
  -T - << EOF
Subject: Test Email 1
From: test@example.com
To: recipient@example.com

Hello from user2!
EOF
```

✅ If everything is configured correctly, you’ll see the email appear in the Mailpit Web UI inbox.



## 🧹 6. Stop and Clean Up

To stop the Mailpit container:

```bash
docker compose down
```

Your emails and settings remain saved inside the `./mailpit-data` directory 💾



## 🎉 Done!

You’ve successfully configured **Mailpit with authentication** in Docker! 🐳
Now you can safely test your app’s outgoing emails without exposing an open SMTP server.



### 💡 Pro Tips

* 🔒 **For production**: always use Mailpit behind a secured network or reverse proxy with HTTPS.
* 🧪 Use `user2` / `pass2123` in your app’s SMTP settings when testing locally.
* 📑 You can modify credentials easily by editing the `MP_SMTP_AUTH` and `MP_UI_AUTH` values.



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


