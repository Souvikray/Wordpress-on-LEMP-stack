First create a system user for this site
```
adduser websiteuser
```
Create a directory that will store the logs for this particular user
```
mkdir -p /home/yourusername/logs
```
Now we want to host multiple wordpress websites (or domains) from a single server since we are building a Wordpress Hosting Platform.
So we create nginx vhost config file and add the follwing content to it
```
nano /etc/nginx/conf.d/yoursitename.conf
```
```
server {
    listen       80;
    server_name  www.jukeboxhero.xyz;

    client_max_body_size 20m;

    index index.php index.html index.htm;
    root   /home/websiteuser/public_html;

    location / {
        try_files $uri $uri/ /index.php?q=$uri&$args;
    }

    # pass the PHP scripts to FastCGI server
    location ~ \.php$ {
            # Basic
            try_files $uri =404;
            fastcgi_index index.php;

            # Create a no cache flag
            set $no_cache "";

            # Don't ever cache POSTs
            if ($request_method = POST) {
              set $no_cache 1;
            }

            # Admin stuff should not be cached
            if ($request_uri ~* "/(wp-admin/|wp-login.php)") {
              set $no_cache 1;
            }

            # WooCommerce stuff should not be cached
            if ($request_uri ~* "/store.*|/cart.*|/my-account.*|/checkout.*|/addons.*") {
              set $no_cache 1;
            }

            # If we are the admin, make sure nothing
            # gets cached, so no weird stuff will happen
            if ($http_cookie ~* "wordpress_logged_in_") {
              set $no_cache 1;
            }

            # Cache and cache bypass handling
            fastcgi_no_cache $no_cache;
            fastcgi_cache_bypass $no_cache;
            fastcgi_cache microcache;
            fastcgi_cache_key $scheme$request_method$server_name$request_uri$args;
            fastcgi_cache_valid 200 60m;
            fastcgi_cache_valid 404 10m;
            fastcgi_cache_use_stale updating;


            # General FastCGI handling
            fastcgi_pass unix:/var/run/php/yoursitename.sock;
            fastcgi_pass_header Set-Cookie;
            fastcgi_pass_header Cookie;
            fastcgi_ignore_headers Cache-Control Expires Set-Cookie;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_param SCRIPT_FILENAME $request_filename;
            fastcgi_intercept_errors on;
            include fastcgi_params;         
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico|woff|ttf|svg|otf)$ {
            expires 30d;
            add_header Pragma public;
            add_header Cache-Control "public";
            access_log off;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}

server {
    listen       80;
    server_name  jukeboxhero.xyz;
    rewrite ^/(.*)$ http://www.jukeboxhero.xyz/$1 permanent;
}
```
Now disable default nginx vhost only for the first time you set up a website
```
rm /etc/nginx/sites-enabled/default
```
Now we create php-fpm vhost pool config file and add the following content
```
vim /etc/php/7.0/fpm/pool.d/yoursitename.conf
```
```
[yoursitename]
listen = /var/run/php/jukeboxhero.sock
listen.owner = websiteuser
listen.group = www-data
listen.mode = 0660
user = websiteuser
group = www-data
pm = dynamic
pm.max_children = 75
pm.start_servers = 8
pm.min_spare_servers = 5
pm.max_spare_servers = 20
pm.max_requests = 500

php_admin_value[upload_max_filesize] = 25M
php_admin_value[error_log] = /home/websiteuser/logs/phpfpm_error.log
php_admin_value[open_basedir] = /home/websiteuser:/tmp
```
Now we create a php-fpm log file to store the logs
```
touch /home/websiteuser/logs/phpfpm_error.log
```
Now we need to create a database and a database user.So we log in to MySQL as root by using the password we create earlier
```
mysql -u root -p
```
This will give you a shell where you can enter the following commands.Use a strong password to enhance security
```
CREATE DATABASE jukeboxhero;
CREATE USER jukeboxhero1@localhost IDENTIFIED BY '@****************';
GRANT ALL PRIVILEGES ON jukeboxhero.* TO jukeboxhero1@localhost IDENTIFIED BY '@****************';
FLUSH PRIVILEGES;
```
Now you can exit the MySQL shell by pressing Ctrl + D

We download and install the WordPress application.So under the home directory in my case 'websiteuser', type the follwing command
```
wget https://wordpress.org/latest.tar.gz
```
Now extract the wordpress archive
```
tar zxf latest.tar.gz
```
Remove the archive
```
rm latest.tar.gz
```
Rename the extracted 'wordpress' directory to 'public_html'.It is not mandatory but we follow this as a standard.
```
mv wordpress public_html
```
Set proper file permissions on your site files
```
cd /home/websiteuser/public_html
chown -R websiteuser:www-data .
find . -type d -exec chmod 755 {} \;
find . -type f -exec chmod 644 {} \;
```
Reflect the changes
```
systemctl restart php7.0-fpm nginx mysql
```
Now in your browser, type in your server's IP address.You will get a Wordpress Installer setup.It should look something like below.Just follow the steps and you will have a Wordpress website up and running.
![Alt text](https://github.com/Souvikray/Wordpress-on-LEMP-stack/blob/master/screenshot4.png?raw=true "Optional Title")
![Alt text](https://github.com/Souvikray/Wordpress-on-LEMP-stack/blob/master/screenshot5.png?raw=true "Optional Title")

The final step is to secure the wp-config.php file so that other users can’t read DB credentials
```
chmod 640 /home/websiteuser/public_html/wp-config.php
```
