# 📬 Mailpit Setup Guide (Docker)

Easily set up and test email sending locally using **Mailpit** with Docker.
This guide walks you through configuration, startup, and testing — step by step 🚀


## 🧱 1. Create Your `docker-compose.yml`

Create a file named **`docker-compose.yml`** and paste the following configuration:

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
      - ./mailpit-data:/data  # 💾 Persistent storage
    environment:
      MP_SMTP_AUTH_ALLOW_INSECURE: 1
      MP_SMTP_AUTH_ACCEPT_ANY: 1  # 🚫 Disable authentication
      MP_DATABASE: /data
      MP_MAX_MESSAGES: 5000       # 💌 Max messages stored
      MP_VERBOSE: true            # 🪵 Enable debug logging
      MP_LOG_FILE: 
    healthcheck:
      test: ["CMD", "/mailpit", "readyz"]
      interval: 15s
      start_period: 10s
      timeout: 30s
      retries: 3
```



## ▶️ 2. Start the Mailpit Container

Run the following command in the same directory as your `docker-compose.yml`:

```bash
docker compose up -d
docker logs -f mailpit
```

✅ This will pull the Mailpit image, start the container, and expose the ports.



## 🌍 3. Access the Web UI

Once the container is running, open the Mailpit interface in your browser:

👉 [http://192.168.1.100:8025/](http://192.168.1.100:8025/)

> 💡 Replace `192.168.1.100` with your host IP if needed.



## ✉️ 4. Test Sending an Email (No Auth Required)

You can send a test email using `curl` — no username or password needed:

```bash
curl --url 'smtp://localhost:1025' \
  --mail-rcpt 'recipient@example.com' \
  --mail-from 'test@example.com' \
  -T - << EOF
Subject: Test Email 1
From: test@example.com
To: recipient@example.com

Hello from without User name and Pass!
EOF
```

✅ After running this command, open your Mailpit GUI to view the received email.



## 🧹 5. Stop and Clean Up

When you're done testing, stop the container:

```bash
docker compose down
```

Your emails and settings are safely stored in `./mailpit-data`.



## 🎉 Done!

You’ve successfully set up **Mailpit** for local email testing with Docker!
Use it to debug and preview your app’s outgoing emails easily. 🧑‍💻💌



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


