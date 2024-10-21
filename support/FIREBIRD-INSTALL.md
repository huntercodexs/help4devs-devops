
# FIREBIRD INSTALL

> The current version for Firebird is 3.0 on Ubuntu 20.04

> The current version for Firebird is 4.0 on Almalinux 8.10

### Ubuntu 20.04

###### Firebird Installing

- System Update

<pre>
sudo add-apt-repository ppa:mapopa/firebird3.0
sudo apt-get update
</pre>

- Install the firebird

<pre>
sudo apt-get install firebird3.0-server
Inform the SYSDBA password
</pre>

> NOTE: In case of the step above don't ask for a password you can manually change the 
> password for SYSDBA in the following file

<pre>
sudo vi /etc/firebird/3.0/SYSDBA.password
</pre>

- Activate the firebird service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable firebird3.0.service
sudo systemctl status firebird3.0
</pre>

- Allow firewall firebird port

<pre>
sudo ufw allow 3050/tcp
sudo ufw reload
</pre>

- Create a database folder

<pre>
sudo mkdir -p /etc/firebird/3.0/databases
sudo chmod 777 /etc/firebird/3.0/databases
</pre>

- Connect via ISQL Tool 

<pre>
sudo isql-fb
SQL> CREATE DATABASE '/etc/firebird/3.0/databases/dbname1.fdb';
SQL> CREATE TABLE users (id INT NOT NULL PRIMARY KEY,name VARCHAR(250) NOT NULL);
SQL> INSERT INTO users (id, name) VALUES ('94734987', 'John Smith Wiz');
SQL> COMMIT;
SQL> SELECT * FROM users;
SQL> QUIT;
</pre>

<pre>
sudo chmod 777 /etc/firebird/3.0/databases/dbname1.fdb
</pre>

- Change firebird configurations

<pre>
sudo vi /etc/firebird/3.0/firebird.conf
</pre>

<pre>
RemoteAccess = true
RemoteServicePort = 3050
AuthServer = Srp
AuthClient = Srp, Srp256, Legacy_Auth #Non Windows clients
UserManager = Srp
WireCrypt = Enabled
WireCompression = false
RemoteBindAddress =
</pre>

- Restart firebird service

<pre>
sudo systemctl stop firebird3.0
sudo systemctl start firebird3.0
sudo systemctl status firebird3.0
</pre>

- Connect via DBMS Manager (remotely: just for testing)

<pre>
host: 192.168.0.26
port: 3050
path: /etc/firebird/3.0/databases/dbname1.fdb
username: sysdba
password: ${FIREBIRD_PASSWORD} or masterkey
jdbc-url: jdbc:firebirdsql://192.168.0.26:3050//etc/firebird/3.0/databases/dbname1.fdb
Reference library file: Jaybird-3.0.12-JDK_1.8.zip (https://firebirdsql.org/en/jdbc-driver/)
  jaybird-3.0.12.jar
  connector-api-1.5.jar
  antlr-runtime-4.7.jar
</pre>

- Check the firebird files installation 

<pre>
#Configuration File
sudo ls -ltr /etc/firebird/3.0/

#Log File
sudo ls -ltr /var/log/firebird/firebird3.0.log

#ISQL Tool
sudo ls -ltr /usr/bin/isql-fb

#Safety Database
sudo ls -ltr /var/lib/firebird/3.0/system/security3.fdb
</pre>

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 3050"
sudo ufw delete allow 3050/tcp
sudo ufw reload

sleep 2

echo "Stopping and removing service - firebird3.0.service"
sudo systemctl stop firebird3.0.service
sudo service firebird3.0 stop
sudo rm -f /etc/systemd/system/firebird3.0.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/firebird3.0.service
sudo rm -f /etc/init.d/firebird3.0
sudo systemctl daemon-reload

sleep 2

echo "Removing firebird, folders and links"
sudo apt-get remove firebird3.0-server -y
sudo apt-get remove --auto-remove -y
sudo rm -rf /usr/lib/systemd/system/firebird3.0.service
sudo rm -rf /usr/lib/x86_64-linux-gnu/firebird
sudo rm -rf /usr/lib/tmpfiles.d/firebird3.0.conf
sudo rm -rf /usr/sbin/firebird
sudo rm -rf /usr/share/doc/firebird3*
sudo rm -rf /usr/share/lintian/overrides/firebird*
sudo rm -rf /usr/share/firebird*
sudo rm -rf /usr/share/man/man8/firebird.8.gz
sudo rm -rf /tmp/firebird*
sudo rm -rf /sys/fs/cgroup/memory/system.slice/firebird*
sudo rm -rf /sys/fs/cgroup/devices/system.slice/firebird*
sudo rm -rf /sys/fs/cgroup/pids/system.slice/firebird3*
sudo rm -rf /sys/fs/cgroup/systemd/system.slice/firebird3*
sudo rm -rf /sys/fs/cgroup/unified/system.slice/firebird3*
sudo rm -rf /run/firebird*
sudo rm -rf /run/systemd/units/invocation:firebird3.0.service
sudo rm -rf /etc/rc3.d/S01firebird3.0
sudo rm -rf /etc/xinetd.d/firebird30
sudo rm -rf /etc/systemd/system/multi-user.target.wants/firebird3.0.service
sudo rm -rf /etc/rc2.d/S01firebird3.0
sudo rm -rf /etc/rc6.d/K01firebird3.0
sudo rm -rf /etc/rc0.d/K01firebird3.0
sudo rm -rf /etc/firebird*
sudo rm -rf /etc/rc1.d/K01firebird3.0
sudo rm -rf /etc/logrotate.d/firebird3.0
sudo rm -rf /etc/init.d/firebird3.0
sudo rm -rf /etc/rc5.d/S01firebird3.0
sudo rm -rf /etc/rc4.d/S01firebird3.0
sudo rm -rf /var/log/firebird*
sudo rm -rf /var/cache/apt/archives/firebird*
sudo rm -rf /var/lib/systemd/deb-systemd-helper-enabled/firebird3.0.service.dsh-also
sudo rm -rf /var/lib/systemd/deb-systemd-helper-enabled/multi-user.target.wants/firebird3.0.service
sudo rm -rf /var/lib/firebird*
sudo rm -rf /var/lib/dpkg/info/firebird*
sudo rm -rf /etc/firebird/3.0/
sudo rm -rf /var/log/firebird/firebird3.0.log
sudo rm -rf /usr/bin/isql-fb
sudo rm -rf /var/lib/firebird/3.0/system/security3.fdb
sudo rm -rf /etc/systemd/system/firebird3.0.service
sudo rm -rf /var/crash/firebird3.0-server.0.crash
sudo rm -rf /var/lib/systemd/deb-systemd-helper-masked/firebird3.0.service

sleep 2

echo "Removing user - firebird"
sudo userdel firebird
cd /home/ubuntu-vbox

sleep 2

echo "DONE"
exit
</pre>

### Almalinux 8.10

###### Firebird Installing

- System Update

<pre>
sudo yum update
sudo dnf update
</pre>

- Prepare installation

<pre>
sudo vi /etc/sysctl.conf
vm.max_map_count = 256000

sudo sysctl -p /etc/sysctl.conf
</pre>

<pre>
sudo dnf install epel-release
sudo dnf install ncurses-compat-libs libicu libtommath
sudo dnf install curl tar
</pre>

- Install the firebird

<pre>
cd /home/almalinux-vbox

#Deprecated
###wget https://github.com/FirebirdSQL/firebird/releases/download/v3.0.11/Firebird-3.0.11.33703-0.i686.tar.gz
###tar -xvf Firebird-3.0.11.33703-0.i686.tar.gz
###cd Firebird-3.0.11.33703-0.i686
###sudo ./install.sh

#From Fedora Repository
sudo yum install firebird
</pre>

- Activate the firebird service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable firebird.service
sudo systemctl start firebird.service
sudo systemctl status firebird.service
</pre>

- Allow firewall firebird port

<pre>
sudo firewall-cmd --add-port=3050/tcp --permanent
sudo firewall-cmd --reload
</pre>

- Create a database folder

<pre>
sudo mkdir -p /etc/firebird/databases
sudo chmod 777 /etc/firebird/databases
</pre>

- Connect via ISQL Tool

<pre>
sudo isql-fb -u sysdba -p masterkey
SQL> CREATE DATABASE '/etc/firebird/databases/dbname1.fdb';
SQL> CREATE TABLE users (id INT NOT NULL PRIMARY KEY,name VARCHAR(250) NOT NULL);
SQL> INSERT INTO users (id, name) VALUES ('123456', 'John Smith Wiz');
SQL> COMMIT;
SQL> SELECT * FROM users;
SQL> QUIT;
</pre>

<pre>
sudo chmod 777 /etc/firebird/databases/dbname1.fdb
</pre>

- Create a user to manager database remotely

<pre>
sudo isql-fb -u sysdba -p masterkey
Use CONNECT or CREATE DATABASE to specify a database
SQL> CONNECT DATABASE '/etc/firebird/databases/dbname1.fdb';
SQL> CONNECT '/etc/firebird/databases/dbname1.fdb';
Database: '/etc/firebird/databases/dbname1.fdb', User: SYSDBA
SQL> CREATE USER devel password '123change';
SQL> commit;
SQL> quit;
</pre>

- Change firebird configurations

<pre>
sudo vi /etc/firebird/firebird.conf
</pre>

<pre>
RemoteAccess = true
RemoteServicePort = 3050
AuthServer = Srp
AuthClient = Srp, Srp256, Legacy_Auth #Non Windows clients
UserManager = Srp
WireCrypt = Enabled
WireCompression = false
RemoteBindAddress =
</pre>

- Restart firebird service

<pre>
sudo systemctl stop firebird.service
sudo systemctl start firebird.service
sudo systemctl status firebird.service
</pre>

- Check the firebird files installation

<pre>
#Configuration File
sudo ls -ltr /etc/firebird/

#Log File
sudo ls -ltr /var/log/firebird/firebird.log

#ISQL Tool
sudo ls -ltr /usr/bin/isql-fb
</pre>

- Connect via DBMS Manager (remotely: just for testing)

<pre>
host: 192.168.0.241
port: 3050
path: /etc/firebird/databases/dbname1.fdb
username: devel
password: 123change
jdbc-url: jdbc:firebirdsql://192.168.0.241:3050/etc/firebird/databases/dbname1.fdb
Reference library file: Jaybird-3.0.12-JDK_1.8.zip (https://firebirdsql.org/en/jdbc-driver/)
  jaybird-3.0.12.jar
  connector-api-1.5.jar
  antlr-runtime-4.7.jar
</pre>

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 3050"
sudo firewall-cmd --zone=public --remove-port=3050/tcp
sudo firewall-cmd --runtime-to-permanent 
sudo firewall-cmd --reload

sleep 2

echo "Stopping and removing service - firebird.service"
sudo systemctl stop firebird.service
sudo service firebird stop
sudo rm -f /etc/systemd/system/firebird.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/firebird.service
sudo rm -f /etc/init.d/firebird
sudo systemctl daemon-reload

sleep 2

echo "Removing firebird, folders and links"
sudo yum remove firebird -y
sudo yum autoremove -y
sudo rm -rf /usr/sbin/firebird /usr/lib64/firebird /etc/firebird /usr/share/firebird
sudo rm -rf /etc/logrotate.d/firebird
sudo rm -rf /var/lib/firebird*
sudo rm -rf /var/log/firebird*
sudo rm -rf /usr/sbin/firebird
sudo rm -rf /usr/lib/systemd/system/firebird.service
sudo rm -rf /usr/lib64/firebird*
sudo rm -rf /usr/share/doc/firebird*

sleep 2

echo "Removing user - firebird"
sudo userdel firebird
cd /home/almalinux-vbox

sleep 2

echo "DONE"
exit
</pre>



