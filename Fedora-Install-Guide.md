A quick install guide for those familiar with Fedora & RPM based distros. This document prepared with Fedora 19 using MariaDB.

**NOT finished!**

### Package Installation
Install development packages
* yum groupinstall "Development Tools" "Development Libraries"

Install general packages needed by nZEDb. (Some will be replaced with updated packages later)
* yum install lame php-pear-MDB2-Driver-mysql php-devel php php-pear php-gd php-mysql php-curl git tmux MySQL-python tmux MySQL-python python-sqlalchemy Cython python-setuptools

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

* wget "http://ffmpeg.org/releases/ffmpeg-2.0.1.tar.bz2"



