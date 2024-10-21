
# SSH TIPS

###### Installing

- Ubuntu 20.04

<pre>
sudo apt-get install ssh
sudo apt install openssh-server -y
sudo systemctl daemon-reload
sudo systemctl enable ssh
sudo systemctl stop ssh
sudo systemctl start ssh
sudo systemctl status ssh
sudo ufw enable
sudo ufw allow ssh
sudo ufw delete allow ssh (delete ssh entry)
</pre>

###### Create the ssh key RSA

<pre>
ssh-keygen -t rsa
</pre>

###### Permission

<pre>
/home/user => 755
~/.ssh => 700
~/.ssh/id_rsa => 400
~/.ssh/authorized_keys => 400
</pre>

###### Sample Key Generated

<pre>
/home/jereelton/.ssh/
-rw-r--r-- 1 jereelton jereelton  222 Apr 28 23:20 known_hosts
-rw-r--r-- 1 jereelton jereelton  756 Apr 28 23:24 id_rsa.pub
-rw------- 1 jereelton jereelton 3401 Apr 28 23:24 id_rsa
</pre>

###### Copy SSH key to remote server

<pre>
ssh-copy-id jereelton@192.168.0.174
</pre>

###### SSH Access sample command

<pre>
ssh jereelton@192.168.0.174
</pre>

###### SSH Directory and File

<pre>
jereelton@homeserver:~/.ssh$ ls -ltra
total 12
drwxr-xr-x 19 jereelton jereelton 4096 Apr 28 23:24 ..
drwx------  2 jereelton jereelton 4096 Apr 28 23:25 .
-rw-------  1 jereelton jereelton  112 Apr 28 23:25 authorized_keys
</pre>

###### Access Log to ssh connections

<pre>
/var/log/auth.log
</pre>

###### File to configure SSH

<pre>
sudo vi /etc/ssh/ssh_config
</pre>

###### Restart SSH Service

<pre>
sudo systemctl restart ssh
</pre>

