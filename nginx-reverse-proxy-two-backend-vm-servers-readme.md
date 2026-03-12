# Nginx Reverse Proxy Setup with Two Ubuntu Backend Servers

## Project Overview

This project demonstrates how to configure an **Nginx Reverse Proxy** on
a local Ubuntu machine that routes traffic to **two separate Ubuntu
virtual machines**, each running its own Nginx web application.

Both backend servers run on the **same port (80)**, and the reverse
proxy distributes traffic based on **URL path routing**.

This setup is commonly used in **DevOps environments, microservices
architectures, and production web infrastructures**.

------------------------------------------------------------------------

# Architecture

Client requests are first received by the **Reverse Proxy Server**.\
The proxy then forwards the request to the appropriate backend server.

                    Client Browser
                           |
                           |
                   Reverse Proxy Server
                   (Local Ubuntu + Nginx)
                           |
                   -------------------
                   |                 |
                VM1 Ubuntu        VM2 Ubuntu
                Web App 1         Web App 2
                Nginx : 80        Nginx : 80

### Routing Rules
```bash
  Request URL   Destination
  ------------- ---------------------
  `/app1`       VM1 Web Application
  `/app2`       VM2 Web Application
```
Example:
```bash
    http://proxy-ip/app1  → VM1
    http://proxy-ip/app2  → VM2
```
------------------------------------------------------------------------

# Environment Setup
#### Network Mode = Bridge
```bash
  Machine                Role
  ---------------------- --------------------------
  Local Ubuntu Machine   Reverse Proxy Server
  Ubuntu VM1             Web Application Server 1
  Ubuntu VM2             Web Application Server 2
```
Example IP configuration:
```bash
  Server          Example IP
  --------------- --------------
  Reverse Proxy   192.168.1.10
  VM1             192.168.1.11
  VM2             192.168.1.12
```
Check IP using:

``` bash
ip a
```

------------------------------------------------------------------------

# Step 1 --- Install Nginx on VM1

    sudo apt update
    sudo apt install nginx -y

Enable and start service:

    sudo systemctl enable nginx
    sudo systemctl start nginx

------------------------------------------------------------------------

# Step 2 --- Create Web Application Page (VM1)

Edit index file:

    sudo nano /var/www/html/index.html

Example content:

``` html
<h1>Welcome to Web Application 1</h1>
<p>This page is served from VM1</p>
```

Restart nginx:

    sudo systemctl restart nginx

Test:

    http://VM1-IP

------------------------------------------------------------------------

# Step 3 --- Install Nginx on VM2

    sudo apt update
    sudo apt install nginx -y

Start nginx:

    sudo systemctl enable nginx
    sudo systemctl start nginx

------------------------------------------------------------------------

# Step 4 --- Create Web Application Page (VM2)

Edit file:

    sudo nano /var/www/html/index.html

Example content:

``` html
<h1>Welcome to Web Application 2</h1>
<p>This page is served from VM2</p>
```

Restart nginx:

    sudo systemctl restart nginx

Test:

    http://VM2-IP

------------------------------------------------------------------------

# Step 5 --- Install Nginx on Reverse Proxy Server

On your **Local Ubuntu machine**:

    sudo apt update
    sudo apt install nginx -y

------------------------------------------------------------------------

# Step 6 --- Configure Reverse Proxy

Create configuration file:

    sudo nano /etc/nginx/sites-available/reverse-proxy

Add configuration:

    server {

        listen 80;

        location /app1/ {
            proxy_pass http://192.168.1.11/;
        }

        location /app2/ {
            proxy_pass http://192.168.1.12/;
        }

    }

------------------------------------------------------------------------

# Step 7 --- Enable Configuration

Enable the site:

    sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/

Test configuration:

    sudo nginx -t

Restart nginx:

    sudo systemctl restart nginx

------------------------------------------------------------------------

# Step 8 --- Test Reverse Proxy

Open browser:

### Access Web App 1

    http://Proxy-IP/app1

Expected output:

    Welcome to Web Application 1

### Access Web App 2

    http://Proxy-IP/app2

Expected output:

    Welcome to Web Application 2

------------------------------------------------------------------------

# Final Result

Single entry point:

    Reverse Proxy → Routes traffic → Backend Servers

Benefits:

-   Improved security
-   Single access point for clients
-   Hide backend infrastructure
-   Easy scaling of applications
-   Centralized traffic management

------------------------------------------------------------------------

# Technologies Used

-   Ubuntu Linux
-   Nginx Web Server
-   Reverse Proxy Architecture
-   Virtual Machines

------------------------------------------------------------------------

# Author

Sharad Patel

DevOps / Linux / Cloud Enthusiast

GitHub: https://github.com/sharad1224

------------------------------------------------------------------------
