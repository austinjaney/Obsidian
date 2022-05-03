# 23rd September 2020 ShoreTel T1k work
#shoretel #hardware 

ShoreTel T1k default local login:

| Username: | anonymous |
| Password: | ShoreTel |

SSH did not work with the default login but probably would with another login:

```
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 anonymous@172.20.21.3
```
