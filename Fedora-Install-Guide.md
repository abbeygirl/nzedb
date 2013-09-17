A quick install guide for those familiar with Fedora & RPM based distros. This document prepared Fedora 19 using Apache & MariaDB. It describes installing under a separately mounted filesystem in this case called /var/www. As nZEDb can generate a heavy I/O load it's generally a good idea to have it on a dedicated disk. The filesystem should have at least 8M inodes.

The installation example is started using the root user, later changes to a specific nzedb user.

### Package Installation
Install development packages (useful for compiling stuff later on)
* yum groupinstall "Development Tools" "Development Libraries"

Install general packages needed by nZEDb.
* yum install yasm lame php-pear-MDB2-Driver-mysql php-devel php php-pear php-gd php-mysql php-curl git tmux MySQL-python tmux MySQL-python python-sqlalchemy Cython python-setuptools

Cython MySQL
* python -m easy_install --upgrade cymysql

### Configure PHP

* vi /etc/php.ini

memory_limit = 1024M   (1G minimum, 2G if system has RAM)

post_max_size = 64M    (used when doing large import/exports)

date.timezone = Europe/YourCountry

**Note**: the timezone settings of system, php, and mysql must match.

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

config goes here ** Not finished **

* systemctl restart httpd.service
* systemctl enable httpd.service

## Post-Processing extras

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

## Create nzedb user-id & git pull the source

It is strongly recommended that you run nZEDb as a non-root user.
* useradd -c nzedb -d /var/www/nZEDb nzedb
* su - nzedb
* mv .b* .v* /tmp/     (HACK: move the .bash etc files to tmp for now for git clone)
* cd /var/www
* git clone https://github.com/nZEDb/nZEDb.git
* cd ~
* mv /tmp/.b* . && mv /tmp/.v* .   (moves dot files back)

Certain directories need to be read/writable to both the nzedb user and apache. One way is to make these directories world writeable. For this see the other install guides. Alternatively you can open the home directory of the nzedb user to group members and add the apache user to the nzedb group.

As root user:
* chmod g+rx /var/www/nZEDb
* usermod -a -G nzedb apache





** Not Finished! **