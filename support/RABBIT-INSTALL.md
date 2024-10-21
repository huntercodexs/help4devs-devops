
# RABBITMQ INSTALL

> NOTE:  RabbitMQ 3.13.3, Erlang 26.2.5

### Ubuntu 20.04 Installing

###### Installing 

> Source: https://www.cherryservers.com/blog/how-to-install-and-start-using-rabbitmq-on-ubuntu-22-04

- System Update

<pre>
sudo apt-get update
</pre>

- Installing RabbitMQ

<pre>
sudo apt-get -y install socat logrotate init-system-helpers adduser
sudo apt-get -y install wget
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.13.3/rabbitmq-server_3.13.3-1_all.deb
sudo apt install rabbitmq-server
### If any error occurs try fix it and try again
sudo apt --fix-broken install
sudo apt install rabbitmq-server
</pre>

- Enabling rabbitmq service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server
sudo systemctl status rabbitmq-server
sudo systemctl stop rabbitmq-server
</pre>

- Enabling plugin management

<pre>
sudo rabbitmq-plugins enable rabbitmq_management
</pre>

- Adding a user: test

<pre>
sudo rabbitmqctl add_user test test
sudo rabbitmqctl set_user_tags test administrator
sudo rabbitmqctl set_permissions -p / test ".*" ".*" ".*"
sudo rabbitmqctl list_users
sudo rabbitmqctl list_connections
sudo rabbitmqctl list_permissions
sudo rabbitmqctl authenticate_user test test
</pre>

- Allow firewall rabbitmq port

<pre>
sudo ufw allow 5672/tcp
sudo ufw allow 15672/tcp
sudo ufw reload
</pre>

- Access the RabbitMQ dashboard

<pre>
http://192.168.0.26:15672/
username: test
password: test
</pre>

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 5672, 15672"
sudo ufw delete allow 5672/tcp
sudo ufw delete allow 15672/tcp
sudo ufw reload

sleep 2

echo "Stopping and removing service - rabbitmq-server.service"
sudo systemctl disable rabbitmq-server
sudo systemctl stop rabbitmq-server
sudo rm -f /usr/lib/systemd/system/rabbitmq-server.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/rabbitmq-server.service
sudo systemctl daemon-reload

sleep 2

echo "Removing folders and links"
sudo rm -rf /usr/bin/rabbitmqadmin
sudo rm -rf /usr/lib/rabbitmq
sudo rm -rf /usr/lib/ocf/resource.d/rabbitmq
sudo rm -rf /usr/sbin/rabbitmq*
sudo rm -rf /usr/share/doc/rabbitmq*
sudo rm -rf /usr/share/man/man5/rabbitmq*
sudo rm -rf /usr/share/rabbitmq*
sudo rm -rf /sys/fs/cgroup/memory/system.slice/rabbitmq*
sudo rm -rf /sys/fs/cgroup/devices/system.slice/rabbitmq*
sudo rm -rf /sys/fs/cgroup/pids/system.slice/rabbitmq*
sudo rm -rf /sys/fs/cgroup/memory/system.slice/rabbitmq*
sudo rm -rf /sys/fs/cgroup/systemd/system.slice/rabbitmq*
sudo rm -rf /sys/fs/cgroup/unified/system.slice/rabbitmq*
sudo rm -rf /run/systemd/units/invocation:rabbitmq*
sudo rm -rf etc/rc3.d/S01rabbitmq*
sudo rm -rf /etc/rc2.d/S01rabbitmq*
sudo rm -rf /etc/rc6.d/K01rabbitmq*
sudo rm -rf /etc/rc0.d/K01rabbitmq*
sudo rm -rf /etc/rc1.d/K01rabbitmq*
sudo rm -rf /etc/rabbitmq
sudo rm -rf /etc/logrotate.d/rabbitmq-server
sudo rm -rf /etc/init.d/rabbitmq-server
sudo rm -rf /etc/profile.d/rabbitmqctl-autocomplete.sh.dpkg-new
sudo rm -rf /etc/rc5.d/S01rabbitmq-server
sudo rm -rf /etc/default/rabbitmq-server
sudo rm -rf /etc/rc4.d/S01rabbitmq-server
sudo rm -rf /var/log/rabbitmq
sudo rm -rf /var/lib/systemd/deb-systemd-helper-enabled/multi-user.target.wants/rabbitmq-server.service
sudo rm -rf /var/lib/systemd/deb-systemd-helper-enabled/rabbitmq-server.service.dsh-also
sudo rm -rf /var/lib/rabbitmq
sudo rm -rf /var/lib/dpkg/info/rabbitmq*

sleep 2

echo "Removing rabbitmq"
sudo apt-get remove --auto-remove rabbitmq-server -y
sudo apt-get purge --auto-remove rabbitmq-server -y
sudo userdel rabbitmq

sleep 2

echo "DONE"
exit
</pre>

### Almalinux 8.10 Installing

###### Installing

> Source: https://www.rabbitmq.com/docs/install-rpm

- System Update

<pre>
sudo dnf update -y
</pre>

- Installing RabbitMQ

<pre>
sudo dnf install -y socat logrotate
curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash
curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
sudo dnf install -y erlang rabbitmq-server
</pre>

- Enabling rabbitmq service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server
sudo systemctl status rabbitmq-server
sudo systemctl stop rabbitmq-server
</pre>

- Enabling plugin management

<pre>
sudo rabbitmq-plugins enable rabbitmq_management
</pre>

- Allow firewall rabbitmq port

<pre>
sudo firewall-cmd --add-port=5672/tcp --permanent
sudo firewall-cmd --add-port=15672/tcp --permanent
sudo firewall-cmd --reload
</pre>

- Adding a user: test

<pre>
sudo rabbitmqctl add_user test test
sudo rabbitmqctl set_user_tags test administrator
sudo rabbitmqctl set_permissions -p / test ".*" ".*" ".*"
sudo rabbitmqctl list_users
sudo rabbitmqctl list_connections
sudo rabbitmqctl list_permissions
sudo rabbitmqctl authenticate_user test test
</pre>

- Access the RabbitMQ dashboard

<pre>
http://192.168.0.241:15672/
username: test
password: test
</pre>

###### Uninstalling from Almalinux

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 5672, 15672"
sudo firewall-cmd --zone=public --remove-port=5672/tcp
sudo firewall-cmd --zone=public --remove-port=15672/tcp
sudo firewall-cmd --runtime-to-permanent 
sudo firewall-cmd --reload

sleep 2

echo "Stopping and removing service - rabbitmq-server.service"
sudo systemctl disable rabbitmq-server
sudo systemctl stop rabbitmq-server
sudo rm -f /usr/lib/systemd/system/rabbitmq-server.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/rabbitmq-server.service
sudo systemctl daemon-reload

sleep 2

echo "Removing folders and links"
sudo rm -rf /usr/share/selinux/targeted/default/active/modules/100/rabbitmq
sudo rm -rf /usr/share/man/man5/rabbitmq-*
sudo rm -rf /usr/share/doc/rabbitmq-server-*
sudo rm -rf /usr/lib/rabbitmq/
sudo rm -rf /etc/rabbitmq
sudo rm -rf /var/lib/selinux/targeted/active/modules/100/rabbitmq
sudo rm -rf /etc/logrotate.d/rabbitmq-server
sudo rm -rf /etc/yum.repos.d/rabbitmq_erlang.repo
sudo rm -rf /etc/yum.repos.d/rabbitmq_rabbitmq-server.repo
sudo rm -rf /etc/profile.d/rabbitmqctl-autocomplete.sh
sudo rm -rf /sys/fs/cgroup/memory/system.slice/rabbitmq*
sudo rm -rf /run/systemd/units/invocation:rabbitmq*
sudo rm -rf /run/rabbitmq

sleep 2

echo "Removing rabbitmq"
sudo dnf remove -y rabbitmq-server
sudo dnf autoremove

sleep 2

echo "DONE"
exit
</pre>
