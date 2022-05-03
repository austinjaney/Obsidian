# Jamf enrollment via Bash
#bash #mdm #macos 

Script to download and install the jamf enrollment package

For this script to work first upload an encrypted JAMF PKG as an encrypted file to a bucket service like backblaze where you can use curl to grab it.


```bash
#/bin/bash

curl -o /tmp/enrollment.enc https://f000.backblazeb2.com/b2api/v1/b2_download_file_by_id?fileId=4_z80e7bbd66e4331a868990f1b_f1048ff29b70147a5_d20190301_m190129_c000_v0001047_t0044

#Decrypt the downloaded file

openssl aes-256-cbc -a -d -in /tmp/enrollment.enc -out /tmp/enrollment.pkg -pass pass:MYctngyqN08yrjdW

# Execute the Jamf agent
installer -pkg /tmp/enrollment.pkg -target /
```