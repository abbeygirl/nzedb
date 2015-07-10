NOTE: This guide implies you have a working FreeNAS 9.3 instance running with at least two standard jails – one for the web application and one for the SQL database. Both jails should be setup to allow you to SSH into them as opposed to going through the FreeNAS first. Anything appearing `like this` can be considered commands that can be entered directly into the command line prompt.

## PART ONE – THE WEB APPLICATION

**_Installing Base Prerequisites:_**

`pkg install -y nano git wget python py27-setuptools lighttpd`

`ln /usr/local/bin/git /usr/bin/git`

**_Installing PHP Prerequisites:_**

`pkg install –y  php56 php56-curl php56-mcrypt php56-ctype php56-dba php56-exif php56-filter php56-gd php56-hash php56-iconv php56-json php56-mbstring php56-openssl php56-pdo php56-posix php56-session php56-simplexml php56-sockets php56-zlib php56-xml php56-mysql php56-pdo_mysql`

`wget http://pear.php.net/go-pear.phar`

`php go-pear.phar`

**_Installing Optional Prerequisites:_**

`pkg install –y unrar p7zip ffmpeg mediainfo screen tmux coreutils`

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

`cd /usr/local/etc/lighttpd`

`nano lighttpd.conf`

Update the following settings:

* Comment out `server.use-ipv6 = "enable"`

* Set `server.document-root` to `/usr/local/www/nZEDb/www`

* Add `alias.url = ( "/covers" => "/usr/local/www/nZEDb/resources/covers" )`

* Set `server.stat-cache-engine` value to `“simple”`

* Set `server.max-connections` to `64`

* Add the following under URL rewrite section

`url.rewrite-once = (`

        `"^/.*\.(css|eot|gif|gz|ico|inc|jpe?g|js|ogg|oga|ogv|mp4|m4a|mp3|png|svg|ttf|txt|woff|xml)$" => "$0",`

        `"^/(admin|install).*$" => "$0",`

        `"^/([^/\.]+)/?(?:\?(.*))$" => "index.php?page=$1&$2",`

        `"^/([^/\.]+)/?$" => "index.php?page=$1",`

        `"^/([^/\.]+)/([^/]+)/?(?:\?(.*))$" => "index.php?page=$1&id=$2&$3",`

        `"^/([^/\.]+)/([^/]+)/?$" => "index.php?page=$1&id=$2",`

        `"^/([^/\.]+)/([^/]+)/([^/]+)/?$" => "index.php?page=$1&id=$2&subpage=$3"`

`)`

* Comment out `$SERVER["socket"] == "0.0.0.0:80" { }`

`nano conf.d/cgi.conf`

