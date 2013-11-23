This install guide contributed by sniffer http://nzedb.com/index.php?topic=967.0

### System Info

> Server version: Apache/2.2.15 (Unix)
> Server built:   Aug 13 2013 17:29:28
> Server's Module Magic Number: 20051115:25
> Server loaded:  APR 1.3.9, APR-Util 1.3.9
> Compiled using: APR 1.3.9, APR-Util 1.3.9
> Architecture:   64-bit
> Server MPM:     Prefork
>   threaded:     no
>     forked:     yes (variable process count

PHP 5.5.3 (cli) (built: Aug 24 2013 10:14:23)
Copyright (c) 1997-2013 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2013 Zend Technologies
    with Zend OPcache v7.0.3-dev, Copyright (c) 1999-2013, by Zend Technologies
   
mysql  Ver 14.14 Distrib 5.1.69, for redhat-linux-gnu (x86_64) using readline 5.1

ffmpeg version git-2013-11-22-51268aa Copyright (c) 2000-2013 the FFmpeg developers
  built on Nov 22 2013 17:37:41 with gcc 4.4.7 (GCC) 20120313 (Red Hat 4.4.7-3)

MediaInfoLib - v0.7.64
  
Python 3.3.3 (default, Nov 22 2013, 16:20:57)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-3)] on linux
distribute-0.6.49

p7zip Version 9.20 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,1 CPU)
unrar-4.2.3-1
=============================================================================================

# netinstall
download http://isoredirect.centos.org/centos/6/isos/x86_64/
url_setup
http://mirror.centos.org/centos/6/os/x86_64/

yum -y update
yum -y nano

nano /etc/sysconfig/iptables
(INSERT AFTER ssh port 22 line)
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT

adduser user 
passwd password

reboot login as root

yum -y install wget screen vim acpid
yum -y install httpd php mysql-server mysql memcached php-mysql php-gd php-pear mod_ssl php-curl php-process
chmod 777 /var/lib/php/session

rpm -Uvh http://pkgs.repoforge.org/unrar/unrar-4.2.3-1.el6.rf.x86_64.rpm

nstalling/Enabling RPMForge on CentOS 6.4 (latest binaries)
rnd=$RANDOM
wget -O /tmp/rpmforge-`echo $rnd`.rpm http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm
rpm --import http://apt.sw.be/RPM-GPG-KEY.dag.txt
rpm -i /tmp/rpmforge-`echo $rnd`.rpm
rm -f /tmp/rpmforge-`echo $rnd`.rpm
#

yum -y install p7zip
/usr/bin/7za

# update php
rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm
yum -y install php55w php55w-opcache
yum -y install yum-plugin-replace
yum -y replace php-common --replace-with=php55w-common
yum -y install php55w-opcache


Compile FFmpeg on CentOS 6.x
Campile is normale user

yum -y install autoconf automake gcc gcc-c++ git libtool make nasm pkgconfig zlib-devel openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel
mkdir -p ~/ffmpeg_sources

Yasm is an assembler used by x264 and FFmpeg

cd ~/ffmpeg_sources
curl -O http://www.tortall.net/projects/yasm/releases/yasm-1.2.0.tar.gz
tar xzvf yasm-1.2.0.tar.gz
cd yasm-1.2.0
./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin"
make && make install && make distclean
. ~/.bash_profile

x264 = H.264 video encoder

cd ~/ffmpeg_sources
git clone --depth 1 git://git.videolan.org/x264
cd x264
./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --enable-static
make && make install && make distclean

libfdk_aac = AAC audio encoder.
 
cd ~/ffmpeg_sources
git clone --depth 1 git://git.code.sf.net/p/opencore-amr/fdk-aac
cd fdk-aac
autoreconf -fiv
./configure --prefix="$HOME/ffmpeg_build" --disable-shared
make && make install && make distclean

libmp3lame = MP3 audio encoder.

cd ~/ffmpeg_sources
curl -L -O http://downloads.sourceforge.net/project/lame/lame/3.99/lame-3.99.5.tar.gz
tar xzvf lame-3.99.5.tar.gz
cd lame-3.99.5
./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --disable-shared --enable-nasm
make && make install && make distclean

libopus = Opus audio decoder and encoder

cd ~/ffmpeg_sources
curl -O http://downloads.xiph.org/releases/opus/opus-1.0.3.tar.gz
tar xzvf opus-1.0.3.tar.gz
cd opus-1.0.3
./configure --prefix="$HOME/ffmpeg_build" --disable-shared
make && make install && make distclean

libogg = Ogg bitstream library. Required by libtheora and libvorbis.

cd ~/ffmpeg_sources
curl -O http://downloads.xiph.org/releases/ogg/libogg-1.3.1.tar.gz
tar xzvf libogg-1.3.1.tar.gz
cd libogg-1.3.1
./configure --prefix="$HOME/ffmpeg_build" --disable-shared
make && make install && make distclean

libvorbis = Vorbis audio encoder. Requires libogg.

cd ~/ffmpeg_sources
curl -O http://downloads.xiph.org/releases/vorbis/libvorbis-1.3.3.tar.gz
tar xzvf libvorbis-1.3.3.tar.gz
cd libvorbis-1.3.3
./configure --prefix="$HOME/ffmpeg_build" --with-ogg="$HOME/ffmpeg_build" --disable-shared
make && make install && make distclean

libvpx = VP8/VP9 video encoder.

cd ~/ffmpeg_sources
git clone --depth 1 http://git.chromium.org/webm/libvpx.git
cd libvpx
./configure --prefix="$HOME/ffmpeg_build" --disable-examples
make && make install && make clean

FFmpeg

cd ~/ffmpeg_sources
git clone --depth 1 git://source.ffmpeg.org/ffmpeg
cd ffmpeg
PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig"
export PKG_CONFIG_PATH
./configure --prefix="$HOME/ffmpeg_build" --extra-cflags="-I$HOME/ffmpeg_build/include" --extra-ldflags="-L$HOME/ffmpeg_build/lib" --bindir="$HOME/bin" --extra-libs="-ldl" --enable-gpl --enable-nonfree --enable-libfdk_aac --enable-libmp3lame --enable-libopus --enable-libvorbis --enable-libvpx --enable-libx264
make && make install && make distclean
hash -r
. ~/.bash_profile

# install MediaInfo

rpm -Uvh http://mediaarea.net/download/binary/libzen0/0.4.29/libzen0-0.4.29-1.x86_64.CentOS_6.rpm
rpm -Uvh http://mediaarea.net/download/binary/libmediainfo0/0.7.64/libmediainfo0-0.7.64-1.x86_64.CentOS_6.rpm
rpm -Uvh http://mediaarea.net/download/binary/mediainfo/0.7.64/mediainfo-0.7.64-1.x86_64.CentOS_6.rpm

# install Python 3.3.3 on CentOS 6 

yum -y groupinstall "Development tools"
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel nasm

# install python3 to /opt/python3/ with

curl -O http://www.python.org/ftp/python/3.3.3/Python-3.3.3.tar.bz2
bzip2 -cd Python-3.3.3.tar.bz2 | tar xvf -  (tar xf Python-3.3.3.tar.bz2)
cd Python-3.3.3 && ./configure --prefix=/opt/python3
make && make install
ln -s /opt/python3/bin/python3 /usr/bin/python3

# install distribute

curl -O http://pypi.python.org/packages/source/d/distribute/distribute-0.6.49.tar
tar xvf distribute-0.6.49.tar.gz && cd distribute-0.6.49 && python3 setup.py install

# and a little pip wouldn't hurt:

cd /opt/pytnon3/bin && ./easy_install pip

# and finally, enter cymysql

cd /opt/pytnon3/bin && ./easy_install cymysql

sed -i -r 's/^(memory_limit)\s*=.*$/\1 = -1/' /etc/php.ini
sed -i -r 's/^(max_execution_time)\s*=.*$/\1 = 120/' /etc/php.ini
sed -i -r 's/^(error_reporting)\s*=.*$/\1 = E_ALL \^ E_STRICT/' /etc/php.ini
sed -i -r 's/^(register_globals)\s*=(.*)$/\1 = Off/' /etc/php.ini
sed -i -r 's/^;date.timezone =.*/date.timezone = Europe\/Amsterdam/' /etc/php.ini

nano /etc/my.cnf
[mysqld]
# nzedb recommended
max_allowed_packet      = 12582912
group_concat_max_len    = 8192

nano /etc/httpd/conf.d/nZEDb.conf
<VirtualHost *:80>
    ServerAdmin root@localhost
    DocumentRoot /var/www/html/nZEDb/www
    ServerName nZEDb.localdomain

    ErrorLog logs/nZEDb-error_log
    CustomLog logs/nZEDb-access_log common
    <Directory /var/www/html/nZEDb/www>
        Options +FollowSymLinks -Indexes
        AllowOverride All
    </Directory>
</VirtualHost>

service mysqld start ; chkconfig mysqld on
service httpd start ; chkconfig httpd on
service memcached start ; chkconfig memcached on
setsebool -P httpd_can_network_connect=1
setsebool -P httpd_can_sendmail=1

cd /var/www/html
git clone https://github.com/nZEDb/nZEDb.git

chcon -R unconfined_u:object_r:httpd_sys_content_t:s0 ./nZEDb
touch ./nZEDb/www/config.php
mkdir ./nZEDb/nzbfiles/tmpunrar
chown -R user:apache ./nZEDb
chmod -R 755 ./nZEDb
chmod 775 ./nZEDb/www/config.php
chmod -R 775 ./nZEDb/www/covers/
chmod 775 ./nZEDb/www
chmod 775 ./nZEDb/www/install
chmod -R 775 ./nZEDb/nzbfiles/
chmod -R 775 ./nZEDb/www/install/
chmod -R 775 ./nZEDb/www/lib/smarty/templates_c/

mysql_secure_installation

mysql -u root -p
-> mySQL password
CREATE USER 'nzedb'@'localhost' IDENTIFIED BY 'nzedb';
CREATE DATABASE nzedb;
GRANT ALL PRIVILEGES ON nzedb.* TO 'nzedb'@'localhost' IDENTIFIED BY 'nzedb';
quit

goto http://nZEDb/install

yum -y install time tmux
run it as normale user
cd /var/www/html/nZEDb/misc/update_scripts/nix_scripts/tmux
php start.php