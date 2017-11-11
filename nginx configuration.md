As an administrator, if you have to make some changes to a file, never directly start editing the file and instead
save a copy of that file so that if something goes wrong, you can always come back to the original file and start editing
it again.

Here we will rename the files to edit by adding an 'ORIG' word to it (or any word you like) and then recreating the files with the original name and
pasting the original content to these new files.

First we need to set up the nginx configuration

```
cd /etc/ngnix
mv nginx.conf nginx.conf.ORIG
```
Recreate the nginx.conf file with vim followd by filename.Note that it doesn't create the file until you don't save it.
```
vim nginx.conf
```
Paste the following content into the nginx.conf file using Ctrl + Shift + v
```
user  www-data;
worker_processes  auto;

pid /run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    error_log  /var/log/nginx_error.log error;
    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    # SSL
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # no sslv3 (poodle etc.)
    ssl_prefer_server_ciphers on;

    # Gzip Settings
    gzip on;
    gzip_disable "msie6";
    gzip_vary on;
    gzip_min_length 512;
    gzip_types text/plain text/html application/x-javascript text/javascript application/javascript text/xml text/css application/font-sfnt;

    fastcgi_cache_path /usr/share/nginx/cache/fcgi levels=1:2 keys_zone=microcache:10m max_size=1024m inactive=1h;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```
The below lines basically compress the data sent by the server to the client (users) so that the page doesn't take longer time to load.
```
# Gzip Settings
    gzip on;
    gzip_disable "msie6";
    gzip_vary on;
    gzip_min_length 512;
    gzip_types text/plain text/html application/x-javascript text/javascript application/javascript text/xml text/css application/font-sfnt;
```
The below line enables caching.When a client requests a webpage, the webserver talks to the interpreter to process the files.
The interpreter in turn taks to the database to fetch the data.Now after the required processing, the page is given back to 
the server which inturns renders an HTML back to the client.But this process is lengthy and if a thousand users request the 
same page, it will put load on the server and page loading will  be very slow.Caching stores the content the very first time 
the process is completed such that if the next time a client sends a requeast, instead of repeating the entire process again, 
the stored content can directly be served to the client.
```
fastcgi_cache_path /usr/share/nginx/cache/fcgi levels=1:2 keys_zone=microcache:10m max_size=1024m inactive=1h;
```
Now create the cache directory as mentioned in the configuration.This essentialy tells nginx to store the cache data here.
```
mkdir -p /usr/share/nginx/cache/fcgi
```


