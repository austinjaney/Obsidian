# Ubuntu Syncthing Server Setup
#ubuntu #linux

Add the release PGP keys:

```
curl -s https://syncthing.net/release-key.txt | sudo apt-key add -
```

Add the "stable" channel to your APT sources:

```
echo "deb https://apt.syncthing.net/ syncthing stable" | sudo tee /etc/apt/sources.list.d/syncthing.list
```

Update and install syncthing:

```
sudo apt-get update
sudo apt-get install syncthing
```

created user: syncthing
Enable and Start with Systemd
USER=root or whatever
probably best not to use root

```
sudo systemctl enable syncthing@$USER.service
sudo systemctl start syncthing@$USER.service
```

config page for syncthing located at /home/syncthing/.config/syncthing/config.xml

```
vi  /home/syncthing/.config/syncthing/config.xml
```

change http://127.0.0.1:8385/ to http://0.0.0.0:8080/

Once done I installed zerotier and called it a day. Zerotier allows the syncthing server and client nodes to talk to eachother easily without any public ip or introduction node being a necessity.