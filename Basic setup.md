Log into your VPS and perform the follwing commands
```
apt-get update
apt-get upgrade
```
The above two commands check for any relevant updates for repositories and then updates those repositories if needed.

Now install the web server (nginx), interpreter (php-fpm), database (MySQL) and monitoring tool (monit)
```
apt-get install mysql-server
apt-get install nginx php7.0-fpm php-mysql monit
```
Now check whether the softwares installed is working properly.We use systemctl to start,reload or stop a service.
```
systemctl start nginx php7.0-fpm mysql monit
```
Now to check if the services has started successfully, type the following commands below
```
systemctl status nginx
```
![Alt text](https://github.com/Souvikray/Wordpress-on-LEMP-stack/blob/master/screenshot2.png?raw=true "Optional Title")

```
systemctl status php7.0-fpm
```
![Alt text](https://github.com/Souvikray/Wordpress-on-LEMP-stack/blob/master/screenshot3.png?raw=true "Optional Title")

Similarly you can see the status of rest of the services by typing systemctl followed by the service name.Ensure that the status is displayed as 'active'.

Note - To quit displaying the status, press 'q'

To enable the above services at boot ie each time your system starts
```
systemctl enable nginx php7.0-fpm mysql monit
```





