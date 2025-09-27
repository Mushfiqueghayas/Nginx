# 🔁 Section 3: NGINX as a Reverse Proxy (Ubuntu/Linux)

## 🧠 What is a Reverse Proxy?
A reverse proxy is a server that receives client requests and forwards them to backend servers, then sends the response back to the client.

NGINX is one of the most popular tools used as a reverse proxy in production.

## 🔄 Reverse Proxy vs Forward Proxy

|Feature              |	Forward Proxy                 |	Reverse Proxy                                |
|---------------------|-------------------------------|----------------------------------------------|
|Who configures it    |	Client                        |	Server-side                                  |
|Forwards requests to |	External servers (internet)   |	Internal backend servers (apps/services)     |
|Use case             |	Browsing anonymously, caching |	Load balancing, SSL termination, API gateway |
|Example              |	Proxy server for office users |	NGINX between frontend and backend apps      |

## 📌 Why Use NGINX as a Reverse Proxy?
Protect backend services from direct access
Centralized SSL termination
Load balancing backend apps
Path-based routing (/api → backend1, /app → backend2)
Easy caching and compression

## 📝 Reverse Proxy Configuration (Ubuntu/Linux)
### 🔧 File: /etc/nginx/sites-available/default
Update the existing server block or create a new one:

    server {
        listen 80;
        server_name localhost;
        
        location /service1/ {
            proxy_pass http://localhost:3000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }

        location /service2/ {
            proxy_pass http://localhost:3001;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }

### Breakdown:
proxy_pass → forwards requests to your backend app
proxy_set_header → preserves original request metadata (like IP and host)

## 🧪 Demo: Reverse Proxy to a Node.js App

### Step 1: Install Node.js (optional if using your own backend)

      sudo apt update
      sudo apt install nodejs npm -y

### Step 2: Create a simple backend app

      mkdir ~/node-backend && cd ~/node-backend
      nano server1.js
      nano server2.js
      
#### Paste this:

        const http = require('http');
        http.createServer((req, res) => {
          res.end('✅Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-1 !!!!');
        }).listen(3000);

        ----------------------------------------------------------------------------------------

        const http = require('http');
        http.createServer((req, res) => {
          res.end('✅Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-2 !!!!');
        }).listen(3001);

#### Run it:

        node server1.js &
        node server2.js &
        
#### Your app is now running at `http://localhost:3000` and `http://localhost:3001`

### Step 3: Test and reload NGINX

Check config for syntax errors:

        sudo nginx -t
        
Reload NGINX:

        sudo systemctl reload nginx
        
Step 5: Test in browser

Visit:

        http://localhost:3000
        http://localhost:3001
        
✅ You should see: ✅Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-1 !!!!

✅ You should see: ✅Hello This is MUSHFIQUE GHAYAS from Node.js backend SERVICE-2 !!!!

## 📁 File Structure Recap (Ubuntu)

|             Path                  |	       Purpose                     |
|-----------------------------------|--------------------------------------|
|/etc/nginx/nginx.conf              |Global NGINX settings                 |
|/etc/nginx/sites-available/default |Active site config for reverse proxy  |
|/var/www/html                      |Not used in reverse proxy             |
|/var/log/nginx/access.log          |Logs all requests                     |

	
## ✅ Summary
NGINX can proxy traffic to backend apps using proxy_pass.
Config changes go in /etc/nginx/sites-available/default (on Ubuntu).
Always test config and reload NGINX after changes.
Ideal for API gateways, internal routing, and SSL termination.	
	
	
