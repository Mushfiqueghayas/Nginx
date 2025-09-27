# Nginx installation on Ubuntu/Debian

    sudo apt update
    sudo apt install nginx -y

<img width="1903" height="755" alt="image" src="https://github.com/user-attachments/assets/11425f07-df0e-4b68-8c7e-f76f2ffdd275" />

<img width="1920" height="513" alt="image" src="https://github.com/user-attachments/assets/f7d76860-a31b-48ff-b250-8f37d8d5cebb" />

<img width="1426" height="107" alt="image" src="https://github.com/user-attachments/assets/137d408f-9a94-4000-b4a8-e844df1962fa" />

<img width="1322" height="105" alt="image" src="https://github.com/user-attachments/assets/dbaf38a0-f7cc-4605-b97b-a3ce1a49e24f" />

<img width="1220" height="108" alt="image" src="https://github.com/user-attachments/assets/b30fc550-12a3-4e45-afe3-fcea6967698f" />

<img width="1641" height="744" alt="image" src="https://github.com/user-attachments/assets/54afb8ab-708b-4a51-b06d-ca52c263783f" />

<img width="1909" height="468" alt="image" src="https://github.com/user-attachments/assets/bdb35386-5184-4c89-94a0-554ef8b5f7e4" />

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
