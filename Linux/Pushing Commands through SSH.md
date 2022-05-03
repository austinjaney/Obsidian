# Pushing Commands through SSH
#macos #linux #network 

You can issue commands remotely by pushing them through ssh like:

ssh ajaney@serverip ls

* this will return the output of ls without dropping into the shell
* this can be used to kick off scripts without connecting to a session.

scp for file transfers scp is a secure file copy tool that uses ssh

```bash
scp file.txt ajaney@serveripaddress:~/
```

this will send the file.txt document to the home folder of ajaney on the specified server

sftp can be used to download files

```bash
sftp ajaney@serverip
```

this will launch the sftp tool that can be used to download different files you have permissions to from the specified server

sftp will let you grab files by using the get command and specifying the file you would like to grab

grabbing a file will put it into the folder you were in when you launched sftp.

If you were in your desktop directory when you launched sftp and then used the get command if would grab the file you specified and put it on your desktop.

you can similarly use the put command to put files on the remote system. CTRL R will let you search your previous command history