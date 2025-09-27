# ⚖️ Section 4: Load Balancing with NGINX (Ubuntu/Linux)

## 🎯 Goal
Use NGINX to distribute traffic across multiple backend servers — improving availability, reliability, and scalability of your applications.

## 🧠 What is Load Balancing?
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
### Step 1: Create Backend Servers
We'll run two simple HTTP servers using Node.js.

#### server1.js

    const http = require('http');
      http.createServer((req, res) => {
        res.end('✅Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-1 SERVER-1 !!!!');
      }).listen(3000);

#### server2.js

    const http = require('http');
          http.createServer((req, res) => {
                  res.end('✅Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-1 SERVER-2 !!!!');
          }).listen(3002);

#### server3.js

    const http = require('http');
      http.createServer((req, res) => {
        res.end('✅Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-2 SERVER-1 !!!!');
      }).listen(3001);

#### server4.js

    const http = require('http');
      http.createServer((req, res) => {
        res.end('✅ Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-2 SERVER-2 !!!!');
      }).listen(3003);

### Step 2: Run both servers

    node server1.js &
    node server2.js &
    node server3.js &
    node server4.js &
    
### 3 Step 3: Reload NGINX

    sudo nginx -t
    sudo systemctl reload nginx
    
### Step 4: Test the Load Balancer

Open a browser or use curl:

    curl http://localhost/api/service1/
    curl http://localhost/api/service2/

Run it multiple times — you should see the response alternate between:

    âœ…Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-1 SERVER-1 !!!!
    âœ…Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-1 SERVER-2 !!!!
    
    âœ…Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-2 SERVER-1 !!!!
    âœ…Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-2 SERVER-2 !!!!

✅ You’ve just created a working load balancer using NGINX!

<img width="1609" height="563" alt="image" src="https://github.com/user-attachments/assets/f03ca223-cfc8-4050-a349-d6170a7ddac9" />

<img width="1715" height="356" alt="image" src="https://github.com/user-attachments/assets/7badccf1-1211-4bd5-8e6d-a8dd5a6beb8c" />

<img width="1605" height="369" alt="image" src="https://github.com/user-attachments/assets/722a4d35-797d-4790-a95f-9217a9e900fb" />

<img width="1608" height="300" alt="image" src="https://github.com/user-attachments/assets/73a1c618-f2af-4e39-9122-19bba6d6394c" />

## 🔄 Switching Load Balancing Methods

### Use Least Connections

    upstream backend_app {
        least_conn;
        server 127.0.0.1:3001;
        server 127.0.0.1:3002;
    }

### Use IP Hash

    upstream backend_app {
        ip_hash;
        server 127.0.0.1:3001;
        server 127.0.0.1:3002;
    }
### Weighted servers:

    upstream service1_backend {
        server localhost:8081 weight=3;
        server localhost:8083 weight=1;
    }

