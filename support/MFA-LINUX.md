
# MFA & LINUX

### Requisites

- SETUP CORRECTLY THE SSHD SERVICE
- GOOGLE AUTHENTICATOR APP IS REQUIRED

### Tested Distros

- UBUNTU 20.04
- FEDORA 31
- FEDORA 40
- REDHAT 9
- ALMA LINUX 8.10
- ALMA LINUX 9.4

> Note: It can be applied in the EC2 instances as well

***********************************************************************************************************************
### UBUNTU 2004 INSTALLATION
***********************************************************************************************************************

###### Install the Google Authenticator PAM Library (PAM: Pluggable Authentication Modules)

<pre>
sudo apt install libpam-google-authenticator
</pre>

###### Configure Google Authenticator for the current user

<pre>
google-authenticator -t -Q UTF8 -s /home/${USER}/.ssh/.google_authenticator
</pre>

- Answer the questions: y for all questions
- Scan the QR Code by your phone device

###### Configure SSH with MFA

<pre>
sudo vi /etc/pam.d/sshd
</pre>

- Add the entry line at the end of the file like this

<pre>
auth required pam_google_authenticator.so secret=/home/${USER}/.ssh/.google_authenticator nullok
</pre>

<pre>
sudo vi /etc/ssh/sshd_config
</pre>

- Set the field ChallengeResponseAuthentication to yes value

<pre>
ChallengeResponseAuthentication yes
</pre>

- Restart SSH service

<pre>
sudo service sshd restart
</pre>

###### Configure Console with MFA

<pre>
sudo vi /etc/pam.d/common-session
</pre>

- Add the entry line at the end of the file like this

<pre>
auth required pam_google_authenticator.so
</pre>

- Make logout session and login new session

###### Configure SUDO with MFA

<pre>
sudo vi /etc/pam.d/common-auth
</pre>

- Add the line at the end of file like this

<pre>
auth required pam_google_authenticator.so nullok
</pre>

- Make logout session and login new session, try run the command sudo -i (you should be prompted by the verification code)

***********************************************************************************************************************
### FEDORA 31 INSTALLATION
***********************************************************************************************************************

###### Update the system and install Google Authenticator libraries

<pre>
sudo yum update
sudo dnf install google-authenticator qrencode-libs
</pre>

###### Edit the SSH files configuration

<pre>
sudo vi /etc/pam.d/sshd
</pre>

The sshd file should be like below
<pre>
#%PAM-1.0
auth       substack     password-auth
auth       include      postlogin
account    required     pam_sepermit.so
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    required     pam_namespace.so
session    optional     pam_keyinit.so force revoke
session    optional     pam_motd.so
session    include      password-auth
session    include      postlogin
auth required pam_google_authenticator.so secret=/home/${USER}/.ssh/.google_authenticator nullok
</pre>

<pre>
sudo vi /etc/ssh/sshd_config
</pre>

The file sshd_config should be changed in the fields described below

<pre>
ChallengeResponseAuthentication yes
UsePAM yes
</pre>

###### Restart SSH service

<pre>
sudo systemctl restart sshd
sudo systemctl status sshd
</pre>

###### Make sure thar the directory .ssh exists

<pre>
mkdir -p /home/${USER}/.ssh/
chmod 755 /home/${USER}/.ssh/
</pre>

###### Generate the QR Google Authentication Code

<pre>
google-authenticator -t -Q UTF8 -s /home/${USER}/.ssh/.google_authenticator
</pre>

###### Read the QR Code in the Google Authenticator App

- Scan the QR code in the app google authenticator

***DONE***

***********************************************************************************************************************
### FEDORA 40 INSTALLATION
***********************************************************************************************************************

###### System Update and install Google Authenticator libraries

<pre>
sudo yum update
sudo yum install net-tools
sudo yum install pam-devel
sudo dnf install google-authenticator qrencode-libs
</pre>

###### Edit the SSH files configuration

<pre>
sudo vi /etc/pam.d/sshd
</pre>

The sshd file should be like below

<pre>
#%PAM-1.0
auth       substack     password-auth
auth       include      postlogin
account    required     pam_sepermit.so
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    required     pam_namespace.so
session    optional     pam_keyinit.so force revoke
session    optional     pam_motd.so
session    include      password-auth
session    include      postlogin
auth required pam_google_authenticator.so secret=/home/${USER}/.ssh/.google_authenticator nullok
</pre>

<pre>
sudo vi /etc/ssh/sshd_config
</pre>

Change the field UsePAM in the sshd_config file

<pre>
UsePAM yes
</pre>

> IMPORTANT: Change the 50-redhat.conf file

<pre>
sudo vi /etc/ssh/sshd_config.d/50-redhat.conf
</pre>

Set the field ChallengeResponseAuthentication according below

<pre>
ChallengeResponseAuthentication yes
</pre>

###### Restart SSH service

<pre>
sudo systemctl restart sshd
sudo systemctl status sshd
</pre>

###### Make sure thar the directory .ssh exists

<pre>
mkdir -p /home/${USER}/.ssh/
chmod 755 /home/${USER}/.ssh/
</pre>

###### Generate the QR Google Authentication Code

<pre>
google-authenticator -t -Q UTF8 -s /home/${USER}/.ssh/.google_authenticator
</pre>

###### Read the QR Code in the Google Authenticator App

- Scan the QR code in the app google authenticator

***DONE***

***********************************************************************************************************************
## REDHAT 9 INSTALLATION
***********************************************************************************************************************

###### System Update and install Google Authenticator libraries

<pre>
sudo yum update
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
sudo /usr/bin/crb enable
sudo yum install net-tools
sudo yum install pam-devel
sudo dnf install google-authenticator qrencode-libs
</pre>

###### Edit the SSH files configuration

<pre>
sudo vi /etc/pam.d/sshd
</pre>

The sshd file should be like below

<pre>
#%PAM-1.0
auth       substack     password-auth
auth       include      postlogin
account    required     pam_sepermit.so
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    required     pam_namespace.so
session    optional     pam_keyinit.so force revoke
session    optional     pam_motd.so
session    include      password-auth
session    include      postlogin
auth required pam_google_authenticator.so secret=/home/${USER}/.ssh/.google_authenticator nullok
</pre>

<pre>
sudo vi /etc/ssh/sshd_config
</pre>

Change the field UsePAM in the sshd_config file

<pre>
UsePAM yes
</pre>

> IMPORTANT: Change the 50-redhat.conf file

<pre>
sudo vi /etc/ssh/sshd_config.d/50-redhat.conf
</pre>

Set the field ChallengeResponseAuthentication according below

<pre>
ChallengeResponseAuthentication yes
</pre>

###### Restart SSH service

<pre>
sudo systemctl restart sshd
sudo systemctl status sshd
</pre>

###### Make sure thar the directory .ssh exists

<pre>
mkdir -p /home/${USER}/.ssh/
chmod 755 /home/${USER}/.ssh/
</pre>

###### Generate the QR Google Authentication Code

<pre>
google-authenticator -t -Q UTF8 -s /home/${USER}/.ssh/.google_authenticator
</pre>

###### Read the QR Code in the Google Authenticator App

- Scan the QR code in the app google authenticator

***DONE***


***********************************************************************************************************************
### ALMALINUX 8.10 INSTALLATION
***********************************************************************************************************************

###### System Update and install Google Authenticator libraries

<pre>
sudo yum update
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo /usr/bin/crb enable
sudo yum install net-tools
sudo yum install pam-devel
sudo dnf install google-authenticator qrencode-libs
</pre>

###### Edit the SSH files configuration

<pre>
sudo vi /etc/pam.d/sshd
</pre>

<pre>
#%PAM-1.0
auth       substack     password-auth
auth       include      postlogin
account    required     pam_sepermit.so
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    required     pam_namespace.so
session    optional     pam_keyinit.so force revoke
session    optional     pam_motd.so
session    include      password-auth
session    include      postlogin
auth required pam_google_authenticator.so secret=/home/${USER}/.ssh/.google_authenticator nullok
</pre>

<pre>
sudo vi /etc/ssh/sshd_config
</pre>

<pre>
ChallengeResponseAuthentication yes
UsePAM yes
</pre>

###### Restart SSH service

<pre>
sudo systemctl restart sshd
sudo systemctl status sshd
</pre>

###### Make sure thar the directory .ssh exists

<pre>
mkdir -p /home/${USER}/.ssh/
chmod 755 /home/${USER}/.ssh/
</pre>

###### Generate the QR Google Authentication Code

<pre>
google-authenticator -t -Q UTF8 -s /home/${USER}/.ssh/.google_authenticator
</pre>

###### Read the QR Code in the Google Authenticator App

- Scan the QR code in the app google authenticator

***DONE***


***********************************************************************************************************************
### ALMALINUX 9.4 INSTALLATION
***********************************************************************************************************************

###### System Update and install Google Authenticator libraries

<pre>
sudo yum update
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
sudo /usr/bin/crb enable
sudo yum install net-tools
sudo yum install pam-devel
sudo dnf install google-authenticator qrencode-libs
</pre>

###### Edit the SSH files configuration

<pre>
sudo vi /etc/pam.d/sshd
</pre>

<pre>
#%PAM-1.0
auth       substack     password-auth
auth       include      postlogin
account    required     pam_sepermit.so
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    required     pam_namespace.so
session    optional     pam_keyinit.so force revoke
session    optional     pam_motd.so
session    include      password-auth
session    include      postlogin
auth       required pam_google_authenticator.so secret=/home/${USER}/.ssh/.google_authenticator nullok
</pre>

<pre>
sudo vi /etc/ssh/sshd_config
</pre>

<pre>
UsePAM yes
</pre>

<pre>
sudo vi /etc/ssh/sshd_config.d/50-redhat.conf
</pre>

<pre>
ChallengeResponseAuthentication yes
</pre>

###### Restart SSH service

<pre>
sudo systemctl restart sshd
sudo systemctl status sshd
</pre>

###### Make sure thar the directory .ssh exists

<pre>
mkdir -p /home/${USER}/.ssh/
chmod 755 /home/${USER}/.ssh/
</pre>

###### Generate the QR Google Authentication Code

<pre>
google-authenticator -t -Q UTF8 -s /home/${USER}/.ssh/.google_authenticator
</pre>

###### Read the QR Code in the Google Authenticator App

- Scan the QR code in the app google authenticator

***DONE***


