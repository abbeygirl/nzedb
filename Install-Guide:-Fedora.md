A quick install guide for those familiar with Fedora & RPM based distros. This document was prepared using Fedora 19 with Apache & MariaDB. It describes installing under a separately mounted filesystem in this case called /var/www. As nZEDb can generate a heavy I/O load it's generally a good idea to have it on a dedicated disk. Is using an extX filesystem it should have at least 8M inodes.

The installation example is started using the root user, later changes to a specific nzedb user.

### Package Installation
Install development packages (useful for compiling stuff later on)
* yum groupinstall "Development Tools" "Development Libraries"

Install general packages needed by nZEDb.
* yum install yasm lame php-pear-MDB2-Driver-mysql php-devel php php-pear php-gd php-mysql php-curl git tmux MySQL-python tmux MySQL-python python-sqlalchemy Cython python-setuptools p7zip php-memcached php-opcache

Cython MySQL
* python -m easy_install --upgrade cymysql

### Configure PHP

* vi /etc/php.ini

```
memory_limit = 1024M   (1G minimum, 2G if system has RAM)
post_max_size = 64M    (used when doing large import/exports)
date.timezone = Europe/YourCountry
max_execution_time = 300    (>120 is recommended)
```

**Note**: the timezone settings of system, php, and mysql must match.

### MariaDB
Initial configuration below to get you started. Use MySQLtuner.pl to tune the database once running.
* vi /etc/my.cnf.d/server.cnf

Under [server]
```
group_concat_max_len=8192
innodb_file_per_table=1
max_allowed_packet=128M   (For dumping large tables)
```

* systemctl restart mariadb.service
* systemctl enable mysqld.service

If you're planning to use the "Table Per Group" feature the number of open file limit must be increased.
* vi /etc/my.cnf.d/server.cnf

Under [server]
```
open_files_limit=24576
```

Increase the total system limit and call "sysctl -p" after this edit:
```
vi /etc/sysctl.conf
fs.file-max = 512000
```

Also increase the open files limit for mysql user:
```
vi /etc/security/limits.conf
mysql soft nofile 24576
mysql hard nofile 32384
```

The systemd start scripts should also be adjusted
```
vi /etc/systemd/system/mariadb.service
.include /lib/systemd/system/mariadb.service
[Service]
LimitNOFILE=24576

systemctl daemon-reload
```

### Apache

Ensure apache is listening on the IP and port you expect
* vi /etc/httpd/conf/httpd.conf  
* Listen 192.168.x.x:80

Create nZEDb VirtualHost config

* vi /etc/httpd/conf.d/nzedb.conf

```  
  <VirtualHost *:80>
      Alias /nZEDb /var/www/nZEDb/www
      ServerAdmin nzedb@example.com
      DocumentRoot /var/www/nZEDb/www
      ServerName nzedb.example.com
      ErrorLog logs/nzedb.log
      CustomLog logs/nzedb.log common
      <Directory "/var/www/nZEDb/www">
                  AllowOverride All
                  Options FollowSymLinks
                  Order Deny,Allow
                  Allow from all
                  Require all granted
      </Directory>
      <Files ".ht*">
        Require all denied
      </Files>
  </VirtualHost>
```


* systemctl restart httpd.service
* systemctl enable httpd.service

Open up the firewall for http & https ports. Be sure to edit the Permanent Configuration.
* firewall-config

### Configure memcached (optional)
* yum install memcached    (done above)
* vi /etc/sysconfig/memcached   (configure as desired)
* systemctl start memcached.service
* systemctl enable memcached.service
* echo stats | nc localhost 11211   (test)

Enable memcache in /var/www/nZEDb/config.php

### Custom PHP extension for FAST yEnc decodes

See kevinlekiller's git page: [simple_php_yenc_decode](https://github.com/kevinlekiller/simple_php_yenc_decode)

This extension speeds up yEnc decoding approx 400x. Should you not successfully be able to use this perferred option, yydecode is a good alternative.

As root:
* yum install boost-regex boost-devel swig gcc-c++

git clone & compile
* git clone https://github.com/kevinlekiller/simple_php_yenc_decode.git
* cd simple_php_yenc_decode/source
* swig -php -c++ yenc_decode.i
* g++ `` `php-config --includes` `` -fpic -c yenc_decode_wrap.cpp
* g++ -fpic -c yenc_decode.cpp -lboost_regex
* g++ -shared *.o -o simple_php_yenc_decode.so -lboost_regex

As root:
* mkdir /usr/lib64/php/extensions
* cp -p /var/www/nZEDb/simple_php_yenc_decode/source/simple_php_yenc_decode.so  /usr/lib64/php/extensions
* vi /etc/php.ini    

and add..

`extension=/usr/lib64/php/extensions/simple_php_yenc_decode.so`

### Install php-opcache stats page (optional)
* cd /var/www/nZEDb/www/admin
* wget https://raw.github.com/rlerdorf/opcache-status/master/opcache.php
* later browse to http://your-site/admin/opcache.php


## Post-Processing extras (optional)

### unrar
Download latest tarball from rarlab http://www.rarlab.com/download.htm

* wget http://www.rarlab.com/rar/rarlinux-x64-5.1.0.tar.gz
* tar -xzf rarlinux-x64-5.1.0.tar.gz 
* cd rar
* sudo cp -p unrar /usr/local/bin/

Once nZEDB is up & running you point to this unrar version in Site -> Edit

### ffmpeg

Download latest tarball from http://ffmpeg.org or the static build here: http://johnvansickle.com/ffmpeg/

* wget http://ffmpeg.org/releases/ffmpeg-2.0.1.tar.bz2
* bunzip2 ffmpeg-2.0.1.tar.bz2
* tar -xf ffmpeg-2.0.1.tar
* cd ffmpeg-2.0.1
* ./configure
* make -j4     (j optional)
* make install

### mediainfo

Get latest version from https://mediaarea.net
* wget http://mediaarea.net/download/binary/libzen0/0.4.29/libzen0-0.4.29-1.x86_64.Fedora_19.rpm
* wget http://mediaarea.net/download/binary/libmediainfo0/0.7.64/libmediainfo0-0.7.64-1.x86_64.Fedora_19.rpm
* wget http://mediaarea.net/download/binary/mediainfo/0.7.64/mediainfo-0.7.64-1.x86_64.Fedora_19.rpm
* rpm -ivh libmediainfo0-0.7.64-1.x86_64.Fedora_19.rpm libzen0-0.4.29-1.x86_64.Fedora_19.rpm mediainfo-0.7.64-1.x86_64.Fedora_19.rpm

## Sphinx Search (optional)

* yum install sphinx

Check the installed MariaDB plugins
* MariaDB [(none)]> SHOW ENGINES;

If the Sphinx engine is not present use this once to load the dynamic module 
* MariaDB [(none)]> INSTALL SONAME 'ha_sphinx';

... to be continued ...


## Create nzedb user-id & git pull the source

It is strongly recommended that you run nZEDb as a non-root user.
* useradd -c nzedb -d /var/www/nZEDb nzedb
* su - nzedb
* mv .b* .v* /tmp/     (Hack: move the .bash* files away for git clone)
* cd /var/www
* git clone https://github.com/nZEDb/nZEDb.git
* cd ~
* mv /tmp/.b* . && mv /tmp/.v* .   (moves dot files back)

Certain directories need to be read/writable to both the nzedb user and apache. One way is to make these directories world writeable. For this see the other install guides. Alternatively you can open the home directory of the nzedb user to group members and add the apache user to the nzedb group.

As root user:
* chmod g+rx /var/www/nZEDb
* usermod -a -G nzedb apache
* chgrp apache /var/www/nZEDb/nzbfiles
* chgrp apache /var/www/smarty/templates_c

## Create nZEDb Database and grant user access
This step is optional as the installer can create the database for you. This method below is useful if you have other applications using the same database.

```
mysql -u root -p    (Default is no password)
create database nzedb;
use nzedb;
grant all privileges on nzedb.* to nzedb@'localhost' identified by 'YourPasswordHere';
grant all privileges on nzedb.* to nzedb@'127.0.0.1' identified by 'YourPasswordHere';
GRANT FILE on *.* to nzedb@'localhost';      (used when loading tables from system files)
GRANT RELOAD on *.* to nzedb@'localhost';    (used when optimising DB)
flush privileges;
exit
```

## Initial test

Point your browser to the defined IP/hostname. You should be automatically redirected to the install script.
Walk through the Pre-Flight checks and correct any possible issues. Follow the onscreen instructions.