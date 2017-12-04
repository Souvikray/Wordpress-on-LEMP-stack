Now you must be tired of entering password each time you log in to your VPS.There is an alternative way to log in which would let you log in without having to type in your password.It is called SSH key based login.

SSH also known as Secure Socket Shell, is a network protocol that provides administrators with a secure way to access a remote computer.An SSH key will let you automatically log into your server from one particular computer without needing to enter your password. This is convenient if you make frequent SSH and scp connections to your server.

First log in to your server and make sure you are currently as a root user.If not type in the below command and enter your password.
```
su - root
```
Go to your home directory and create a .ssh directory
```
cd /root && mkdir .ssh
```
