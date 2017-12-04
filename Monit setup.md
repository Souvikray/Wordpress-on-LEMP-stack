Now we will set up the monitoring service 'Monit' to monitor the services and report any problem.

Edit the configuration file 'monitrc'
```
vim /etc/monit/monitrc
```
```
set daemon  30
set logfile /var/log/monit.log
set alert website@jukeboxhero.xyz

set httpd
    port 2812
    use address localhost  # only accept connection from localhost
    # allow localhost        # allow localhost to connect to the server and
    allow username:passwd
# include files for individual sites
include /etc/monit/conf.d/lemp_services
```
If you want your monitrc file to look cleaner, you can add the nginx, mysql and php-fpm configurations in a seperate file and include the path of that file in the montrc.

Create a new file with nano /etc/monit/conf.d/lemp
You can name it lemp or whatever you wish to name it
```
#check nginx
check process nginx with pidfile /var/run/nginx.pid
  start program = "/etc/init.d/nginx start"
  stop program = "/etc/init.d/nginx stop"
  group www-data

# Check MySQL
check host localmysql with address 127.0.0.1
      if failed ping then alert
      if failed port 3306 protocol mysql then alert

# Check php-fpm
check process phpfpm with pidfile /var/run/php/php7.0-fpm.pid
      if cpu > 50% for 2 cycles then alert     # we have defined 1 cycle as 30s
      # if total cpu > 60% for 5 cycles then restart
      if memory > 800 MB then alert
      # if total memory > 500 MB then restart
```
Now, let's add some site files at /etc/monit/conf-available/
For your first website, add a file named site-jukeboxhero
```
vim /etc/monit/conf-available/site-jukeboxhero
```
Add the following content
```
    # Site monitoring fragment for jukeboxhero.xyz
    check host jukeboxhero.xyz with address jukeboxhero.xyz
        if failed port 80 protocol http for 2 cycles then alert

    check file access.log with path /var/log/nginx_error.log
        if size > 15 MB then exec "/usr/local/sbin/logrotate -f /var/log/nginx_error.log"
```
Restart monit and it's ready!
```
systemctl restart monit
```
Now the monit application is running and you can view it.

To view the monitoring status, exit the server and type the command below.The command specifically tells ssh to forward the port 2812 used by monit on the server to our local machine at 1234.We can choose any port number for our local machine to view the status.
```
ssh -L 1234:localhost:2812
```
Now go to the browser and type in the following
```
localhost:1234
```
or
```
127.0.0.1:1234
```
You should be able to see something like this
![Alt text](https://github.com/Souvikray/Wordpress-on-LEMP-stack/blob/master/screenshot6.png?raw=true "Optional Title")
