
# MONGODB INSTALL

> The version used in this document for MongoBO is 7.0.11

### Ubuntu 20.04

###### Mongodb Installing

- System Update

<pre>
sudo apt-get update
</pre>

- Prepare environment

<pre>
sudo apt-get install gnupg curl

curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor

echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

sudo apt-get update
</pre>

- Install MongoDB

<pre>
#sudo apt-get install mongodb-org=7.0.7 mongodb-org-database=7.0.7 mongodb-org-server=7.0.7 mongodb-mongosh=7.0.7 mongodb-org-mongos=7.0.7 mongodb-org-tools=7.0.7
sudo apt-get install mongodb-org
</pre>

- Lock MongoDB apt-get update (optional)

<pre>
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-database hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-mongosh hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
</pre>

- Check directories

<pre>
/var/lib/mongodb
/var/log/mongodb
</pre>

- MongoDB configuration file

<pre>
/etc/mongod.conf
</pre>

- Allow firewall mongodb port

> 27017 The default port for mongod and mongos instances. You can change this port with port or --port.

> 27018 The default port for mongod when running with --shardsvr command-line option or the shardsvr value for the clusterRole setting in a configuration file.

> 27019 The default port for mongod when running with --configsvr command-line option or the configsvr value for the clusterRole setting in a configuration file.

> 27020 The default port from which mongocryptd listens for messages. mongocryptd is installed with MongoDB Enterprise Server and supports automatic encryption operations.

<pre>
sudo ufw allow 27017/tcp
sudo ufw allow 27018/tcp
sudo ufw allow 27019/tcp
sudo ufw allow 27020/tcp
sudo ufw reload
</pre>

- Enable MongoDB service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable mongod
</pre>

- Manage MongoDB service

<pre>
sudo systemctl stop mongod
sudo systemctl start mongod
sudo systemctl status mongod
</pre>

- Check MongoDB version

<pre>
mongod --version
</pre>

<pre>
db version v7.0.11
Build Info: {
    "version": "7.0.11",
    "gitVersion": "f451220f0df2b9dfe073f1521837f8ec5c208a8c",
    "openSSLVersion": "OpenSSL 1.1.1f  31 Mar 2020",
    "modules": [],
    "allocator": "tcmalloc",
    "environment": {
        "distmod": "ubuntu2004",
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}

</pre>

- Access MongoDB

<pre>
mongosh
</pre>

- Manage MongoDB Database

<pre>
#SHOW DATABASES
show dbs

#CREATE DATABASE
use dbtest
db

#CREATE USER
db.createUser(
    {
        user: "test",
        pwd: "123change",
        roles: [{role: "readWrite", db: "dbtest"}]
    }
)

#SELECT USERS
db.getUsers()
show users

#DELETE USER
db.dropUser("test", {w: "majority", wtimeout: 4000})
</pre>

- Secure MongoDB

<pre>
mongosh
</pre>

<pre>
use admin
</pre>

<pre>
db.createUser(
  {
    user: "adminTest",
    pwd: passwordPrompt(),
    roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
 }
)
</pre>

<pre>
sudo vi /etc/mongod.conf
</pre>

<pre>
security:
  authorization: enabled
</pre>

<pre>
sudo systemctl restart mongod
</pre>

<pre>
mongosh  "mongodb://adminTest@mongo-ip-address:27017"
</pre>

- Remote Access

<pre>
sudo vi /etc/mongod.conf
</pre>

<pre>
# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1, mongo-server-ip
</pre>

Install tools for access remotely

<pre>
sudo apt install netcat
</pre>

<pre>
nc -zv mongodb_server_ip 27017
</pre>

- Database MongoDB handler

Data insert

<pre>
db.staff.insertOne({ name: "Alice", age: 25, city: "London", married: true, hobbies: ["Travelling", "Swimming", "Cooking"] })
</pre>

Retrieve data

<pre>
db.staff.find()
db.staff.find({ married: true })
</pre>

Update data

<pre>
db.staff.update({ name: "Bob" }, {$set: { name: "Robert" }})
</pre>

Delete data

<pre>
.deleteOne()
.deleteMany() 
</pre>

<pre>
db.staff.deleteOne({ name: "Robert"})
db.staff.deleteMany({married: true})
</pre>

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 27017"
sudo ufw delete allow 27017/tcp
echo "Locking firewall - port 27018"
sudo ufw delete allow 27018/tcp
echo "Locking firewall - port 27019"
sudo ufw delete allow 27019/tcp
echo "Locking firewall - port 27020"
sudo ufw delete allow 27020/tcp
sudo ufw reload

sleep 2

echo "Stopping and removing service - mongodb.service"
sudo systemctl stop mongod.service
sudo service mongod stop
sudo rm -f /etc/systemd/system/mongod.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/mongod.service
sudo rm -f /etc/init.d/mongod
sudo systemctl daemon-reload

sleep 2

echo "Removing mongodb, folders and links"
sudo apt-get remove mongodb* -y
sudo apt-get remove --auto-remove -y
cd /
sudo find . | grep -v home | grep mongo | xargs -i sudo rm -rf {}
cd $HOME
sudo rm -rf $HOME/.mongodb/*

sleep 2

echo "Removing user - mongodb"
sudo userdel mongodb
cd /home/ubuntu-vbox

sleep 2

echo "DONE"
exit
</pre>

### Almalinux 8.10

###### Mongodb Installing

- System Update

<pre>
sudo yum update -y
sudo dnf update -y
</pre>

- Prepare environment

<pre>
sudo vi /etc/yum.repos.d/mongodb-org-7.0.repo
</pre>

<pre>
[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/8/mongodb-org/7.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://pgp.mongodb.com/server-7.0.asc
</pre>

<pre>
sudo yum update -y
sudo dnf update -y
</pre>

- Install MongoDB

<pre>
sudo yum install mongodb-org
</pre>

- Lock MongoDB apt-get update (optional)

<pre>
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-database hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-mongosh hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
</pre>

- Check directories

<pre>
/var/lib/mongodb
/var/log/mongodb
</pre>

- MongoDB configuration file

<pre>
/etc/mongod.conf
</pre>

- Allow firewall mongodb port

> 27017 The default port for mongod and mongos instances. You can change this port with port or --port.

> 27018 The default port for mongod when running with --shardsvr command-line option or the shardsvr value for the clusterRole setting in a configuration file.

> 27019 The default port for mongod when running with --configsvr command-line option or the configsvr value for the clusterRole setting in a configuration file.

> 27020 The default port from which mongocryptd listens for messages. mongocryptd is installed with MongoDB Enterprise Server and supports automatic encryption operations.

<pre>
sudo firewall-cmd --add-port=27017/tcp --permanent
sudo firewall-cmd --add-port=27018/tcp --permanent
sudo firewall-cmd --add-port=27019/tcp --permanent
sudo firewall-cmd --add-port=27020/tcp --permanent
sudo firewall-cmd --reload
</pre>

- Enable MongoDB service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable mongod
</pre>

- Manage MongoDB service

<pre>
sudo systemctl stop mongod
sudo systemctl start mongod
sudo systemctl status mongod
</pre>

- Check MongoDB version

<pre>
mongod --version
</pre>

<pre>
db version v7.0.11
Build Info: {
    "version": "7.0.11",
    "gitVersion": "f451220f0df2b9dfe073f1521837f8ec5c208a8c",
    "openSSLVersion": "OpenSSL 1.1.1f  31 Mar 2020",
    "modules": [],
    "allocator": "tcmalloc",
    "environment": {
        "distmod": "ubuntu2004",
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}

</pre>

- Access MongoDB

<pre>
mongosh
</pre>

- Manage MongoDB Database

<pre>
#SHOW DATABASES
show dbs

#CREATE DATABASE
use dbtest
db

#CREATE USER
db.createUser(
    {
        user: "test",
        pwd: "123change",
        roles: [{role: "readWrite", db: "dbtest"}]
    }
)

#SELECT USERS
db.getUsers()
show users

#DELETE USER
db.dropUser("test", {w: "majority", wtimeout: 4000})
</pre>

- Secure MongoDB

<pre>
mongosh
</pre>

<pre>
use admin
</pre>

<pre>
db.createUser(
  {
    user: "adminTest",
    pwd: passwordPrompt(),
    roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
 }
)
</pre>

<pre>
sudo vi /etc/mongod.conf
</pre>

<pre>
security:
  authorization: enabled
</pre>

<pre>
sudo systemctl restart mongod
</pre>

<pre>
mongosh  "mongodb://adminTest@mongo-ip-address:27017"
</pre>

- Remote Access

<pre>
sudo vi /etc/mongod.conf
</pre>

<pre>
# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1, mongo-server-ip
</pre>

Install tools for access remotely

<pre>
sudo apt install netcat
</pre>

<pre>
nc -zv mongodb_server_ip 27017
</pre>

- Database MongoDB handler

Data insert

<pre>
db.staff.insertOne({ name: "Alice", age: 25, city: "London", married: true, hobbies: ["Travelling", "Swimming", "Cooking"] })
</pre>

Retrieve data

<pre>
db.staff.find()
db.staff.find({ married: true })
</pre>

Update data

<pre>
db.staff.update({ name: "Bob" }, {$set: { name: "Robert" }})
</pre>

Delete data

<pre>
.deleteOne()
.deleteMany() 
</pre>

<pre>
db.staff.deleteOne({ name: "Robert"})
db.staff.deleteMany({married: true})
</pre>

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 27017"
sudo firewall-cmd --zone=public --remove-port=27017/tcp
echo "Locking firewall - port 27018"
sudo firewall-cmd --zone=public --remove-port=27018/tcp
echo "Locking firewall - port 27019"
sudo firewall-cmd --zone=public --remove-port=27019/tcp
echo "Locking firewall - port 27020"
sudo firewall-cmd --zone=public --remove-port=27020/tcp
sudo firewall-cmd --runtime-to-permanent 
sudo firewall-cmd --reload

sleep 2

echo "Stopping and removing service - mongodb.service"
sudo systemctl stop mongod.service
sudo service mongod stop
sudo rm -f /etc/systemd/system/mongod.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/mongod.service
sudo rm -f /etc/init.d/mongod
sudo systemctl daemon-reload

sleep 2

echo "Removing mongodb, folders and links"
sudo yum remove mongodb* -y
sudo dnf remove mongodb* -y
sudo yum autoremove -y
cd /
sudo find . | grep -v home | grep mongo | xargs -i sudo rm -rf {}
cd $HOME
sudo rm -rf $HOME/.mongodb

sleep 2

echo "Removing user - mongodb"
sudo userdel mongodb
cd /home/almalinux-vbox

sleep 2

echo "DONE"
exit
</pre>

