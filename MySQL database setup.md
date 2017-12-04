Create MySQL root pass.The command below is basically creating an aplhanumberic 16 character length long password with an @ at the beginning.
```
echo -n @ && cat /dev/urandom | env LC_CTYPE=C tr -dc [:alnum:] | head -c 15 && echo
```
Execute the MySQL installation script.Press 'Y' for all questions
```
/usr/bin/mysql_secure_installation
```
Restart MySQL
```
systemctl restart mysql
```
