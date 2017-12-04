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
Now in your local computer, generate a key
```
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa -C "optional comment"
```
Note- The default directory and name for new keys is ~/.ssh/id_rsa, and this is where SSH will look for your keys.

Now you should receive a prompt asking to enter passphrase.Please use a strong password.Once you enter the passphrase twice, you should see something like this below
```
Your identification has been saved in /Users/username/.ssh/id_rsa.
Your public key has been saved in /Users/username/.ssh/id_rsa.pub.
The key fingerprint is:
60:b5:c1:b7:ee:ab:31:d1:70:d8:03:41:df:0f:08:eb Enter an optional comment about your key
The key's randomart image is:
+--[ RSA 2048]----+
|     .=.         |
|     . B o       |
|      X B o      |
|     o X o o     |
|      E S   .    |
|       o         |
|      o .        |
|       +         |
|      ..o.       |
+-----------------+
```
Make sure your .ssh directory and the files it contains have the correct permissions
```
chmod 700 ~/.ssh && chmod 600 ~/.ssh/*
```
You need to upload your public key to your server. The command below reads the content of the key you just created on your computer, and appends that key to the authorized_keys file on your server. If you don't have an existing authorized_keys file, it creates one.
```
cat ~/.ssh/id_rsa.pub | ssh root@139.59.94.123 'cat - >> ~/.ssh/authorized_keys'
```
Now in your remote server, ensure your .ssh directory is on the server, and the files it contains, have the correct permissions.
```
chmod 600 ~/.ssh/authorized_keys && chmod 700 ~/.ssh/
```
That's it fellas! You should now be able to log into your server from this computer without being prompted for a password.
