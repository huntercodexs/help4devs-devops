
# LINUX TIPS

##### USER CREATE

<pre>
sudo /usr/sbin/adduser --user-group --no-create-home --shell=/sbin/nologin --comment='{USERNAME} account' {USERNAME}
</pre>

##### USER DELETE

<pre>
sudo /usr/sbin/userdel {USERNAME}
</pre>

##### CHECK USER

<pre>
cat /etc/passwd
</pre>

##### CHECK GROUPS

<pre>
cat /etc/group
</pre>

##### SSH CONNECT USING PEM

<pre>
sudo ssh -i /home/ec2-user/key/KEY-NAME-AWS.pem ec2-user@10.0.7.160
sudo ssh -i /home/ec2-user/key/KEY-NAME-AWS.pem ubuntu@10.0.X.X
</pre>

##### DISABLE SWAP

<pre>
sudo swapoff -a
</pre>

Check swap in the FSTAB system linux

<pre>
cat /etc/fstab
</pre>

##### FIREWALL

- Ubuntu 20.04

<pre>
#Allow
sudo ufw allow ${PORT_NUMBER}/tcp
sudo ufw reload

#GET APP LIST
sudo apt install nginx

#Delete
sudo ufw delete allow ${PORT_NUMBER}/tcp
sudo ufw reload
</pre>

- Almalinux 8.10

<pre>
#Allow
sudo firewall-cmd --add-port=${PORT_NUMBER}/tcp --permanent
sudo firewall-cmd --reload

#Delete
sudo firewall-cmd --zone=public --remove-port=${PORT_NUMBER}/tcp
sudo firewall-cmd --runtime-to-permanent 
sudo firewall-cmd --reload
</pre>

##### LINUX HARDWARE INFORMATION

- Overall information

<pre>
sudo inxi -Fxz

or

sudo hwinfo
sudo hwinfo --short

or

sudo lshw
sudo lshw -short
</pre>

- CPU Details

<pre>
sudo lscpu

sudo lshw -C cpu

sudo lshw -C cpu | grep -i product
</pre>

- Memory Details

<pre>
sudo dmidecode -t memory
sudo dmidecode -t memory | grep -i size

or
sudo lshw -short -C memory
</pre>

- Memory in use

<pre>
free -m
</pre>

- Watch the process in memory

<pre>
top
</pre>

- Maximum memory possible

<pre>
sudo dmidecode -t memory | grep -i max
</pre>

- Check if there is any slot empty to put a new memory stick

<pre>
sudo lshw -short -C memory | grep -i empty
</pre>

- GPU and VGA details

<pre>
lspci | grep -i vga
lspci -v -s 00:02.0
lspci -v -s 01:00.0
</pre>

- Disks, filesystems, and devices

> Disk details information

<pre>
sudo lshw -short -C disk
</pre>

> LIst disk partitions

<pre>
lsblk

# With more details
sudo fdisk -l
</pre>

> List partition system (UUID)

<pre>
sudo blkid
</pre>

- USB Details

<pre>
sudo lsusb
sudo lspci
</pre>

- Network devices details

<pre>
sudo lshw -C network
</pre>

##### CHECK RELEASE INFORMATION

<pre>
cat /etc/lsb-release
</pre>

<pre>
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04.6 LTS"
</pre>

##### CHECK AVAILABLE PORTS

- sudo netstat
- sudo netstat -ano
- sudo netstat -tunl
- sudo netstat -tulpn
- sudo netstat -ano | grep 443
- sudo netstat -ano | egrep '443|I-Node'
- sudo netstat -na | grep tcp | grep -i listen

##### DNS COMMANDS

- nslookup
- netstat

##### NETWORK INTERFACE (LEGACY)

- ifconfig (net-tools)

##### TRACE A ADDRESS

- traceroute

##### IP ROUTE 2

- ip addr
- ip link
- ip route
- ss

##### KERNEL RECOVERY

- Kernel Recovery

<pre>
When an update is made and updated to the kernel and its header files, it may occur
unexpected problems during the boot process, or even after the boot process during a
system initialization, such as a video problem. To agree, or return to the state
previous stable (which worked) we can follow the following step by step:
</pre>

> NOTE: Before taking any action, make a backup of the files

> NOTE: kernel files loaded during the boot process are located in the /boot directory

> NOTE: You need to know which kernel worked to use in this process

- Check the /etc/default/grub file

> NOTE: You should be careful when changing this file, just change the line:

<pre>
GRUB_DEFAULT=saved
</pre>

Add the line

<pre>
GRUB_SAVEDEFAULT=true
</pre>

- Check the /boot/grub/grub.cfg file

<pre>
In this file, you need to identify the Menu export linux_gfx_mode command line that has
sequences the kernel that will be loaded during the process. After finding out which is the correct kernel
to be loaded, simply change this code block by inserting the desired kernel by placing the kernel
problematic in the background or third plan, as shown in the example below:
</pre>

<pre>
export linux_gfx_mode
menuentry 'Ubuntu' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-simple-f569871c-76ce-4e25-842c-ab8039ead5f4' {
    recordfail
    load_video
    gfxmode $linux_gfx_mode
    insmod gzio
    if [ x$grub_platform = xxen ]; then insmod xzio; insmod lzopio; fi
    insmod part_msdos
    insmod ext2
    if [ x$feature_platform_search_hint = xy ]; then
      search --no-floppy --fs-uuid --set=root  f569871c-76ce-4e25-842c-ab8039ead5f4
    else
      search --no-floppy --fs-uuid --set=root f569871c-76ce-4e25-842c-ab8039ead5f4
    fi
    [[[[ALTERAR A LINHA ABAIXO COM O KERNEL CORRETO]]]]
    linux	/boot/vmlinuz-5.8.0-59-generic root=UUID=f569871c-76ce-4e25-842c-ab8039ead5f4 ro  quiet splash $vt_handoff
    linux	/boot/vmlinuz-5.8.0-42-generic root=UUID=f569871c-76ce-4e25-842c-ab8039ead5f4 ro  quiet splash $vt_handoff << EXEMPLO
    [[[[ALTERAR A LINHA ABAIXO COM O KERNEL CORRETO]]]]
    initrd	/boot/initrd.img-5.8.0-59-generic
    initrd	/boot/initrd.img-5.8.0-42-generic << EXEMPLO
}
</pre>

- Update grub and restart the machine

> NOTE: Before restarting the machine, check if grub has been correctly updated according to the desired kernel.

<pre>
sudo update-grub
reboot
</pre>

<hr />

##### NVIDIA Drivers Install

<pre>
To install the nvidia drivers correctly on a machine that uses Nvidia cards, follow the instructions below:
</pre>

To find out if the driver is already installed, type:

<pre>
lsmod | grep nvidia
</pre>

The command should not return any results

- Check if the nouveau generic driver is in memory:

<pre>
lsmod | grep nouveau
</pre>

The command should return the nouveau drivers loaded into memory

- Find out which driver to use:

<pre>
To do this, visit the NVIDIA DRIVERS website and search for the exact model of the video card, operating system,
branch, and language. After knowing the correct version of the driver to be used, type the following command
in the terminal:
</pre>

<pre>
apt search nvidia
</pre>

<pre>
The command should return a list of drivers, updates and features available for Nvidia Graphics,
let's assume that the driver version in question is nvidia-390, so to install just run the command:
</pre>

<pre>
apt install nvidia-390
</pre>

<pre>
After installation, do not restart the machine, go to the /etc/modprobe.d directory and check if any files
nvidia.conf was created with the name nvidia.conf or similar, check its content and see if the
content is the same as below:
</pre>

<pre>
blacklist nouveau
blacklist lbm-nouveau
alias nouveau off
alias lbm-nouveau off
</pre>

<pre>
If the content is different or the file has not been created, create the file and insert this content into it.
Still in the /etc/modprobe.d directory, run the command:
</pre>

<pre>
grep nvidia *
</pre>

<pre>
and check if there is any nvidia resource in any of the files, as in the case of the blacklist-framebuffer.conf file
which contains the nvidiafb blacklist line, just comment the line as shown below
</pre>

<pre>
#blacklist nvidiafb
</pre>

- Reload INIT RAM FS for all kernels

 Run the command below to recompile and update the kernels with the new driver installed:

<pre>
sudo update-initramfs -u
</pre>

- Restart the machine

<pre>
reboot
</pre>

- Verify that the driver was installed and loaded correctly

<pre>
lsmod | grep nvidia
</pre>

- The command must return the following values, or similar:

<pre>
nvidia_uvm             36864  0
nvidia              10592256  88 nvidia_uvm
nvidiafb               53248  0
vgastate               20480  1 nvidiafb
fb_ddc                 16384  1 nvidiafb
i2c_algo_bit           16384  1 nvidiafb
drm                   552960  6 nvidia
</pre>

##### STARTUP CONFIGURATION PROGRAMS

To configure a bash program or script to "go up" along with the system boot, follow the instructions below:

> NOTE: For demonstration purposes, it will be shown how to boot anydesk, a program for remotely 
> accessing computers on the local network or the internet.

- Anydesk remote access program

After correctly installing and configuring the Anydesk program, create a bash script in the 
directory /etc/init.d with the following command:

<pre>
sudo touch anydesk
</pre>

and add the following content

<pre class="code-style">
#!/bin/bash

/usr/bin/anydesk --tray &
/usr/bin/anydesk &
</pre>

- Symbolic link

Create a link in the /etc/rc2.d directory pointing to the script created in /etc/init.d

<pre>
sudo ln -s /etc/init.d/anydesk /etc/rc2.d/S99anydesk
</pre>

> Note that the link name composition contains S99 + target script name: S99anydesk
> <br />
> Restart the machine with the reboot command
> <br />
> In the case in question, AnyDesk is being tested, so when the system is loaded during the 
> boot process, it will be possible to access the machine even if no login has been performed.

##### CREATE SYSTEMD SERVICES

###### Tomcat example

- Create user

<pre>
sudo useradd -m -d /opt/tomcat -U -s /bin/false tomcat
</pre>

- Create service

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

#Environment="JAVA_HOME=/usr/share/jdk/jdk-11.0.1"
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

- Configure the service logs

<pre>
sudo mkdir -p /opt/tomcat/logs/
sudo chmod 777 /opt/tomcat/logs/
sudo chown tomcat:tomcat /opt/tomcat/logs/ -R

sudo mkdir -p /opt/tomcat/temp/
sudo chmod 777 /opt/tomcat/temp/
sudo chown tomcat:tomcat /opt/tomcat/temp/ -R
</pre>

- Allow service port

<pre>
sudo ufw allow 8080/tcp
sudo ufw reload
</pre>

- Manager service status

<pre>
sudo systemctl daemon-reload
sudo systemctl enable tomcat
sudo systemctl stop tomcat
sudo systemctl start tomcat
sudo systemctl status tomcat
</pre>

###### Java example

- Get the service files

<pre>
{APPLICATION_NAME}.jar
application.properties
log4j.xml
...
</pre>

- Create user

<pre>
sudo useradd -m -d /opt/deployer -U -s /bin/false deployer
</pre>

- Create service

<pre>
sudo vi /etc/systemd/system/{APPLICATION_NAME}.service
</pre>

<pre>
[Unit]
Description={APPLICATION_NAME}.service
After=network-online.target

[Service]
Type=simple
WorkingDirectory=/var/opt/deployer/{APPLICATION_NAME}
ExecStart=/usr/bin/java -jar /opt/deployer/{APPLICATION_NAME}/{APPLICATION_NAME}.jar --spring.config.location=/etc/opt/deployer/{APPLICATION_NAME}/application.properties
Restart=on-abort
User=deployer
Group=deployer

[Install]
WantedBy=multi-user.target
</pre>

- Create service structure

<pre>
APP_DIR
/opt/deployer/{APPLICATION_NAME}/

CONFIG_DIR
/etc/opt/deployer/{APPLICATION_NAME}/

DATA_DIR
/var/opt/deployer/{APPLICATION_NAME}/

LOG_DIR
/var/log/deployer/{APPLICATION_NAME}/

SERVICE_DIR
/usr/lib/systemd/system/
/etc/systemd/system/
</pre>

- Service files

<pre>
/opt/deployer/{APPLICATION_NAME}/{APPLICATION_NAME}.jar
/etc/opt/deployer/{APPLICATION_NAME}/application.properties
/etc/opt/deployer/{APPLICATION_NAME}/log4j2.xml
/usr/lib/systemd/system/{APPLICATION_NAME}.service
</pre>

- Allow service port

<pre>
sudo ufw allow {APPLICATION_PORT}/tcp
sudo ufw reload
</pre>

- Manager service status

<pre>
sudo systemctl daemon-reload
sudo systemctl enable {APPLICATION_NAME}
sudo systemctl stop {APPLICATION_NAME}
sudo systemctl start {APPLICATION_NAME}
sudo systemctl status {APPLICATION_NAME}
</pre>

###### Python example

- Create user

<pre>
sudo useradd -m -d /opt/py_deployer -U -s /bin/false py_deployer
</pre>

- Create service

<pre>
sudo vi /etc/systemd/system/{APPLICATION_NAME}.service
</pre>

<pre>
[Install]
WantedBy=multi-user.target

[Unit]
Description=Gunicorn instance to serve {APPLICATION_NAME}
After=network.target

[Service]
User=py_deployer
Group=www-data
WorkingDirectory=/opt/py_deployer/{APPLICATION_NAME}
Environment="PATH=/opt/py_deployer/{APPLICATION_NAME}/bin"
ExecStart=/opt/py_deployer/{APPLICATION_NAME}/bin/gunicorn --workers 3 --bind unix:{APPLICATION_NAME}.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
</pre>

- Service UWSGI

<pre>
[uwsgi]
module = wsgi:app

master = true
processes = 5

socket = {APPLICATION_NAME}.sock
chmod-socket = 660
vacuum = true

die-on-term = true
logto = /var/log/{APPLICATION_NAME}/{APPLICATION_NAME}.log
</pre>

- Create service structure

<pre>
/var/log/py_deployer/{APPLICATION_NAME}
/opt/py_deployer/{APPLICATION_NAME}
</pre>

- Allow service port

<pre>
sudo ufw allow {APPLICATION_PORT}/tcp
sudo ufw reload
</pre>

- Manager service status

<pre>
sudo systemctl daemon-reload
sudo systemctl enable {APPLICATION_NAME}
sudo systemctl stop {APPLICATION_NAME}
sudo systemctl start {APPLICATION_NAME}
sudo systemctl status {APPLICATION_NAME}
</pre>

