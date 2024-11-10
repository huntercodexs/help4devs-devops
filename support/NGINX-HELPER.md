
# NGINX HELPER

> The version used in this document for Nginx is xxx

### Ubuntu 20.04

###### Installing

- System Update

<pre>
sudo apt update
sudo apt-get update
</pre>

- Install Nginx

<pre>
sudo apt install nginx
</pre>

- Allow Nginx on firewall

<pre>
sudo ufw allow 'Nginx Full'
sudo ufw status
</pre>

- Manage the NGINX service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable nginx
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl status nginx
</pre>

- Check the NGINX page

<pre>
http://192.168.0.26:80
</pre>

- Edit NGINX configurations

<pre>
/etc/nginx/nginx.conf
/etc/nginx/sites-enable
/etc/nginx/sites-available
</pre>

- Sample site configuration

<pre>
server {
    listen 80;
    listen [::]:80;
    
    root /var/www/huntercodexs/html;
    index index.html index.htm index.nginx-debian.html;
    
    server_name huntercodexs www.huntercodexs;
    
    location / {
            try_files $uri $uri/ =404;
    }
}
</pre>

###### Uninstall

<pre>
sudo ufw delete allow 80/tcp
sudo ufw delete allow 443/tcp
sudo ufw reload

sudo systemctl stop nginx.service
sudo apt remove nginx
sudo apt-get remove nginx
sudo apt-get remove --auto-remove -y
cd /
sudo find . | grep nginx | xargs -i sudo rm -rf {}
</pre>

> TIP: Maybe it will be necessary to restart the machine

### Almalinux 8.10

###### Installing

- System Update

<pre>
sudo yum update
</pre>

- Install NGINX

<pre>
sudo yum install nginx
</pre>

- Allow Nginx on firewall

<pre>
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
sudo firewall-cmd --reload
</pre>

- Manage the NGINX service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable nginx
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl status nginx
</pre>

- Check the NGINX page

<pre>
http://192.168.0.241:80
</pre>

- Edit NGINX configurations

<pre>
/etc/nginx/nginx.conf
/etc/nginx/sites-enable
/etc/nginx/sites-available
</pre>

- Sample site configuration

<pre>
server {
    listen 80;
    listen [::]:80;
    
    root /var/www/huntercodexs/html;
    index index.html index.htm index.nginx-debian.html;
    
    server_name huntercodexs www.huntercodexs;
    
    location / {
            try_files $uri $uri/ =404;
    }
}
</pre>

###### Uninstall

<pre>
sudo firewall-cmd --zone=public --remove-port=80/tcp
sudo firewall-cmd --zone=public --remove-port=443/tcp
sudo firewall-cmd --runtime-to-permanent 
sudo firewall-cmd --reload

sudo systemctl stop nginx.service
sudo yum remove nginx
sudo yum autoremove -y
cd /
sudo find . | grep nginx | xargs -i sudo rm -rf {}
</pre>

> TIP: Maybe it will be necessary to restart the machine

### EXTRAS

###### Rewrite URL Rules

To create a rewrite rules in the nginx configuration follow the tips below

- Set up the nginx.conf file as something like this

<pre>
sudo vi /etc/nginx/nginx.conf
</pre>

<pre>
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

}
</pre>

- Create a configuration file to application, for example

<pre>
sudo vi /etc/nginx/conf.d/application-name.conf
</pre>

<pre>
 server {
    listen       33900;
    server_name  application-name.local;
    index index.html;
    client_max_body_size 20M;

    error_log  /var/log/nginx/error-application-name.local.log;
    access_log /var/log/nginx/access-application-name.local.log;

    root /var/www;

    location /application-name-uri {
        index index.html;
        try_files $uri $uri/ /application-name-uri/index.html;
    }
}
</pre>

This configuration can be used in cases where we have a Vue or Angular application that makes automatic 
management from the request into the application, and makes the redirect for the inside resources.


