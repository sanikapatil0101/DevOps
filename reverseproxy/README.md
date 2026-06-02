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
