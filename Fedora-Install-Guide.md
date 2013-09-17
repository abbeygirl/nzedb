A quick install guide for those familiar with Fedora & RPM based distros. This document prepared with Fedora 19 using MariaDB.

**NOT finished!**

### Package Installation
Install development packages
* yum groupinstall "Development Tools" "Development Libraries"

Install general packages needed by nZEDb. (Some will be replaced with updated packages later)
* yum install yasm lame php-pear-MDB2-Driver-mysql php-devel php php-pear php-gd php-mysql php-curl git tmux MySQL-python tmux MySQL-python python-sqlalchemy Cython python-setuptools

Cython MySQL
* python -m easy_install --upgrade cymysql

### Configure PHP

* vi /etc/php.ini

memory_limit = 1024M   (1G minimum, 2G if system has RAM)

post_max_size = 64M    (used when doing large import/exports)

date.timezone = Europe/London

### MariaDB
Initial configuration below to get you started. Use MySQLtuner.pl to tune the database once running.
* vi /etc/my.cnf.d/server.cnf

Under [server]

group_concat_max_len=8192

innodb_file_per_table=1

* systemctl restart mariadb.service
* systemctl enable mysqld.service


### Apache

* vi /etc/httpd/conf.d/nzedb.conf

config goes here

* systemctl restart httpd.service
* systemctl enable httpd.service

## Post-Processing extras

### ffmpeg

Download latest tarball from http://ffmpeg.org

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

## Create nzedb user-id

It is strongly recommended that you run nZEDb as a non-root user.
* useradd 


