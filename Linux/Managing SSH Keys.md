# Managing SSH Keys
#sysadmin #linux 

SSH has a great feature. It allows you to setup a private and public key pair with which you can SSH into the server without requiring a password. There are various reasons why you should want that. For example, you might be lazy (and you don't like typing it in). Another possibility is that you may want to have scripts which automatically login into the server (and fetch or place data). Setting it up is easy.

On the client machine (the one from which you login), type ssh-keygen -t rsa and three times press 'enter'. You will see the following response (or something similar).

```
% ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your-user/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/your-user/.ssh/id_rsa.
Your public key has been saved in /Users/your-user/.ssh/id_rsa.pub.
The key fingerprint is:
96:a5:ef:83:65:4b:a6:e5:d7:90:a0:e6:b3:e7:b2:06 yours-user@your-machine
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|                 |
|          .      |
|         +.      |
|        S. . .   |
|      E.o.* o    |
|       + X.. o   |
|        B.= . .  |
|       .oBoo     |
+-----------------+
```

Note that you generated a private key which is saved in ~/.ssh/id_rsa and a public key in ~/.ssh/id_rsa.pub. The next step is to copy the contents of the public key (e.g., type cat ~/.ssh/id_rsa.pub) and add them to the file .ssh/authorized_keys on the server machine (the computer to which you want to login). If such a file does not exist, then you should make a new one. Make sure however that your home directory, the .ssh directory and the files cannot be written by other people. If so, SSH will not like it! To make sure that everything has the right permissions, type

```
chmod 755 ~ ~/.ssh
chmod 644 ~/.ssh/authorized_keys
chmod 400 {any private key files}
```

Try it by SSHing into the server. It should not ask you for a password. Note that you should do that only if you trust the security on your client machine. The reason is that any intruder on the client will also be able to gain access to your server machine.
That's it!