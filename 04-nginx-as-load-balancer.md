# ⚖️ Section 4: Load Balancing with NGINX (Ubuntu/Linux)

## 🎯 Goal
Use NGINX to distribute traffic across multiple backend servers — improving availability, reliability, and scalability of your applications.

# 🧠 What is Load Balancing?
Load balancing is the process of distributing incoming network traffic across multiple backend servers.

Benefits:

 * Prevents server overload
 * Increases availability and fault tolerance
 * Enables horizontal scaling
 * NGINX supports multiple load balancing algorithms out of the box.

## 🧮 Load Balancing Algorithms in NGINX

| Algorithm  |	Behavior                                                           |
|------------|---------------------------------------------------------------------|
|round-robin |	Default — rotates through all backends equally                     |
|least_conn  |	Sends traffic to the backend with the fewest active connections    |
|ip_hash     |	Uses client IP to consistently route requests to the same backendv |

## 📝 Basic Load Balancer Configuration

Edits

    sudo nano /etc/nginx/sites-available/default

Replace contents with:

    upstream service1_backend {
        server localhost:3000;
        server localhost:3002;
        # Optional load balancing method: round-robin (default), least_conn, ip_hash
        # least_conn;
    }
    
    upstream service2_backend {
        server localhost:3001;
        server localhost:3003;
        # ip_hash;   # Uncomment to bind a client to same backend
    }
  
  server {
      listen 80;
      server_name localhost;
  
      root /var/www/html;
      index index.html;
  
      location / {
          try_files $uri $uri/ =404;
      }
  
      location /api/service1/ {
          proxy_pass http://service1_backend/;
              #proxy_pass http://localhost:3000;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
      }
  
      location /api/service2/ {
           proxy_pass http://service2_backend/;
              #proxy_pass http://localhost:3001;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
      }
  }

## 🧪 Demo: Load Balance Two Local Backend Servers
Step 1: Create Backend Servers
We'll run two simple HTTP servers using Node.js.

### server1.js

    const http = require('http');
      http.createServer((req, res) => {
        res.end('✅Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-1 SERVER-1 !!!!');
      }).listen(3000);

### server2.js

    const http = require('http');
          http.createServer((req, res) => {
                  res.end('✅Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-1 SERVER-2 !!!!');
          }).listen(3002);

### server3.js

    const http = require('http');
      http.createServer((req, res) => {
        res.end('✅Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-2 SERVER-1 !!!!');
      }).listen(3001);

### server4.js

    const http = require('http');
      http.createServer((req, res) => {
        res.end('✅ Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-2 SERVER-2 !!!!');
      }).listen(3003);
