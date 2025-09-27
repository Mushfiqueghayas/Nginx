# 🔒 Section 5: SSL/TLS Setup in NGINX Using a Self-Signed Certificate (Ubuntu/Linux)

## 🎯 Goal
Secure your application with HTTPS using a self-signed SSL certificate.
This is ideal for local development, internal tools, and non-public test environments.

## 🧠 Why Use HTTPS (Even in Dev)?
  * Encrypts traffic between client and server
  * Simulates production-like environment for testing
  * Helps catch mixed-content issues early
  * Required by modern frontend frameworks and APIs
    
## 🛠️ Step-by-Step: Create a Self-Signed Certificate

## Step 1: Generate SSL Certificate and Key
  
    sudo openssl req -x509 -nodes -days 365 \
     -newkey rsa:2048 \
     -keyout /etc/ssl/private/nginx-selfsigned.key \
     -out /etc/ssl/certs/nginx-selfsigned.crt
   
When prompted:

  * Common Name (CN): use localhost or your server’s IP

## Step 2: Update NGINX Configuration

Edit the default site config:

    sudo nano /etc/nginx/sites-available/default
  
Replace with the following:

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
 
     ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
     ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
   
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
