
# SAMBA SERVER

###### Installing Samba Server
	
- Ubuntu Installing

<pre>
sudo apt install samba
</pre>

The command above should be created one folder in the /etc/samba/ path called smb.conf.

- Make a copy/backup the file

<pre>
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bkp
</pre>

- Edit the smb.conf file

<pre>
sudo vi /etc/samba/smb.conf
</pre>

Below are some explanation and sample to put in this file
<pre>
[global]
#### workgroup name
workgroup = myhouse
#### machine name to show in the workgroup network windows
netbios name = LinuxServerName
#### Authentication
#### Access mode to linux files access: [share=no-authentication, user=authentication]
security = share
</pre>

Add these line in the same file (smb.conf) at the end of the file

<pre>
#### sharing name
[files]
#### sharing description
comment = shared files
#### linux path (directory that will be shared)
path = /media/${device}
#### allow for alls [yes, no]
public = yes
#### allow visibility in the network [yes, no]
browseable = yes
#### allow writing [yes, no]
writable = yes
#### is it read only ? [yes, no]
read only = no
#### default mask permission to create files (see more in the chmod command)
create mask = 0700
#### default mask permission to create directories
directory mask = 0700
</pre>

Save the samba file configuration and restart the service

<pre>
sudo /etc/init.d/samba restart
</pre>

In case you have had defined the security field as "user" so you need to make some things more

- Create user

<pre>
sudo adduser test
</pre>

- Add the user test in the samba users

<pre>
sudo smbpasswd -a test
</pre>

### Example

<pre>
[global] workgroup = HOME
netbios name = LINUX
server string = SAMBA-SRV
security = share
[Files] comment = Shared_Files
path = /home/user-samba-srv/Files
read only = no
public = yes
writable = yes
create mask = 0777
directory mask = 0777
browseable = yes
</pre>

Make sure that the shared folder has the enough permission for anyone read, write and change the information

<pre>
sudo chmod -R o+rwx /home/user-samba-srv/Files
</pre>


