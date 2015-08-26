NOTE: This guide implies you have a working FreeNAS 9.3 instance running with at least two standard jails – one for the web application and one for the SQL database. Both jails should be setup to allow you to SSH into them as opposed to going through the FreeNAS first. Anything appearing `like this` can be considered commands that can be entered directly into the command line prompt.

_Regardless of whether you're a Novice or FreeNAS Guru, do not be intimidated by these instructions! I worked tirelessly (and with loads of coffee) to make these instructions as easy as possible to the point where you should be able to copy and paste most (figure 95%) of what is written here :)_

## PART ONE – THE WEB APPLICATION

**_Installing Base Prerequisites:_**

`pkg remove -y sqlite3`

`pkg upgrade -y`

`pkg install -y nano git wget python py27-setuptools apache24`

`ln -s /usr/local/bin/git /usr/bin/git`

**_Installing PHP Prerequisites:_**

`pkg install –y php56 php56-curl php56-mcrypt php56-ctype php56-dba php56-exif php56-filter php56-gd php56-hash php56-iconv php56-json php56-mbstring php56-openssl php56-pdo php56-posix php56-session php56-simplexml php56-sockets php56-zlib php56-xml php56-mysql php56-pdo_mysql mod_php56`

`cd /media`

`wget http://pear.php.net/go-pear.phar`

`php go-pear.phar`

**_Installing Optional Prerequisites:_**

`pkg install –y unrar p7zip ffmpeg mediainfo screen coreutils yydecode`

**_Configure PHP options:_**

`cd /usr/local/etc`

`cp php.ini-production php.ini`

`nano php.ini`

Update the following settings:

* Set `date.timezone` to your timezone (refer to http://ca3.php.net/manual/en/timezones.php) – e.g. `America/New_York`

* Set `memory_limit` to 1024M or more (depending on system resources available)

* Set `max_execution_time` to 120 or more

* Confirm `sessions` is enabled (should be enabled by default)

**_Configure Web Server:_**

`nano /usr/local/etc/apache24/Includes/php.conf`

php.conf should contain the following:

`<FilesMatch "\.php$">`

`    SetHandler application/x-httpd-php`

`</FilesMatch>`

`<FilesMatch "\.phps$">`

`    SetHandler application/x-httpd-php-source`

`</FilesMatch>`

`nano /usr/local/etc/apache24/Includes/nZEDb.conf`

nZEDb.conf should have the following:

`ServerAdmin ABC` where ABC is an email address

`ServerName XYZ` where XYZ is either `localhost` or the name of the server

`<VirtualHost *:80>`

`    DocumentRoot "/usr/local/www/nZEDb/www"`


`    LogLevel warn`

`    ServerSignature Off`

`    ErrorLog /var/log/httpd-error.log`

`    <Directory "/usr/local/www/nZEDb/www">`

`        Options FollowSymLinks`

`        AllowOverride All`

`        Require all granted`

`    </Directory>`

`    Alias /covers /usr/local/www/nZEDb/resources/covers`

`</VirtualHost>`

_**Deploying the nZEDb code from GitHub:**_

`cd /usr/local/www`

`git clone https://github.com/nZEDb/nZEDb.git`

`chown –R www:www nZEDb`

`service lighttpd start`

## PART TWO – THE SQL DATABASE

**_Installing Prerequisites:_**

`pkg install –y mariadb55-server mariadb55-client nano`

`sysrc mysql_enable=YES`

`service mysql-server start`

`service mysql-server stop`

**_Configuring MariaDB:_**

`cd /usr/local/share/mysql`

`cp my-small.cnf my.cnf`

`nano my.cnf`

Make the following changes:

* Set `max_allowed_packet` to 16M

* Add `group_concat_max_len = 8192`

* Set `key_buffer_size` to 256M

`/usr/local/bin/mysql_secure_installation`

* `Set root password? [Y/n] = Y (and set accordingly)`

* `Remove anonymous users? [Y/n] = Y`

* `Disallow root login remotely? [Y/n] = Y`

* `Remove test database and access to it? [Y/n] = Y`

* `Reload privilege tables now? [Y/n] = Y`

service mysql-server start

**_Adding SQL user and table:_**

`mysql –u root –p`

`CREATE USER 'nzedb' IDENTIFIED BY 'PASSWORD';` (Replace PASSWORD with whatever you want)

`CREATE DATABASE nzedb`

`GRANT ALL on nzedb.* TO 'nzedb';`

quit

## PART THREE – SETUP NZEDB

* Open your web browser and point to http://[nzedb-app] where [nzedb-app] is the name of the jail/server setup in Part One

* Click the Pre-flight checklist button to proceed

* Step 1 should show everything as OK except for mod rewrite for lighttpd, which is a warning that can be ignored it steps above were followed

* Step 2 is the SQL setup, fill in the required information and, if done correctly, should connect to the database on the second FreeNAS jail

* Step 3 refers to SSL (only do if you plan on connecting remotely)

* Step 4 is Usenet setup (enter server information accordingly

* Step 5 saves the settings entered thus far

* Step 6 is to create an ADMIN user

* Step 7 is to confirm directories for nZEDb (OK to accept defaults)

* Login to the admin console and add some groups (e.g. teevee)

Back at the command prompt for the jail for the nZEDb web application…

`cd /usr/local/www/nZEDb/misc/update/nix/screen/sequential`

`screen sh simple.sh`

From here, you should be able to add groups, change site settings, connect Sabnzbd, etc.