
# MYSQL INSTALL

### UBUNTU 20.04
	
###### Mysql Installing

- System Update

<pre>
sudo apt update
</pre>

- Installing Mysql Server

<pre>
sudo apt install mysql-server
</pre>

- Set up the Mysql Server (root)

<pre>
sudo mysql_secure_installation
sudo mysql
SELECT user,authentication_string,plugin,host FROM mysql.user;
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '123change';
FLUSH PRIVILEGES;
SELECT user,authentication_string,plugin,host FROM mysql.user;
exit
</pre>

- Create a user

<pre>
mysql -u root -p
CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY '123change';
GRANT ALL PRIVILEGES ON *.* TO 'mysqluser'@'localhost' WITH GRANT OPTION;
ALTER USER 'mysqluser'@'localhost' IDENTIFIED WITH caching_sha2_password BY '123change';
FLUSH PRIVILEGES;
exit
mysql -u mysqluser -p
</pre>

- Create a remote user

<pre>
mysql -u root -p
CREATE USER 'mysqluser'@'%' IDENTIFIED BY '123change';
GRANT ALL PRIVILEGES ON *.* TO 'mysqluser'@'%';
FLUSH PRIVILEGES;
exit
mysql -u mysqluser -p
</pre>

- Check the mysql service status

<pre>
systemctl status mysql.service
</pre>

- Allow remote access

> Be sure that you will not expose your database in the bad way

<pre>
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
</pre>

<pre>
#bind-address           = 127.0.0.1
#mysqlx-bind-address    = 127.0.0.1
bind-address            = 0.0.0.0
mysqlx-bind-address     = 0.0.0.0
</pre>

<pre>
sudo ufw allow 3306
sudo ufw allow from 0.0.0.0 to any port 3306
sudo ufw reload
sudo systemctl restart mysql
</pre>

### ALMALINUX 8.10

###### Mysql Installing

- System Update

<pre>
sudo yum update
sudo dnf update
</pre>

- Installing Mysql Server

<pre>
sudo dnf install mysql-server
</pre>

- Manage the service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable mysqld
sudo systemctl start mysqld.service
sudo systemctl status mysqld.service
</pre>

- Allow firewall mysql port

<pre>
sudo firewall-cmd --add-port=3306/tcp --permanent
sudo firewall-cmd --reload
</pre>

- Set up the mysql security

<pre>
sudo mysql_secure_installation
mysql -u root -p
SELECT user,authentication_string,plugin,host FROM mysql.user;
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '123change';
FLUSH PRIVILEGES;
SELECT user,authentication_string,plugin,host FROM mysql.user;
exit
</pre>

- Create a localhost user

<pre>
mysql -u root -p
CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY '123change';
GRANT ALL PRIVILEGES ON *.* TO 'mysqluser'@'localhost' WITH GRANT OPTION;
ALTER USER 'mysqluser'@'localhost' IDENTIFIED WITH caching_sha2_password BY '123change';
FLUSH PRIVILEGES;
exit
mysql -u mysqluser -p
</pre>

- Create a remote user

<pre>
mysql -u root -p
CREATE USER 'mysqluser'@'%' IDENTIFIED BY '123change';
GRANT ALL PRIVILEGES ON *.* TO 'mysqluser'@'%';
FLUSH PRIVILEGES;
exit
mysql -u mysqluser -p
</pre>

