
# PROMETHEUS INSTALL

> NOTE: The current prometheus version is 2.43.0 

### Ubuntu 20.04 Installing
	
###### Installing 

> Source: https://www.cherryservers.com/blog/install-prometheus-ubuntu

- System Update Packages

<pre>
sudo apt update
</pre>

- Create a System User for Prometheus

<pre>
sudo groupadd --system prometheus
sudo useradd -s /sbin/nologin --system -g prometheus prometheus
</pre>

- Create Directories for Prometheus

<pre>
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
</pre>

- Download Prometheus and Extract Files

<pre>
wget https://github.com/prometheus/prometheus/releases/download/v2.43.0/prometheus-2.43.0.linux-amd64.tar.gz
tar vxf prometheus*.tar.gz
</pre>

- Navigate to the Prometheus Directory

<pre>
cd prometheus*/
ls -ltr
</pre>

- Move the Binary Files & Set Owner

<pre>
sudo mv prometheus /usr/local/bin
sudo mv promtool /usr/local/bin
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
</pre>

- Move the Configuration Files & Set Owner

<pre>
sudo mv consoles /etc/prometheus
sudo mv console_libraries /etc/prometheus
sudo mv prometheus.yml /etc/prometheus

sudo chown prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
sudo chown -R prometheus:prometheus /var/lib/prometheus
</pre>

- Set the prometheus file configuration

<pre>
sudo vi /etc/prometheus/prometheus.yml
</pre>

- Create Prometheus Systemd Service

<pre>
sudo vi /etc/systemd/system/prometheus.service
</pre>

<pre>
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
</pre>

- Handle the prometheus service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
sudo systemctl status prometheus
</pre>

- Allow firewall prometheus port

<pre>
sudo ufw allow 9090/tcp
sudo ufw reload
</pre>

- Access the prometheus dashboard

<pre>
http://${IP-PROMETHEUS}:9090
</pre>

###### Uninstalling from Ubuntu

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 9090"
sudo ufw delete allow 9090/tcp
sudo ufw reload

sleep 2

echo "Stopping and removing service - prometheus.service"
sudo systemctl stop prometheus.service
sudo service prometheus stop
sudo rm -f /etc/systemd/system/prometheus.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/prometheus.service
sudo rm -f /etc/init.d/prometheus
sudo systemctl daemon-reload

sleep 2

echo "Removing folders and links"
sudo rm -rf /etc/systemd/system/prometheus.service
sudo rm -rf /usr/local/bin/prometheus
sudo rm -rf /etc/init.d/prometheus
sudo rm -rf /usr/local/bin/prometheus
sudo rm -rf /usr/local/bin/promtool
sudo rm -rf /etc/prometheus
sudo rm -rf /var/lib/prometheus

sleep 2

echo "Removing user - prometheus"
sudo userdel prometheus
sudo rm -rf /home/prometheus
sudo rm -rf /var/spool/mail/prometheus
cd /home/ubuntu-vbox

sleep 2

echo "DONE"
exit
</pre>

### Almalinux 8.10 Installing from

> Source: https://idroot.us/install-prometheus-almalinux-8/

- System Update.

<pre>
sudo dnf clean all
sudo dnf update
</pre>

- Create system user and group for Prometheus.

<pre>
sudo adduser -M -r -s /sbin/nologin prometheus
</pre>

- Create directory

<pre>
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
</pre>

- Download Prometheus and Extract Files

<pre>
wget https://github.com/prometheus/prometheus/releases/download/v2.43.0/prometheus-2.43.0.linux-amd64.tar.gz
tar vxf prometheus*.tar.gz
</pre>

- Navigate to the Prometheus Directory

<pre>
cd prometheus*/
ls -ltr
</pre>

- Move the Binary Files & Set Owner

<pre>
sudo mv prometheus /usr/local/bin
sudo mv promtool /usr/local/bin
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
</pre>

- Move the Configuration Files & Set Owner

<pre>
sudo mv consoles /etc/prometheus
sudo mv console_libraries /etc/prometheus
sudo mv prometheus.yml /etc/prometheus

sudo chown prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
sudo chown -R prometheus:prometheus /var/lib/prometheus
</pre>

- Set up the prometheus file configuration

<pre>
sudo vi /etc/prometheus/prometheus.yml
</pre>

- Create Prometheus Systemd Service

<pre>
sudo vi /etc/systemd/system/prometheus.service
</pre>

<pre>
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
</pre>

- Activate prometheus in the SELinux

<pre>
sudo systemctl enable --now prometheus
sudo chcon -R -t bin_t /usr/local/bin/prometheus
sudo ln -s /usr/local/bin/prometheus /etc/init.d/prometheus
cd /etc/init.d
sudo chkconfig --add prometheus
sudo chkconfig --levels 345 prometheus on
</pre>

- Handle the prometheus service

<pre>
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
sudo systemctl status prometheus
</pre>

- Allow firewall prometheus port

<pre>
sudo firewall-cmd --add-port=9090/tcp --permanent
sudo firewall-cmd --reload
</pre>

- Access the prometheus dashboard

<pre>
http://${IP-PROMETHEUS}:9090
</pre>

###### Uninstalling from Almalinux

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 9090"
sudo firewall-cmd --zone=public --remove-port=9090/tcp
sudo firewall-cmd --runtime-to-permanent 
sudo firewall-cmd --reload

sleep 2

echo "Stopping and removing service - prometheus.service"
sudo systemctl stop prometheus.service
sudo service prometheus stop
sudo rm -f /etc/systemd/system/prometheus.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/prometheus.service
sudo rm -f /etc/init.d/prometheus
sudo systemctl daemon-reload

sleep 2

echo "Removing folders and links"
sudo rm -rf /etc/systemd/system/prometheus.service
sudo rm -rf /usr/local/bin/prometheus
sudo rm -rf /etc/init.d/prometheus
sudo rm -rf /usr/local/bin/prometheus
sudo rm -rf /usr/local/bin/promtool
sudo rm -rf /etc/prometheus
sudo rm -rf /var/lib/prometheus

sleep 2

echo "Removing user - prometheus"
sudo userdel prometheus
sudo rm -rf /home/prometheus
sudo rm -rf /var/spool/mail/prometheus
cd /home/almalinux-vbox

sleep 2

echo "DONE"
exit
</pre>


