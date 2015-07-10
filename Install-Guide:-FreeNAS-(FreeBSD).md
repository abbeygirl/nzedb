NOTE: This guide implies you have a working FreeNAS 9.3 instance running with at least two standard jails – one for the web application and one for the SQL database. Both jails should be setup to allow you to SSH into them as opposed to going through the FreeNAS first. Anything appearing `like this` can be considered commands that can be entered directly into the command line prompt.

# **PART ONE – THE WEB APPLICATION**
**_Installing Base Prerequisites:_**
`pkg install -y nano git wget python py27-setuptools lighttpd`
`ln /usr/local/bin/git /usr/bin/git`
**_Installing PHP Prerequisites:_**
`pkg install –y  php56 php56-curl php56-mcrypt php56-ctype php56-dba php56-exif php56-filter php56-gd php56-hash php56-iconv php56-json php56-mbstring php56-openssl php56-pdo php56-posix php56-session php56-simplexml php56-sockets php56-zlib php56-xml php56-mysql php56-pdo_mysql`
`wget http://pear.php.net/go-pear.phar`
`php go-pear.phar`
**_Installing Optional Prerequisites:_**
`pkg install –y unrar p7zip ffmpeg mediainfo screen tmux coreutils`
