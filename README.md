I am going to build a wordpress hosting platform running on a VPS.Below is one of the Wordpress websites running on a LEMP stack.

www.jukeboxhero.xyz
![Alt text](https://github.com/Souvikray/Wordpress-on-LEMP-stack/blob/master/screenshot1.png?raw=true "Optional Title")

LEMP stack

<b> L </b> - Linux  <b>  E </b> - nginx  <b>  M </b> - MySQL  <b>  P </b> - PHP-FPM

Let us understand each of the technologies here

<b> VPS </b>

VPS stands for Virtual Private Server. A physical server is housed in a data center, the location of which depends upon the hosting provider you’re using. A VPS is a partitioned part of one of these servers that contains it’s own operating system, bandwidth, and disc space.This gives you the liberty to install your own softwares, maintain and customize it the way you want.

Here we are using Digital Ocean VPS which has economical pricing should you opt for a plan and provides wonderful serivce.

<b> Linux </b>

I am specifically using Linux Ubuntu since it is my primary OS and so I am quite comfortable working on it.But again it boils down to a user's personal preference.

<b> nginx </b>

NGINX is a free, open-source, high-performance HTTP server and reverse proxy.It is known for its high performance, stability, rich feature set, simple configuration, and low resource consumption.

Unlike traditional servers, NGINX doesn’t rely on threads to handle requests. Instead it uses a much more scalable event-driven (asynchronous) architecture. This architecture uses small, but more importantly, predictable amounts of memory under load.

<b> MySQL </b>

MySQL is an open-source relational database management system (RDBMS).It is ideal for both small and large applications.It is very fast, reliable, and easy to use.It uses standard SQL

Here I am using MySQL to store user data since it is stable, has been around for very long time, has huge community of developers and is a primary database of large companies like Facebook, Adobe etc.Once again it boils down to a user's personal choice and they may use any database they like.

<b> PHP-FPM </b>

PHP FastCGI Process Manager (PHP-FPM) is an alternative FastCGI daemon for PHP that allows a website to handle serious loads. PHP-FPM maintains "pools" (workers available to respond to PHP requests) to accomplish this.PHP-FPM is faster than traditional CGI-based methods. It does not overload a system's memory with PHP from Apache processes.

I am using php-fpm over apache for this set up.Nothing personal :)

<b> Wordpress </b>

WordPress is a free and open-source content management system (CMS) based on PHP and MySQL.To function, WordPress has to be installed on a web server, which would either be part of an Internet hosting service or a network host in its own right.

The best part about Wordpress is that you don't have to spend hours creating a website.Everything you need has a plugin or theme which can be easily set up.Here my work as an administrator relies on the backend of the website and and not on the designing aspect so Wordpress saves a lot of time on that and lets us focus on the real work.

<b> Monit </b>

In the end, we will be also setting up a monitoring tool called 'monit' to monitor the site activities and ensure all the components of the website are functioning properly.

<b> Activities <b>

Set up a web server, an interpreter and a database
  
Enable SSH based log in to the server  
  
Set up wordpress sites for multiple users
  
Implement appropriate file permissions and configurations
  
Monitor servers
  
Backup databases and site files periodically  




