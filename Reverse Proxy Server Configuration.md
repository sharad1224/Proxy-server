## Reverse Proxy Server

Architecture (Simple Diagram)
```bash

                Internet User
                      │
                      │
              ┌─────────────────┐
              │ Reverse Proxy   │
              │ Ubuntu + Nginx  │
              │ 192.168.1.5     │
              └────────┬────────┘
                       │
        ┌──────────────┴──────────────┐
        │                             │
 ┌──────────────┐              ┌──────────────┐
 │ Backend App1 │              │ Backend App2 │
 │ 192.168.1.10 │              │ 192.168.1.11 │
 │ Port 3000    │              │ Port 5000    │
 └──────────────┘              └──────────────┘

```

### Step 1 — Install Ubuntu Server

You need a system with Ubuntu installed.

Check version:
```bash
lsb_release -a
```
Update system:
```bash
sudo apt update
sudo apt upgrade -y
```
### Step 2 — Install Nginx

Install Nginx web server.
```bash
sudo apt install nginx -y
sudo systemctl status nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```
Check browser:
```bash
curl localhost             #http://server-ip
```

### Step 3 — Create Backend Test Server

Let's create two backend servers using nginx web server (for testing).

Backend Website 1:
```bash
mkdir backend1
cd backend1
```
```bash
vim index.html
```
```bash
<!DOCTYPE html>
<html>
<head>
<title>Backend Server 1</title>
</head>
<body>

<h1>This is Backend Server 1</h1>
<p>Running on Port 3000</p>

</body>
</html>
```

Backend Website 2:
```bash
mkdir backend2
cd backend2
```
```bash
vim index.html
```
```bash
<!DOCTYPE html>
<html>
<head>
<title>Backend Server 2</title>
</head>
<body>

<h1>This is Backend Server 2</h1>
<p>Running on Port 5000</p>

</body>
</html>
```
Check file permission 
```bash
ls -l /home/sharad
sudo chmod -R 755 /home/kiosk/backend1
sudo chmod -R 755 /home/kiosk/backend2
sudo chmod 755 /home/kiosk
```


### Step 4 — Backend Servers Run

Configure Backend1:
```bash
vim /etc/nginx/sites-available/backend1
```
```bash
server {
    listen 3000;

    root /home/kiosk/backend1;
    index index.html;
}
```
Configure Backend2:
```bash
vim /etc/nginx/sites-available/backend2
```
```bash
server {
    listen 5000;

    root /home/kiosk/backend2;
    index index.html;
}
```

### Step 5 — Sites enable

```bash
sudo ln -s /etc/nginx/sites-available/backend1 /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/backend2 /etc/nginx/sites-enabled/
```
Default site disable:
```bash
sudo rm /etc/nginx/sites-enabled/default
```


### Step 6 — Nginx reload

```bash
sudo nginx -t
sudo systemctl reload nginx
```

Step 7 — Test Backend Servers

Open Browser:
http://server-ip:3000
http://server-ip:5000

```bash
curl localhost:3000
curl localhost:5000
```

### Step 7 — Now Configure Reverse Proxy

```bash
sudo vim /etc/nginx/sites-available/reverse-proxy
```
```bash
server {

    listen 80;

    location /app1/ {
        proxy_pass http://localhost:3000/;
    }

    location /app2/ {
        proxy_pass http://localhost:5000/;
    }

}
```
Reload nginx:
```bash
sudo nginx -t
sudo systemctl reload nginx
```

### Step 8 — Final Test

Open Browser:
 
http://server-ip/app1
http://server-ip/app2

```bash
curl http://server-ip/app1
curl http://server-ip/app2
```

Reverse proxy enable:
```bash
sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/
```

Default site disable:
```bash
sudo rm /etc/nginx/sites-enabled/default
```

```bash
sudo nginx -t
sudo systemctl restart nginx
```


final test
```bash
curl http://localhost/app1/
curl http://localhost/app2/
































