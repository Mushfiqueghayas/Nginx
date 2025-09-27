# 🚀 Goal

Learn how to use NGINX to serve static content such as HTML, CSS, JavaScript, and images — a foundational skill for DevOps and Cloud Engineers.

#🧠 What is a Web Server?

A web server is software that serves static files (like .html, .css, .js, .png) over HTTP.
When users visit your website, the web server responds with these files.

NGINX is one of the fastest and most popular web servers used for this purpose.

# 📝 Anatomy of a Basic server Block
  server {
      listen 80;
      server_name localhost;

      root /var/www/html;
      index index.html;

      location / {
        try_files $uri $uri/ =404;
      }
  }

# Breakdown:
 * listen 80; → Listens on HTTP port 80
 * server_name localhost; → Domain or IP to respond to
 * root → Path where NGINX looks for files
 * index → Default file to serve (usually index.html)
 * location / → URL path handling

<img width="1110" height="851" alt="image" src="https://github.com/user-attachments/assets/c8c0132d-820b-42dc-8ea5-54b8c0e1d832" />

<img width="877" height="112" alt="image" src="https://github.com/user-attachments/assets/03c59710-0a22-495a-bbdb-05c6aa585863" />

<img width="1399" height="501" alt="image" src="https://github.com/user-attachments/assets/c49f6d1a-bc93-44be-8f03-8053e369bc78" />

# 🧪 Demo: Serve a Static Website Using NGINX

## 🔧 Option 1: Using Native Linux NGINX

1. Create an HTML file:
   
        echo "<h1>Hello from NGINX Web Server</h1>" | sudo tee /var/www/html/index.html

3. Reload NGINX:
   
        sudo systemctl reload nginx

5. Test: Visit: http://localhost or your server’s IP in browser.

## 🐳 Option 2: Serve HTML from Docker

1. Create a project folder:
        mkdir nginx-static && cd nginx-static

3. Add index.html:
        <!-- index.html -->
        <h1>Hello from NGINX in Docker!</h1>
  
4. Run NGINX Docker container:
        docker run --name web-nginx -v $PWD:/usr/share/nginx/html:ro -p 8080:80 -d nginx

5.Open in browser:
      http://localhost:8080
  
# 🔄 Root vs Alias
These two directives behave differently inside location blocks.

## `root` example:
    location /static/ {
        root /data/www;
    }
    # /static/img.png → /data/www/static/img.png

## `alias` example:
    location /static/ {
        alias /data/www/;
    }
    # /static/img.png → /data/www/img.png
📌 Use alias when you want to replace the URI path.

# 🧯 Common Errors & Fixes
|     Error	         |  Solution                                          |
|--------------------|----------------------------------------------------|
|403 Forbidden       |	Check file permissions (use chmod/chown)          |
|404 Not Found       |	Ensure correct root or alias                      |
|NGINX not reloading | changes	Use sudo nginx -s reload or restart NGINX |
|Port already in use |	Use sudo lsof -i :80 to identify process          |

# ✅ Summary
NGINX can serve static files efficiently.
The root and index directives define where and what to serve.
Use Docker volumes to serve files without touching the host filesystem.
Always reload NGINX after making config changes.
