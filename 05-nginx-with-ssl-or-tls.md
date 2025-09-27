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

### Step 1: Generate SSL Certificate and Key
  
    sudo openssl req -x509 -nodes -days 365 \
     -newkey rsa:2048 \
     -keyout /etc/ssl/private/nginx-selfsigned.key \
     -out /etc/ssl/certs/nginx-selfsigned.crt
   
When prompted:

  * Common Name (CN): use localhost or your server’s IP

### Step 2: Update NGINX Configuration

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

### Step 3: Reload NGINX

Check and reload configuration:

    sudo nginx -t
    sudo systemctl reload nginx
    
Step 4: Test HTTPS Locally

Open your browser and visit:

    https://localhost
    https://localhost/api/service1/
    https://localhost/api/service2/

 <img width="1920" height="637" alt="image" src="https://github.com/user-attachments/assets/52cb6195-a662-4b16-b3aa-579e0d0b3b30" />

<img width="1620" height="468" alt="image" src="https://github.com/user-attachments/assets/517b045a-ee70-41c8-b38f-8fcee137662c" />

<img width="1532" height="278" alt="image" src="https://github.com/user-attachments/assets/9026c3c2-8646-46bc-aca5-9f4a5196c387" />

<img width="1589" height="250" alt="image" src="https://github.com/user-attachments/assets/8f0efcbb-9c42-4f04-8411-e2b78da500b5" />

<img width="1624" height="341" alt="image" src="https://github.com/user-attachments/assets/fa2718a5-c37d-43b8-acb2-87b467162d3f" />

    
⚠️ You will see a warning:

"Your connection is not private"

✅ That’s expected with self-signed certs. Proceed anyway to view your site securely.

## 📁 SSL File Paths Recap

|          Path                        |	Purpose            |
|--------------------------------------|--------------------|
|/etc/ssl/certs/nginx-selfsigned.crt   |	SSL certificate    |
|/etc/ssl/private/nginx-selfsigned.key |	Private key        |
|/etc/nginx/sites-available/default    |	HTTPS proxy config |

#🧪 Bonus: Test Without Browser (curl)
    
    curl -k https://localhost

`-k` allows insecure (self-signed) HTTPS connections.

## ✅ Summary
 * Self-signed SSL is perfect for secure local development.
 * Requires only OpenSSL and a few lines in NGINX.
 * Always test your HTTPS setup with curl and browser.
 * In production, switch to Let’s Encrypt or trusted CAs.
