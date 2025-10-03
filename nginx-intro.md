![Nginx Logo](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQtry6J_Jp0xzhsr7By5pFzb0DWYDDyeyeITtGA7JPQQnf2a2XWN8Q7biqFdt8a2EEKiN8&usqp=CAU)

# Nginx Test Markdown

This document serves as a comprehensive test of Markdown formatting while providing useful information about **Nginx**, a high-performance web server and reverse proxy.

---

## Table of Contents
1. [Introduction](#introduction)
2. [Key Features](#key-features)
3. [Basic Configuration](#basic-configuration)
4. [Common Use Cases](#common-use-cases)
5. [Performance Tips](#performance-tips)
6. [Troubleshooting](#troubleshooting)
7. [References](#references)

---

## Introduction

**Nginx** (pronounced "engine-x") is an open-source web server that can also be used as a reverse proxy, load balancer, mail proxy, and HTTP cache. Created by Igor Sysoev in 2004, it's known for its high performance, stability, rich feature set, simple configuration, and low resource consumption.

> **Fun Fact**: As of 2023, Nginx serves over **33%** of all active websites globally!

---

## Key Features

- ✅ **High Performance**: Handles thousands of concurrent connections with low memory usage
- ✅ **Reverse Proxy**: Efficiently forwards requests to backend servers
- ✅ **Load Balancing**: Distributes traffic across multiple servers
- ✅ **SSL/TLS Termination**: Handles encryption/decryption efficiently
- ✅ **Static File Serving**: Optimized for serving static content
- ✅ **HTTP/2 and HTTP/3 Support**: Modern protocol support
- ✅ **Modular Architecture**: Extensible through modules

---

## Basic Configuration

### Simple Server Block

```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    
    root /var/www/html;
    index index.html index.htm;
    
    location / {
        try_files $uri $uri/ =404;
    }
    
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    }
}
```

### Reverse Proxy Example

```nginx
server {
    listen 80;
    server_name api.example.com;
    
    location / {
        proxy_pass http://backend_servers;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

upstream backend_servers {
    server 192.168.1.10:3000;
    server 192.168.1.11:3000;
    server 192.168.1.12:3000;
}
```

---

## Common Use Cases

| Use Case | Description | Configuration Complexity |
|----------|-------------|--------------------------|
| **Static Web Server** | Serving HTML, CSS, JS, images | ⭐ |
| **Reverse Proxy** | Forwarding requests to application servers | ⭐⭐ |
| **Load Balancer** | Distributing traffic across multiple servers | ⭐⭐⭐ |
| **SSL/TLS Termination** | Handling HTTPS encryption | ⭐⭐ |
| **Caching Layer** | Caching responses to reduce backend load | ⭐⭐⭐ |
| **API Gateway** | Routing and managing API requests | ⭐⭐⭐⭐ |

---

## Performance Tips

### 🔧 Essential Optimizations

1. **Worker Processes**: Set to match CPU cores
   ```nginx
   worker_processes auto;
   ```

2. **Worker Connections**: Increase connection limit
   ```nginx
   events {
       worker_connections 1024;
   }
   ```

3. **Enable Gzip Compression**
   ```nginx
   gzip on;
   gzip_vary on;
   gzip_min_length 1024;
   gzip_types text/plain text/css application/json application/javascript;
   ```

4. **Buffer Tuning**
   ```nginx
   client_body_buffer_size 128k;
   client_max_body_size 10m;
   client_header_buffer_size 1k;
   large_client_header_buffers 4 4k;
   ```

### 📊 Monitoring Commands

```bash
# Check configuration syntax
nginx -t

# Reload configuration without downtime
nginx -s reload

# View active connections
netstat -an | grep :80 | wc -l

# Check Nginx status (if stub_status enabled)
curl http://localhost/nginx_status
```

---

## Troubleshooting

### Common Issues & Solutions

| Issue | Possible Cause | Solution |
|-------|----------------|----------|
| **502 Bad Gateway** | Backend server down/unreachable | Check backend health, verify `proxy_pass` |
| **403 Forbidden** | Permission issues | Check file/directory permissions |
| **404 Not Found** | Incorrect root path | Verify `root` directive and file existence |
| **High CPU Usage** | Misconfigured workers | Adjust `worker_processes` and `worker_connections` |
| **SSL Handshake Failed** | Certificate issues | Verify certificate chain and expiration |

### 🔍 Debugging Steps

1. **Check Error Logs**
   ```bash
   tail -f /var/log/nginx/error.log
   ```

2. **Test Configuration**
   ```bash
   nginx -T  # Test and dump full config
   ```

3. **Verify Process Status**
   ```bash
   systemctl status nginx
   ```

---

## References

- 📚 [Official Nginx Documentation](https://nginx.org/en/docs/)
- 🛠️ [Nginx Configuration Generator](https://www.digitalocean.com/community/tools/nginx)
- 📊 [Nginx vs Apache Comparison](https://www.nginx.com/nginx-vs-apache/)
- 🐳 [Docker Nginx Image](https://hub.docker.com/_/nginx)

---

> **Note**: Always test configuration changes in a staging environment before deploying to production!

