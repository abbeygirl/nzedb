A quick install guide for those familiar with Fedora & RPM based distros. This document prepared with Fedora 19 using a standard MySQL/InnoDB backend.

**NOT finished!**

### Package Installation
Install development packages
* yum groupinstall "Development Tools" "Development Libraries"

Install general packages needed by nZEDb. (Some will be replaced with updated packages later)
* yum install lame ffmpeg php-pear-MDB2-Driver-mysql php-devel php php-pear php-gd php-mysql php-curl git tmux MySQL-python tmux MySQL-python python-sqlalchemy Cython python-setuptools

Cython MySQL
* python -m easy_install --upgrade cymysql

### Configure PHP

* vi /etc/php.ini

memory_limit = 1024M   (1G minimum, 2G if system has RAM)

post_max_size = 64M    (used when doing large import/exports)

date.timezone = Europe/London





