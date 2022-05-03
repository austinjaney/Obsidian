# Encrypt files with OpenSSL in macOS
#macos #mdm #sysadmin 

you can use a password to encrypt files in macOS, this is useful if you want to store an encrypted blob in a cloud storage service like backblaze or aws to call down with a script.

Encrypt a file

```bash
openssl aes-256-cbc -a -in ~/Desktop/fielddeploy.sh -out ~/Desktop/Encrypt/fielddeploy.enc -pass pass:KazfABPhgtpi1z
```

Decrypt an encrypted file

```bash
openssl aes-256-cbc -a -d -in ~/Desktop/fielddeploy.enc -out ~/Desktop/fielddeploy.sh -pass pass:KazfABPhgtpi1z
```