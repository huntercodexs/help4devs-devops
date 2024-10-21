
# TOMCAT HELPER

> The version used in this document for Tomcat is xxx

### Ubuntu 20.04

###### Installing

- System Update

<pre>
sudo apt update
sudo apt-get update
</pre>

- Install OpenJDK

> TIP: Give a look int the
> <a href="https://github.com/huntercodexs/help4devs/blob/commons/support/JAVA-INSTALL.md">
> JAVA INSTALL
> </a>
> to install more than one java version or manually installation

<pre>
sudo sudo apt install default-jdk
</pre>

- Create tomcat user

<pre>
sudo useradd -m -d /opt/tomcat -U -s /bin/false tomcat
</pre>

- Get download tomcat (Binary File)

<pre>
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.25/bin/apache-tomcat-10.1.25.tar.gz
</pre>

<pre>
sudo tar -xvf apache-tomcat-10*tar.gz -C /opt/tomcat --strip-components=1

sudo chown -R tomcat:tomcat /opt/tomcat/
sudo chmod -R u+x /opt/tomcat/bin
</pre>

- Configure Admin Users

<pre>
sudo vi /opt/tomcat/conf/tomcat-users.xml
</pre>

<code>

    <role rolename="manager-gui" />
    <user username="manager" password="manager_password" roles="manager-gui" />
    
    <role rolename="admin-gui" />
    <user username="admin" password="admin_password" roles="manager-gui,admin-gui" />

</code>

- Edit restriction manager

<pre>
sudo vi /opt/tomcat/webapps/manager/META-INF/context.xml
</pre>

<code>

    <Context antiResourceLocking="false" privileged="true" >
        <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
    sameSiteCookies="strict" />
    <!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
             allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
        <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.Csr>
    </Context>

</code>

<pre>
sudo vi /opt/tomcat/webapps/host-manager/META-INF/context.xml
</pre>

<code>

    <Context antiResourceLocking="false" privileged="true" >
        <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
                       sameSiteCookies="strict" />
    <!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
             allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
        <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.Csr>
    </Context>

</code>

- Create a tomcat service

<pre>
sudo vi /etc/systemd/system/tomcat.service
</pre>

<pre>
[Unit]
Description=Tomcat
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/share/jdk/jdk-11.0.1"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
</pre>

- Create tomcat path logs

<pre>
sudo mkdir -p /opt/tomcat/logs/
sudo chmod 777 /opt/tomcat/logs/
sudo chown tomcat:tomcat /opt/tomcat/logs/ -R

sudo mkdir -p /opt/tomcat/temp/
sudo chmod 777 /opt/tomcat/temp/
sudo chown tomcat:tomcat /opt/tomcat/temp/ -R
</pre>

- Allow firewall tomcat port

<pre>
sudo ufw allow 8080/tcp
sudo ufw reload
</pre>

- Manage Tomcat Service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable tomcat
sudo systemctl stop tomcat
sudo systemctl start tomcat
sudo systemctl status tomcat
</pre>

- Access the tomcat interface

<pre>
http://${IP-TOMCAT}:8080
</pre>

###### Uninstalling

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

echo "Stopping and removing service - tomcat.service"
sudo systemctl stop tomcat.service
sudo service tomcat stop
sudo rm -f /etc/systemd/system/tomcat.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/tomcat.service
sudo systemctl daemon-reload

sleep 2

echo "Removing tomcat, folders and links"
sudo apt remove tomcat* tomcat-contrib* -y
sudo apt remove wget ca-certificates -y
sudo apt-get remove tomcat* -y
sudo apt-get remove --auto-remove -y
sudo dpkg --purge tomcat

cd /
sudo find . | grep -v home | grep tomcat | xargs -i sudo rm -rf {} >> /dev/null 2>&1

sleep 2

echo "Removing user - tomcat"
sudo userdel tomcat
cd /home/ubuntu-vbox

sleep 2

echo "DONE"
exit
</pre>

### Almalinux 20.04

###### Installing

- System Update

<pre>
sudo yum update
sudo dnf update
</pre>

- Install OpenJDK

> TIP: Give a look int the
> <a href="https://github.com/huntercodexs/help4devs/blob/commons/support/JAVA-INSTALL.md">
> JAVA INSTALL
> </a>
> to install more than one java version or manually installation

<pre>
sudo sudo apt install default-jdk
</pre>

- Create tomcat user

<pre>
sudo useradd -m -d /opt/tomcat -U -s /bin/false tomcat
</pre>

- Get download tomcat (Binary File)

<pre>
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.25/bin/apache-tomcat-10.1.25.tar.gz
</pre>

<pre>
sudo tar -xvf apache-tomcat-10*tar.gz -C /opt/tomcat --strip-components=1

sudo chown -R tomcat:tomcat /opt/tomcat/
sudo chmod -R u+x /opt/tomcat/bin
</pre>

- Configure Admin Users

<pre>
sudo vi /opt/tomcat/conf/tomcat-users.xml
</pre>

<code>

    <role rolename="manager-gui" />
    <user username="manager" password="manager_password" roles="manager-gui" />
    
    <role rolename="admin-gui" />
    <user username="admin" password="admin_password" roles="manager-gui,admin-gui" />

</code>

- Edit restriction manager

<pre>
sudo vi /opt/tomcat/webapps/manager/META-INF/context.xml
</pre>

<code>

    <Context antiResourceLocking="false" privileged="true" >
        <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
    sameSiteCookies="strict" />
    <!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
             allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
        <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.Csr>
    </Context>

</code>

<pre>
sudo vi /opt/tomcat/webapps/host-manager/META-INF/context.xml
</pre>

<code>

    <Context antiResourceLocking="false" privileged="true" >
        <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
                       sameSiteCookies="strict" />
    <!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
             allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
        <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.Csr>
    </Context>

</code>

- Create a tomcat service

<pre>
sudo vi /etc/systemd/system/tomcat.service
</pre>

<pre>
[Unit]
Description=Tomcat
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/share/jdk/jdk-11.0.1"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
</pre>

- Create tomcat path logs

<pre>
sudo mkdir -p /opt/tomcat/logs/
sudo chmod 777 /opt/tomcat/logs/
sudo chown tomcat:tomcat /opt/tomcat/logs/ -R

sudo mkdir -p /opt/tomcat/temp/
sudo chmod 777 /opt/tomcat/temp/
sudo chown tomcat:tomcat /opt/tomcat/temp/ -R
</pre>

- Allow firewall tomcat port

<pre>
sudo firewall-cmd --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
</pre>

- Manage Tomcat Service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable tomcat
sudo systemctl stop tomcat
sudo systemctl start tomcat
sudo systemctl status tomcat
</pre>

- Access the tomcat interface

<pre>
http://${IP-TOMCAT}:8080
</pre>

###### Uninstalling

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

echo "Stopping and removing service - tomcat.service"
sudo systemctl stop tomcat.service
sudo service tomcat stop
sudo rm -f /etc/systemd/system/tomcat.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/tomcat.service
sudo systemctl daemon-reload

sleep 2

echo "Removing tomcat, folders and links"
sudo yum remove tomcat* -y
sudo dnf remove tomcat* -y
sudo yum autoremove -y

cd /
sudo find . | grep -v home | grep tomcat | xargs -i sudo rm -rf {} >> /dev/null 2>&1

sleep 2

echo "Removing user - tomcat"
sudo userdel tomcat
cd /home/almalinux-vbox

sleep 2

echo "DONE"
exit
</pre>

> IMPORTANT: In some cases it will be necessary to restart the machine

### EXTRAS

###### Activate Rewrite Rules

> Tomcat 8.5.41 or later

When we have a application that needs to rewrite url, like Vue or Angular, we need to activate 
the rewrite rules in the tomcat server, so use the following steps to make it

- Go to tomcat path directory
  - /opt/tomcat or /etc/tomcat
- Open the server.xml file and put the content in it

<code>

    <Valve className="org.apache.catalina.valves.rewrite.RewriteValve" />

</code>

This content must be put in the Host section at the end of the server.xml file, for example:

<code>
    
    <Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
        
        <Valve className="org.apache.catalina.valves.rewrite.RewriteValve" />
    
        <!-- SingleSignOn valve, share authentication between web applications Documentation at: /docs/config/valve.html -->
        <!--
        <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
        -->
        
        <!-- Access log processes all example.
         Documentation at: /docs/config/valve.html
         Note: The pattern used is equivalent to using pattern="common" -->
        
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
           prefix="localhost_access_log" suffix=".txt"
           pattern="%h %l %u %t &quot;%r&quot; %s %b" />
    
    </Host>
    
</code>

Also, you need to create a configuration file named rewrite.config in the catalina configuration path as showed below:

<pre>
sudo vi /opt/tomcat/conf/Catalina/localhost/rewrite.config
</pre>

Create a content similar like below

<pre>
RewriteCond %{REQUEST_PATH} !-f
RewriteRule ^/application-name1/(.*) /application-name1/index.html
RewriteCond %{REQUEST_PATH} !-f
RewriteRule ^/application-name2/(.*) /application-name2/index.html
RewriteCond %{REQUEST_PATH} !-f
RewriteRule ^/application-name3/(.*) /application-name3/index.html
</pre>

And the application needs to be put in the following path

<pre>
/opt/tomcat/webapps/application-name1
/opt/tomcat/webapps/application-name2
/opt/tomcat/webapps/application-name3
</pre>

