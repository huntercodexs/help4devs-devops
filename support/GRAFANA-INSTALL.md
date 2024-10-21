
# GRAFANA INSTALL

> NOTE: The current version of Grafana is v11.0.0

### Ubuntu 20.04

###### Installing

> Source: https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/

<pre>
sudo apt-get install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com beta main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana
sudo apt-get install grafana --fix-missing
</pre>

###### Installing from

> Source : https://grafana.com/grafana/download

<pre>
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/enterprise/release/grafana-enterprise_11.0.0_amd64.deb
sudo dpkg -i grafana-enterprise_11.0.0_amd64.deb
</pre>

###### Login and change admin password

<pre>
http://${IP-GRAFANA-SERVER}:3000
</pre>

<pre>
Administration > Users and Access > Users > admin > Password > Edit > Save
</pre>

### Almalinux 8.10

###### Installing

> Source: https://grafana.com/grafana/download

<pre>
sudo yum install -y https://dl.grafana.com/enterprise/release/grafana-enterprise-11.0.0-1.x86_64.rpm
</pre>

###### Allow firewall mysql port

<pre>
sudo firewall-cmd --add-port=3000/tcp --permanent
sudo firewall-cmd --reload
</pre>

###### Login and change admin password

<pre>
http://${IP-GRAFANA-SERVER}:3000
</pre>

<pre>
Administration > Users and Access > Users > admin > Password > Edit > Save
</pre>

### Manage the grafana service

<pre>
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable grafana-server
sudo /bin/systemctl start grafana-server
sudo /bin/systemctl status grafana-server
</pre>

