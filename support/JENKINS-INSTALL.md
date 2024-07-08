
# JENKINS INSTALL

> The current version is 2.464 and requires Java 17 on Ubuntu 8.10

> The current version is 2.452.2 and requires Java 17 on Almalinux 8.10

### Ubuntu 20.04

###### Jenkins Installing

- System Update

<pre>
sudo apt-get update
</pre>

- Prepare environment

<pre>
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
    https://pkg.jenkins.io/debian/jenkins.io-2023.key

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
</pre>

- Install OpenJDK 17 (required)

> TIP: Give a look int the 
> <a href="https://github.com/huntercodexs/help4devs/blob/commons/support/JAVA-INSTALL.md">
> JAVA INSTALL
> </a> 
> to install more than one java version or manually installation

<pre>
sudo apt-get install openjdk-17-jre
</pre>

- Install Jenkins

<pre>
sudo apt-get install jenkins
</pre>

> TIP: If Jenkins fails to start because a port is in use, run systemctl edit jenkins and add 
> the following:

<pre>
sudo systemctl edit jenkins
</pre>

<pre>
[Service]
Environment="JENKINS_PORT=8080"
</pre>

- Allow firewall jenkins port

<pre>
sudo ufw allow 8080/tcp
sudo ufw reload
</pre>

- Enable Jenkins service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable jenkins
</pre>

- Manage Jenkins service

<pre>
sudo systemctl stop jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
</pre>

- Access Jenkins

<pre>
http://${IP-JENKINS}:8080

##Get the password from console
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
2f7e9402b77b4aabb2cd745b1930ea37
</pre>

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 8080"
sudo ufw delete allow 8080/tcp
sudo ufw reload

sleep 2

echo "Stopping and removing service - jenkins.service"
sudo systemctl stop jenkins.service
sudo service jenkins stop
sudo rm -f /etc/systemd/system/jenkins.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/jenkins.service
sudo rm -f /etc/init.d/jenkins
sudo systemctl daemon-reload

sleep 2

echo "Removing jenkins, folders and links"
sudo apt-get remove jenkins -y
sudo apt-get remove --auto-remove -y
cd /
sudo find . | grep -v "\/home\/" | grep jenkins | xargs -i sudo rm -rf {}

sleep 2

echo ""
echo -n "Removing Java installation ? Continue[Y], Skip[N]: "
read RMJAVA

if [[ "${RMJAVA}" == "Y" ]]; 
then 
    sudo apt-get remove -y openjdk*
    cd /
    sudo find . | egrep -v "\/usr\/share\/jdk" | egrep -v "\/home|nexus|sonatype\/" | grep jdk | egrep "java|jvm|openjdk" | xargs -i sudo rm -rf {}
    export JAVA_HOME=
    unset JAVA_HOME
else
    echo "OK - Skipping"
fi
echo ""

sleep 2

echo "Removing user - jenkins"
sudo userdel jenkins
cd /home/ubuntu-vbox

sleep 2

echo "DONE"
exit
</pre>

### Almalinux 8.10

###### Jenkins Installing

- System Update

<pre>
sudo yum update
sudo dnf update
</pre>

- Prepare environment

<pre>
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

sudo yum upgrade
sudo yum install fontconfig
</pre>

- Install OpenJDK 17 (required)

> TIP: Give a look int the
> <a href="https://github.com/huntercodexs/help4devs/blob/commons/support/JAVA-INSTALL.md">
> JAVA INSTALL
> </a>
> to install more than one java version or manually installation

<pre>
sudo yum install java-17-openjdk
</pre>

- Install Jenkins

<pre>
sudo yum install jenkins
</pre>

> TIP: If Jenkins fails to start because a port is in use, run systemctl edit jenkins and add
> the following:

<pre>
sudo systemctl edit jenkins
</pre>

<pre>
[Service]
Environment="JENKINS_PORT=8080"
</pre>

- Allow firewall jenkins port

<pre>
sudo firewall-cmd --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
</pre>

- Enable Jenkins service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable jenkins
</pre>

- Manage Jenkins service

<pre>
sudo systemctl stop jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
</pre>

- Access Jenkins

<pre>
http://${IP-JENKINS}:8080

##Get the password from console
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
ecd4c01fbaee4070a3e2017ddfe74a71
</pre>

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 8080"
sudo firewall-cmd --zone=public --remove-port=8080/tcp
sudo firewall-cmd --runtime-to-permanent 
sudo firewall-cmd --reload

sleep 2

echo "Stopping and removing service - jenkins.service"
sudo systemctl stop jenkins.service
sudo service jenkins stop
sudo rm -f /etc/systemd/system/jenkins.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/jenkins.service
sudo rm -f /etc/init.d/jenkins
sudo systemctl daemon-reload

sleep 2

echo "Removing jenkins, folders and links"
sudo yum remove jenkins -y
sudo yum autoremove -y
cd /
sudo find . | grep -v "\/home\/" | grep jenkins | xargs -i sudo rm -rf {}

sleep 2

echo ""
echo -n "Removing Java installation ? Continue[Y], Skip[N]: "
read RMJAVA

if [[ "${RMJAVA}" == "Y" ]]; 
then 
    sudo yum remove -y openjdk*
    cd /
    sudo find . | egrep -v "\/usr\/share\/jdk" | egrep -v "\/home|nexus|sonatype\/" | grep jdk | egrep "java|jvm|openjdk" | xargs -i sudo rm -rf {}
    export JAVA_HOME=
    unset JAVA_HOME
else
    echo "OK - Skipping"
fi
echo ""

sleep 2

echo "Removing user - jenkins"
sudo userdel jenkins
cd /home/almalinux-vbox

sleep 2

echo "DONE"
exit
</pre>
