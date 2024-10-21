
# POSTGRES INSTALL

> The version used in this document for PostgresSQL is 16.x on Ubuntu 20.04

> The version used in this document for PostgresSQL is 10.23 on Almalinux 8.10

### Ubuntu 20.04

###### Postgres Installing

- System Update

<pre>
sudo apt-get update
</pre>

- Prepare environment

<pre>
sudo apt install wget ca-certificates

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

sudo apt update
sudo apt-get update
</pre>

- Install PostgresSQL

<pre>
sudo apt install postgresql postgresql-contrib
</pre>

- Allow firewall postgresql port
<pre>
sudo ufw allow 5432/tcp
sudo ufw reload
</pre>

- Enable PostgresSQL service

<pre>
sudo systemctl stop postgresql
sudo systemctl daemon-reload
sudo systemctl enable postgresql
</pre>

- Manage PostgresSQL service

<pre>
sudo systemctl start postgresql
sudo systemctl status postgresql
</pre>

- Access PostgresSQL

<pre>
sudo -u postgres psql
</pre>

- PostgresSQL Version

<pre>
sudo -u postgres psql
psql (16.3 (Ubuntu 16.3-1.pgdg20.04+1))
Type "help" for help.

postgres=# SELECT version();
                                                              version                                                              
-----------------------------------------------------------------------------------------------------------------------------------
 PostgreSQL 16.3 (Ubuntu 16.3-1.pgdg20.04+1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 9.4.0-1ubuntu1~20.04.2) 9.4.0, 64-bit
(1 row)

postgres=#
</pre>

- Manage PostgresSQL Database

<pre>
#LIST ALL USERS
\du

#SET PASSWORD
\password postgres

#QUIT
\q
</pre>

- Configure PostgresSQL Server

<pre>
sudo vi /etc/postgresql/${POSTGRES-VERSION}/main/postgresql.conf
</pre>

<pre>
listen_addresses = '*'
</pre>

<pre>
sudo vi /etc/postgresql/${POSTGRES-VERSION}/main/pg_hba.conf
</pre>

<pre>
host all all 0.0.0.0/0 md5
</pre>

<pre>
sudo systemctl restart postgresql
</pre>

- Check port listening 5432

<pre>
sudo ss -nlt | grep 5432
</pre>

- Connect via CLI remotely

<pre>
psql -h 5.199.162.56 -p 5432 -d test_erp -U postgres
</pre>

- Connect to postgres database with DBeaver

<pre>
Host: 192.168.26
Port: 5432
Database: postgres
Authentication: Database Native
Username: postgres
Password: postgres
</pre>

> See more details in the link https://github.com/huntercodexs/docker-series/tree/databases?tab=readme-ov-file#postgres

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 5432"
sudo ufw delete allow 5432/tcp
sudo ufw reload

sleep 2

echo "Stopping and removing service - postgresql.service"
sudo systemctl stop postgresql.service
sudo service postgresql stop
sudo rm -f /etc/systemd/system/postgresql.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/postgresql.service
sudo systemctl daemon-reload

sleep 2

echo "Removing postgresql, folders and links"
sudo apt remove postgresql* postgresql-contrib* -y
sudo apt remove wget ca-certificates -y
sudo apt-get remove postgresql* -y
sudo apt-get remove wget ca-certificates -y
sudo apt-get remove --auto-remove -y
sudo dpkg --purge postgresql-16
sudo dpkg --purge postgresql-client-common 
sudo dpkg --purge postgresql-common 

cd /
sudo rm -rf /etc/apt/sources.list.d/pgdg.list
sudo find . | grep -v home | grep postgres | xargs -i sudo rm -rf {} >> /dev/null 2>&1

sleep 2

echo "Removing user - postgresql"
sudo userdel postgresql
sudo userdel postgres
sudo userdel postgre
cd /home/ubuntu-vbox

sleep 2

echo "DONE"
exit
</pre>

> IMPORTANT: In some cases it will be necessary to restart the machine

### Almalinux 8.10

###### Postgres Installing

- System Update

<pre>
sudo yum update
sudo dnf update
</pre>

- Install PostgresSQL

<pre>
sudo dnf install postgresql-server postgresql-contrib postgresql
</pre>

- Initialize database

<pre>
sudo postgresql-setup --initdb
</pre>

- Allow firewall postgresql port
- 
<pre>
sudo firewall-cmd --add-port=5432/tcp --permanent
sudo firewall-cmd --reload
</pre>

- Enable PostgresSQL service

<pre>
sudo systemctl stop postgresql
sudo systemctl daemon-reload
sudo systemctl enable postgresql
</pre>

- Manage PostgresSQL service

<pre>
sudo systemctl start postgresql
sudo systemctl status postgresql
</pre>

- Access PostgresSQL

<pre>
sudo -u postgres psql
</pre>

- PostgresSQL Version

<pre>
sudo -u postgres psql
psql (16.3 (Ubuntu 16.3-1.pgdg20.04+1))
Type "help" for help.

postgres=# SELECT version();
                                                              version                                                              
-----------------------------------------------------------------------------------------------------------------------------------
 PostgreSQL 16.3 (Ubuntu 16.3-1.pgdg20.04+1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 9.4.0-1ubuntu1~20.04.2) 9.4.0, 64-bit
(1 row)

postgres=#
</pre>

- Manage PostgresSQL Database

<pre>
#LIST ALL USERS
\du

#SET PASSWORD
\password postgres

#QUIT
\q
</pre>

- Configure PostgresSQL Server

<pre>
sudo vi /var/lib/pgsql/data/postgresql.conf
</pre>

<pre>
listen_addresses = '*'
</pre>

<pre>
sudo vi /var/lib/pgsql/data/pg_hba.conf
</pre>

<pre>
host all all 0.0.0.0/0 md5
</pre>

<pre>
sudo systemctl restart postgresql
</pre>

- Check port listening 5432

<pre>
sudo ss -nlt | grep 5432
</pre>

- Connect via CLI remotely

<pre>
psql -h 5.199.162.56 -p 5432 -d test_erp -U postgres
</pre>

- Connect to postgres database with DBeaver

<pre>
Host: 192.168.26
Port: 5432
Database: postgres
Authentication: Database Native
Username: postgres
Password: postgres
</pre>

> See more details in the link https://github.com/huntercodexs/docker-series/tree/databases?tab=readme-ov-file#postgres

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 5432"
sudo firewall-cmd --zone=public --remove-port=5432/tcp
sudo firewall-cmd --runtime-to-permanent 
sudo firewall-cmd --reload

sleep 2

echo "Stopping and removing service - postgresql.service"
sudo systemctl stop postgresql.service
sudo service postgresql stop
sudo rm -f /etc/systemd/system/postgresql.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/postgresql.service
sudo systemctl daemon-reload

sleep 2

echo "Removing postgresql, folders and links"
sudo yum remove postgresql-server postgresql-contrib postgresql -y
sudo dnf remove postgresql-server postgresql-contrib postgresql -y
sudo yum autoremove -y

cd /
sudo rm -rf /etc/apt/sources.list.d/pgdg.list
sudo find . | grep -v home | egrep "postgres|pgsql" | xargs -i sudo rm -rf {} >> /dev/null 2>&1

sleep 2

echo "Removing user - postgresql"
sudo userdel postgresql
sudo userdel postgres
sudo userdel postgre
cd /home/almalinux-vbox

sleep 2

echo "DONE"
exit
</pre>

> IMPORTANT: In some cases it will be necessary to restart the machine

