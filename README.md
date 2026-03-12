# Nginx Reverse Proxy Lab (Ubuntu)

## 📌 Project Overview

This project demonstrates how to configure a **Reverse Proxy Server
using Nginx on Ubuntu**.\
A reverse proxy sits in front of backend servers and forwards client
requests to the appropriate backend service.

In this lab: - **Nginx (Port 80)** acts as the **Reverse Proxy** -
**Backend Server 1 (Port 3000)** serves a static HTML page - **Backend
Server 2 (Port 5000)** serves another static HTML page

Users access the backend servers through a single entry point (the
reverse proxy).

------------------------------------------------------------------------

## 🏗 Architecture

                    User Browser
                         │
                         ▼
                Nginx Reverse Proxy
                     (Port 80)
                         │
               ┌─────────┴─────────┐
               ▼                   ▼
         Backend Server 1     Backend Server 2
             Port 3000            Port 5000
             HTML Page            HTML Page

------------------------------------------------------------------------

## 📂 Project Structure

    nginx-reverse-proxy-lab/
    │
    ├── backend1/
    │   └── index.html
    │
    ├── backend2/
    │   └── index.html
    │
    └── nginx-config/
        ├── backend1.conf
        ├── backend2.conf
        └── reverse-proxy.conf

------------------------------------------------------------------------

## ⚙ Requirements

-   Ubuntu Server (20.04 / 22.04 / 24.04)
-   Nginx installed
-   Basic Linux command knowledge

Install Nginx:

``` bash
sudo apt update
sudo apt install nginx -y
```

------------------------------------------------------------------------

## 🚀 Step‑by‑Step Setup

### 1️⃣ Create Backend Website 1

``` bash
mkdir ~/backend1
nano ~/backend1/index.html
```

Example HTML:

``` html
<html>
<body>
<h1>Backend Server 1</h1>
<p>This page is running on port 3000</p>
</body>
</html>
```

------------------------------------------------------------------------

### 2️⃣ Create Backend Website 2

``` bash
mkdir ~/backend2
nano ~/backend2/index.html
```

Example HTML:

``` html
<html>
<body>
<h1>Backend Server 2</h1>
<p>This page is running on port 5000</p>
</body>
</html>
```

------------------------------------------------------------------------

### 3️⃣ Configure Backend Nginx Server (Port 3000)

    sudo nano /etc/nginx/sites-available/backend1

    server {
        listen 3000;
        root /home/USERNAME/backend1;
        index index.html;
    }

------------------------------------------------------------------------

### 4️⃣ Configure Backend Nginx Server (Port 5000)

    sudo nano /etc/nginx/sites-available/backend2

    server {
        listen 5000;
        root /home/USERNAME/backend2;
        index index.html;
    }

------------------------------------------------------------------------

### 5️⃣ Enable Backend Sites

``` bash
sudo ln -s /etc/nginx/sites-available/backend1 /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/backend2 /etc/nginx/sites-enabled/
```

------------------------------------------------------------------------

### 6️⃣ Create Reverse Proxy Configuration

    sudo nano /etc/nginx/sites-available/reverse-proxy

    server {

        listen 80;

        location /app1/ {
            proxy_pass http://localhost:3000/;
        }

        location /app2/ {
            proxy_pass http://localhost:5000/;
        }

    }

------------------------------------------------------------------------

### 7️⃣ Enable Reverse Proxy

``` bash
sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
```

------------------------------------------------------------------------

### 8️⃣ Restart Nginx

``` bash
sudo nginx -t
sudo systemctl restart nginx
```

------------------------------------------------------------------------

## 🧪 Testing

Open in browser:

    http://SERVER-IP/app1/

Output:

    Backend Server 1

Test second backend:

    http://SERVER-IP/app2/

Output:

    Backend Server 2

------------------------------------------------------------------------

## 🔄 Request Flow

    User → http://server-ip/app1
            │
            ▼
       Nginx Reverse Proxy
            │
            ▼
       localhost:3000

------------------------------------------------------------------------

## 🎯 Key Concepts Learned

-   What is a **Reverse Proxy**
-   How **Nginx routes requests using `location`**
-   Using **multiple backend servers**
-   **Port-based web services**
-   Basic **DevOps web architecture**

------------------------------------------------------------------------

## 💡 Real‑World Use Cases

Reverse proxies are used for:

-   Load balancing
-   Security and hiding backend servers
-   SSL termination
-   Microservices routing

Popular companies using reverse proxy architectures include:

-   Netflix
-   Amazon
-   Google

------------------------------------------------------------------------

## 👨‍💻 Author

Sharad Patel

GitHub: https://github.com/sharad1224

