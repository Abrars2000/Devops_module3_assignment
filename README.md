# Devops_module3_assignment







########                        All screenshots are at the end of this file                   #########







#  Nginx Web Server with HTTPS, SSL & Reverse Proxy

##  Project Overview

This project demonstrates a production-like secure web server setup using **Nginx on Linux**.
It includes static hosting, HTTPS security using self-signed SSL, HTTP to HTTPS redirection, and reverse proxy to a backend service running on port 3000.

---

#  Features

*  Static website hosting using Nginx
*  HTTPS enabled using self-signed SSL (OpenSSL)
*  Automatic HTTP → HTTPS redirect
*  Reverse proxy to backend (Node.js on port 3000)
*  Full testing and validation

---

#  Part 1: Basic Setup

## Install Nginx & OpenSSL

```bash
sudo apt update
sudo apt install nginx openssl -y
```

---

## Create Web Directory

```bash
sudo mkdir -p /var/www/secure-app
echo "<h1>Secure Server Running via Nginx</h1>" | sudo tee /var/www/secure-app/index.html
```

---

#  Part 2: SSL Configuration

## Generate Self-Signed SSL (365 days)

```bash
sudo mkdir -p /etc/nginx/ssl

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/nginx/ssl/secure.key \
-out /etc/nginx/ssl/secure.crt
```

---

#  Part 3: Nginx Configuration

## File Location

```
/etc/nginx/sites-available/default
```

## Nginx Config

```nginx
server {
    listen 80;
    server_name _;

    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name _;

    ssl_certificate /etc/nginx/ssl/secure.crt;
    ssl_certificate_key /etc/nginx/ssl/secure.key;

    root /var/www/secure-app;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location /api/ {
        proxy_pass http://localhost:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

#  Part 4: Backend (Reverse Proxy)

## Run Backend on Port 3000

Example (Node.js):

```bash
node app.js
```

Backend will run on:

```
http://localhost:3000
```

---

#  Restart Nginx

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

#  Part 5: Testing

## Check Nginx Config

```bash
nginx -t
```


Screenshots :

<img width="812" height="320" alt="ss_3" src="https://github.com/user-attachments/assets/3b658ba3-1243-44bf-9798-3d1a42941d06" />
<img width="962" height="891" alt="Screenshot _6" src="https://github.com/user-attachments/assets/f6957381-fc0c-4414-bdfb-bd8a15ef913b" />
<img width="935" height="767" alt="Screenshot _5" src="https://github.com/user-attachments/assets/ead54cb8-bf2b-482e-9ed8-2a162bedfbb1" />
<img width="817" height="122" alt="Screenshot _4" src="https://github.com/user-attachments/assets/ba91be5d-bae1-4548-88a5-64f04e36c083" />
<img width="960" height="1020" alt="Screenshot _2" src="https://github.com/user-attachments/assets/e5d509a6-11ca-4e67-a967-3d4474efd808" />
<img width="1920" height="1020" alt="Screenshot _1" src="https://github.com/user-attachments/assets/0fcb81c6-e29b-4159-83e0-f1c716e5acd2" />




