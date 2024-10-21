
# APACHE SETTINGS

### Apache Install

<pre>
sudo apt-get update
sudo apt upgrade
sudo apt-get install apache2
</pre>

### Service Manager

<pre>
sudo service apache2 stop
sudo service apache2 start
sudo service apache2 status
sudo service apache2 restart
</pre>

### Virtual Hosts Sample

- Create files in the folder /etc/apache2/sites-available

[FILE] test.apache.conf

<code>

    <VirtualHost *:80>
        ServerAdmin admin@example.com
        ServerName test.apache
        ServerAlias test.apache.local
        DocumentRoot /var/www/test.apache/public
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>

</code>

vim: syntax=apache ts=4 sw=4 sts=4 sr noet

[FILE] sample.apache.conf

<code>

    <VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName sample.apache
    ServerAlias sample.apache.local
    DocumentRoot /var/www/sample.apache/public
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>

</code>

vim: syntax=apache ts=4 sw=4 sts=4 sr noet

- Active the new sites

<pre>
sudo a2ensite test.apache.conf
sudo a2ensite sample.apache.conf
</pre>

- Restart Apache

<pre>
sudo service apache2 restart
</pre>

- Enable Rewrite

<pre>
touch .htaccess
sudo a2enmod restart
sudo a2enmod rewrite
</pre>

- Create a symbol link for the project structure (optional)

<pre>
cd /var/www
sudo ln -s /home/jereelton/Server/Devel/apache/sample sample.apache
sudo ln -s /home/jereelton/Server/Devel/apache/test test.apache
</pre>

- Edit the /etc/hosts file like below

<pre>
192.168.15.13   sample.apache.local
192.168.15.13   test.apache.local
</pre>

- HTACCESS Sample

<pre>
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php?url=$1 [QSA,L]
</pre>

