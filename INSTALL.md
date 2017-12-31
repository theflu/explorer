# INSTALL Explorer Ubuntu 16.04

## Install MongoDB Community Edition
### https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/

```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
$ echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
$ sudo apt-get update
$ sudo apt-get install -y mongodb-org
```

## Set MongoDB to auto start
```
$ sudo nano /etc/systemd/system/mongod.service
```
##### Add
```
[Unit]
Description=MongoDB Database Service
Wants=network.target
After=network.target

[Service]
ExecStart=/usr/bin/mongod --config /etc/mongod.conf
ExecReload=/bin/kill -HUP $MAINPID
Restart=always
User=mongodb
Group=mongodb
StandardOutput=syslog
StandardError=syslog

[Install]
WantedBy=multi-user.target
```

```
$ sudo systemctl daemon-reload
$ sudo systemctl start mongod
$ sudo systemctl enable mongod
```

## Creating a MongoDB Database
```
$ sudo mongo
> use explorerdb
> db.createUser( { user: "3er44we222", pwd: "3eu4ue8hhk988!kd8", roles: [ "readWrite" ] } )
> exit
```

## Installing Nodejs
```
$ sudo apt-get update
$ sudo apt-get install nodejs nodejs-legacy -y
$ sudo apt-get install npm -y
```

## Installing the Explorer
```
$ cd /opt/
$ git clone https://github.com/theflu/explorer.git
$ cd explorer && npm install --production
$ cp ./settings.json.template ./settings.json
```

## Modify the Settings File
```
$ sudo nano settings.json
```

## Setup CRON
```
@reboot /usr/local/bin/coind
@reboot cd /opt/explorer && forver start bin/cluster
*/1 * * * * cd /opt/explorer && /usr/bin/nodejs scripts/sync.js index update > /dev/null 2>&1
*/2 * * * * cd /opt/explorer && /usr/bin/nodejs scripts/sync.js market > /dev/null 2>&1
*/5 * * * * cd /opt/explorer && /usr/bin/nodejs scripts/peers.js > /dev/null 2>&1

## Start Explorer
```
$ cd /opt/explorer
$ forver start bin/cluster
```