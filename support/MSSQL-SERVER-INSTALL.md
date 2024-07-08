
# MSSQL SERVER INSTALL

> The version used in this document for MSSQL Server is 2022

> You can use DOCKER + DOCKER-COMPOSE to make it quickly and easier just check the follow link 
> https://github.com/huntercodexs/docker-series/tree/databases?tab=readme-ov-file#mssql

### Requirements

- 2GB memory

### Ubuntu 20.04

###### MSSQL Server Installing

- System Update

<pre>
sudo apt-get update
sudo apt-get upgrade
</pre>

- Prepare environment

<pre>
sudo apt install software-properties-common
sudo apt-get install wget
sudo apt update
</pre>

<pre>
sudo wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
</pre>

Choose the MSSQL Server version

<pre>
# 2017
sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017.list)"

# 2019
sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/20.04/mssql-server-2019.list)"

# 2022
sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/20.04/mssql-server-2022.list)"
</pre>

<pre>
sudo apt-get update
</pre>

- Install MSSQL Server

<pre>
sudo apt-get install mssql-server
</pre>

- Configure the MSSQL Server

<pre>
sudo /opt/mssql/bin/mssql-conf setup

#PASSWORD
mssql1Ipw
</pre>

- Manage MSSQL Server service

<pre>
sudo systemctl stop mssql-server
sudo systemctl daemon-reload
sudo systemctl enable mssql-server
sudo systemctl start mssql-server
sudo systemctl status mssql-server
</pre>

- Allow firewall MSSQL port

<pre>
sudo ufw allow 1433/tcp
sudo ufw allow 31812/tcp
sudo ufw reload
</pre>

- MSSQL tools install (optional)

<pre>
curl https://packages.microsoft.com/keys/microsoft.asc | sudo tee /etc/apt/trusted.gpg.d/microsoft.asc

curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/mssql-release.list

sudo apt-get update

#Commom version
sudo apt-get install mssql-tools

#Latest version
sudo apt-get install mssql-tools18

sudo apt-get install unixodbc-dev

PATH=$PATH:/opt/mssql-tools/bin
export PATH
</pre>

- Connect locally

<pre>
sqlcmd -S localhost -U sa -P 'mssql1Ipw'
</pre>

- Create a database sample

<pre>
#LIST DATABASES (SHOW TABLES)
SELECT Name from sys.databases;
GO

#CREATE DATABASE
CREATE DATABASE testdb;
GO

#CREATE TABLE
USE testdb;
CREATE TABLE dbo.Inventory (
    id INT,
    name NVARCHAR(50),
    quantity INT,
    PRIMARY KEY (id)
);
GO

#INSERT DATA
INSERT INTO dbo.Inventory VALUES (1, 'banana', 150);
INSERT INTO dbo.Inventory VALUES (2, 'orange', 154);

#SELECT DATA
SELECT * FROM dbo.Inventory WHERE quantity > 152;
GO

#EXIT
QUIT
</pre>

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 1433"
sudo ufw delete allow 1433/tcp
echo "Locking firewall - port 31812"
sudo ufw delete allow 31812/tcp
sudo ufw reload

sleep 2

echo "Stopping and removing service - mssql.service"
sudo systemctl stop mssql-server.service
sudo service mssql-server stop
sudo rm -f /etc/systemd/system/mssql-server.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/mssql-server.service
sudo systemctl daemon-reload

sleep 2

echo "Removing MSSQL Server, folders and links"
sudo apt-get remove mssql* -y
sudo apt-get remove mssql* -y
sudo apt-get remove unixodbc-dev -y
sudo apt-get remove --auto-remove -y
sudo dpkg --purge mssql-server
sudo dpkg --purge mssql-tools
sudo dpkg --purge mssql-tools18

cd /
sudo rm -rf /etc/apt/sources.list.d/pgdg.list
sudo find . | grep -v home | grep mssql | xargs -i sudo rm -rf {} >> /dev/null 2>&1

sleep 2

echo "Removing user - mssql"
sudo userdel mssql
cd /home/ubuntu-vbox

sleep 2

echo "DONE"
exit
</pre>

### Almalinux 8.10

###### MSSQL Server Installing

- System Update

<pre>
sudo yum update
sudo dnf update
</pre>

- Prepare environment

<pre>
sudo curl https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo -o /etc/yum.repos.d/mssql-server-2019.repo
sudo curl https://packages.microsoft.com/config/rhel/8/prod.repo -o /etc/yum.repos.d/msprod.repo
</pre>

<pre>
sudo yum update
sudo dnf update
</pre>

- Install MSSQL Server

<pre>
sudo dnf install mssql-server
sudo dnf install mssql-tools unixODBC-devel

# Do you accept the license terms? (Enter YES or NO) YES
# Do you accept the license terms? (Enter YES or NO) YES
</pre>

- Configure the MSSQL Server

<pre>
sudo /opt/mssql/bin/mssql-conf setup

#PASSWORD
mssql1Ipw
</pre>

- Manage MSSQL Server service

<pre>
sudo systemctl stop mssql-server
sudo systemctl daemon-reload
sudo systemctl enable mssql-server
sudo systemctl start mssql-server
sudo systemctl status mssql-server
</pre>

- Allow firewall MSSQL port

<pre>
sudo firewall-cmd --add-port=1433/tcp --permanent
sudo firewall-cmd --add-port=31812/tcp --permanent
sudo firewall-cmd --reload
</pre>

- Connect locally

<pre>
sqlcmd -S localhost -U sa -P 'mssql1Ipw'
</pre>

- Create a database sample

<pre>
#LIST DATABASES (SHOW TABLES)
SELECT Name from sys.databases;
GO

#CREATE DATABASE
CREATE DATABASE testdb;
GO

#CREATE TABLE
USE testdb;
CREATE TABLE dbo.Inventory (
    id INT,
    name NVARCHAR(50),
    quantity INT,
    PRIMARY KEY (id)
);
GO

#INSERT DATA
INSERT INTO dbo.Inventory VALUES (1, 'banana', 150);
INSERT INTO dbo.Inventory VALUES (2, 'orange', 154);

#SELECT DATA
SELECT * FROM dbo.Inventory WHERE quantity > 152;
GO

#EXIT
QUIT
</pre>

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 1433"
sudo firewall-cmd --zone=public --remove-port=1433/tcp
echo "Locking firewall - port 31812"
sudo firewall-cmd --zone=public --remove-port=31812/tcp
sudo firewall-cmd --runtime-to-permanent 
sudo firewall-cmd --reload

sleep 2

echo "Stopping and removing service - mssql.service"
sudo systemctl stop mssql-server.service
sudo service mssql-server stop
sudo rm -f /etc/systemd/system/mssql-server.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/mssql-server.service
sudo systemctl daemon-reload

sleep 2

echo "Removing MSSQL Server, folders and links"
sudo yum remove mssql* -y
sudo dnf remove mssql* -y
sudo dnf remove unixodbc-dev* -y
sudo yum autoremove -y

cd /
sudo rm -rf /etc/apt/sources.list.d/pgdg.list
sudo find . | grep -v home | grep mssql | xargs -i sudo rm -rf {} >> /dev/null 2>&1

sleep 2

echo "Removing user - mssql"
sudo userdel mssql
cd /home/almalinux-vbox

sleep 2

echo "DONE"
exit
</pre>
