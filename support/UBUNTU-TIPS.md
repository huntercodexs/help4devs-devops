
# UBUNTU TIPS

### Convert Ubuntu Desktop into Ubuntu Server

###### Change the GRUB file as below 

<pre>
sudo vi /etc/default/grub
</pre>

<pre>
# If you change this file, run 'update-grub' afterwards to update
# /boot/grub/grub.cfg.
# For full documentation of the options in this file, see:
#   info -f grub -n 'Simple configuration'

GRUB_DEFAULT=0
GRUB_TIMEOUT=5
#GRUB_HIDDEN_TIMEOUT=0
#GRUB_HIDDEN_TIMEOUT_QUIET=true
GRUB_TIMEOUT_STYLE=5
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
#GRUB_CMDLINE_LINUX_DEFAULT="quiet splash" 
GRUB_CMDLINE_LINUX="text"

# Uncomment to enable BadRAM filtering, modify to suit your needs
# This works with Linux (no patch required) and with any kernel that obtains
# the memory map information from GRUB (GNU Mach, kernel of FreeBSD ...)
#GRUB_BADRAM="0x01234567,0xfefefefe,0x89abcdef,0xefefefef"

# Uncomment to disable graphical terminal (grub-pc only)
GRUB_TERMINAL=console

# The resolution used on graphical terminal
# note that you can use only modes which your graphic card supports via VBE
# you can see them in real GRUB with the command `vbeinfo'
#GRUB_GFXMODE=640x480
GRUB_GFXMODE="1024x768"
GRUB_GFXPAYLOAD_LINUX="1024x768"

# Uncomment if you don't want GRUB to pass "root=UUID=xxx" parameter to Linux
#GRUB_DISABLE_LINUX_UUID=true

# Uncomment to disable generation of recovery mode menu entries
#GRUB_DISABLE_RECOVERY="true"

# Uncomment to get a beep at grub start
#GRUB_INIT_TUNE="480 440 1"
</pre>

###### Update de GRUB configuration 

<pre>
sudo update-grub
sudo echo FRAMEBUFFER=y | sudo tee /etc/initramfs-tools/conf.d/splash
sudo update-initramfs -u
</pre>

###### Convert Ubuntu Desktop to Ubuntu Server

- Install the dependencies below

<pre>
sudo apt-get install tasksel
sudo tasksel remove ubuntu-desktop
sudo tasksel install server
sudo apt-get install linux-server linux-image-server
sudo apt-get --purge remove lightdm
</pre>

- If required you can remove the network manger and use one static IP

<pre>
sudo apt-get --purge remove network-manager
</pre>

- Restart machine

<pre>
sudo reboot -n
</pre>

### Put Ubuntu in the text mode

- In case that everything above has failed try the steps below

##### Step 1: Find out the current configuration

<pre>
systemctl get-default
</pre>

Can be graphical.target or multi-user.target

##### Step 2: Set the text mode

<pre>
sudo systemctl set-default multi-user.target
</pre>

##### Step3: Set the graphical mode

<pre>
systemctl set-default graphical.target
</pre>

##### Step 4: Reboot the machine

Restart the machine to apply all configurations made so far, and see that the Linux is now in the Text Mode

