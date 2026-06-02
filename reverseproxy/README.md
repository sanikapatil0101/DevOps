# AWS + EC2 + Nginx Notes

## Step 1 - AWS Basics

### What is AWS?

AWS (**Amazon Web Services**) is Amazon's cloud platform.

It lets you:

* Rent servers
* Manage domains
* Store/upload files (mp4, jpg, mp3, etc.)
* Autoscale servers
* Create Kubernetes clusters
* Use databases, storage, networking services

Focus: **Renting Servers**

---

## Step 2 - EC2 Servers

### What is EC2?

EC2 = **Elastic Compute Cloud**

AWS virtual machines are called **EC2 instances**.

Meaning:

* **Elastic** → Increase/decrease machine resources
* **Compute** → Virtual machine/server

You can create EC2 instances from the AWS Dashboard.

---

## Step 3 - Creating an EC2 Instance

Steps:

1. Click **Launch Instance**
2. Give instance name
3. Choose Operating System (Ubuntu/Linux)
4. Select machine size
5. Create key pair (`.pem` file)
6. Configure storage
7. Allow **HTTP/HTTPS traffic**
8. Launch instance

---

## Step 4 - SSH into EC2 Server

### Give SSH Key Permission

```bash
chmod 700 kirat-class.pem
```

### Connect to Server

```bash
ssh -i kirat-class.pem ubuntu@your-ec2-domain
```

### Clone Repository

```bash
git clone https://github.com/hkirat/sum-server
cd sum-server
```

---

## Step 5 - Install Backend Dependencies

### Install Node.js

Follow Node installation guide for Ubuntu.

### Install Dependencies

```bash
npm install
```

### Start Backend

```bash
node index.js
```

---

## Step 6 - Accessing the Server

Your EC2 server provides:

* Public IP
* Public DNS

Backend runs on:

```text
http://your-domain:8080
```

or

```text
http://your-domain:3000
```

### If website is not accessible:

Check:

* Open required port in **Security Groups**
* Allow inbound traffic for:

  * HTTP (80)
  * HTTPS (443)
  * Custom backend ports (3000/8080)

---

## Step 7 - Nginx Reverse Proxy

### Install Nginx

```bash
sudo apt update
sudo apt install nginx
```

Nginx starts on **Port 80**.

---

### What is Nginx?

Nginx is used as:

* Web Server
* Reverse Proxy
* Load Balancer
* SSL Handler

### Reverse Proxy Concept

```text
Client → Nginx → Backend Server
```

Nginx receives requests and forwards them to backend.

---

### Configure Nginx

Edit config file:

```bash
sudo vi /etc/nginx/nginx.conf
```

Example config:

```nginx
events {
}

http {
    server {
        listen 80;
        server_name be1.100xdevs.com;

        location / {
            proxy_pass http://localhost:8080;
            proxy_http_version 1.1;

            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
}
```

---

### Reload Nginx

```bash
sudo nginx -s reload
```

### Start Backend

```bash
node index.js
```

### Request Flow

```text
Client → Nginx (Port 80) → Backend (Port 8080)
```

---

## Step 8 - SSL Certificate Management

Use **Certbot** for HTTPS certificates.

Official Site:

```text
https://certbot.eff.org/
```

Purpose:

* Generate SSL certificates
* Enable HTTPS
* Auto renew certificates

---

## Quick Revision

### AWS

Cloud platform for servers, storage, networking, etc.

### EC2

AWS virtual machine service.

### SSH

Remote login into server using `.pem` key.

### Security Group

Firewall rules controlling allowed traffic.

### Nginx

Web server + Reverse Proxy.

### Reverse Proxy Flow

```text
Client → Nginx → Backend
```

### Certbot

Tool for SSL/HTTPS certificate management.

# Forward Proxy vs Reverse Proxy

## Forward Proxy

**Client-side proxy**

Flow:

```text
Client → Forward Proxy → Internet
```

* Represents **client**
* Hides client IP
* Used for filtering, security, caching

**Example:** Company / College Proxy

---

## Reverse Proxy

**Server-side proxy**

Flow:

```text
Client → Reverse Proxy → Backend Server
```

* Represents **server**
* Hides backend servers
* Used for load balancing, SSL, security, caching

**Example:** Nginx before Node.js / Java servers

---

## Key Difference

| Forward Proxy    | Reverse Proxy    |
| ---------------- | ---------------- |
| Protects client  | Protects server  |
| Client side      | Server side      |
| Outgoing traffic | Incoming traffic |

---

## Nginx

Nginx is used as:

* Web Server
* Reverse Proxy
* Load Balancer
* SSL/TLS Handler

### Basic Reverse Proxy Config

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://localhost:5000;
    }
}
```

Flow:

```text
Client → Nginx → Backend App
```

---

## Install Nginx (Ubuntu)

```bash
sudo apt update
sudo apt install nginx
```

---

## Edit Nginx Config

```bash
sudo vi /etc/nginx/nginx.conf
```

Example config:

```nginx
events {
    # Event directives ???not needed
}

http {
    server {
        listen 80;
        server_name be1.100xdevs.com;

        location / {
            proxy_pass http://localhost:8080;
            proxy_http_version 1.1;

            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
}
```

---

## Reload Nginx

```bash
sudo nginx -s reload
```
