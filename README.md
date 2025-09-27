# Nginx installation on Ubuntu/Debian

    sudo apt update
    sudo apt install nginx -y

<img width="1903" height="755" alt="image" src="https://github.com/user-attachments/assets/11425f07-df0e-4b68-8c7e-f76f2ffdd275" />

<img width="1920" height="513" alt="image" src="https://github.com/user-attachments/assets/f7d76860-a31b-48ff-b250-8f37d8d5cebb" />



# Nginx installation on RHEL/CentOS

    sudo yum install epel-release -y
    sudo yum install nginx -y

# Using Docker (Recommended for DevOps)

    docker run --name nginx -p 8080:80 -d nginx
    docker stop nginx-demo
    docker rm nginx-demo

Visit: http://localhost:8080

# 📁 NGINX File Structure (Linux)
|      File/Directory        |    Purpose                                  |
|----------------------------|---------------------------------------------|
|/etc/nginx/nginx.conf       |	Main configuration file                    |
|/etc/nginx/sites-available/ |	Stores virtual host (server block) configs |
|/etc/nginx/sites-enabled/   |	Symlinks to active site configs            |
|/var/www/html               |	Default web root directory                 |
|/var/log/nginx/             |	Contains access and error logs             |
