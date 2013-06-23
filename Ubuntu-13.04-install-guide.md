First thing you will need is a copy of Ubuntu. You can grab a copy of their server edition [here](http://www.ubuntu.com/download/server)

Once it is installed, ensure everything is up to date by running:  
`sudo apt-get update`   
`sudo apt-get upgrade`

If you didn't install ssh during the install, you can install it now using:  
`sudo apt-get install ssh`

Apparmor interferes with some of our files, here is how to disable it:  
`sudo /etc/init.d/apparmor stop`  
`sudo /etc/init.d/apparmor teardown`   
`sudo update-rc.d -f apparmor remove`  

For the threaded scripts you will require the Python MySQLdb module:  
`sudo apt-get install python-mysqldb`

## Install and configure PHP

Install php5 by running:  
 `sudo apt-get install -y php5 php5-dev php-pear php5-gd php5-mysql php5-curl`

Configure PHP using the nano text editor:  
`sudo nano /etc/php5/cli/php.ini`

Change the following settings:  
register_globals = Off  
max_execution_time = 120  
(you can set 1024M to -1 if you have lots of RAM)  
memory_limit = 1024M  
(change Europe/London to your settings, see [http://php.net/manual/en/timezones.php](http://php.net/manual/en/timezones.php) for valid ones, remove the ; if there is one preceeding the date.timezone)  
date.timezone = Europe/London  

Press control+o to save and control+x to exit when you are done.

## Install a database

You can use a magnitude of database software including MySQL, Percona and MariaDB. If this is your first time MySQL is advised

### MYSQL

`sudo apt-get install mysql-server mysql-client libmysqlclient-dev`

### Percona

First you need to add Percona's repository to your machine:  
`gpg --keyserver  hkp://keys.gnupg.net --recv-keys 1C4CBDCDCD2EFD2A`  
`gpg -a --export CD2EFD2A | sudo apt-key add -`  
`sudo sh -c "echo  \"\n#Percona\" >> /etc/apt/sources.list"`  
`sudo sh -c "echo  \"deb http://repo.percona.com/apt raring main\" >> /etc/apt/sources.list"`  
`sudo sh -c "echo  \"deb-src http://repo.percona.com/apt raring main\" >> /etc/apt/sources.list"` 

Then you can install it using:  
`sudo apt-get update` 
`sudo apt-get install -y percona-server-client-5.5 \ percona-server-server-5.5 \ libmysqlclient-dev 

### MariaDB

`sudo apt-get install software-properties-common`  
`sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xcbcb082a1bb943db`  
`sudo add-apt-repository 'deb http://ftp.osuosl.org/pub/mariadb/repo/5.5/ubuntu raring main'`  

You can then install it using
`sudo apt-get update`  
`sudo apt-get install mariadb-server`

## Choose a webserver

You can also use a magnitude of webservers. Apache is the worlds most popular and the easiest for beginners. Nginx will use less resources but requires additional setup.

### Apache

To install Apache:  
`sudo apt-get install apache2`

Configure the php file:
`sudo nano /etc/php5/apache2/php.ini`

Change the following settings:   
register_globals = Off  
max_execution_time = 120  
(you can set 1024M to -1 if you have lots of RAM)  
memory_limit = 1024M  
(change Europe/London to your settings, see [http://php.net/manual/en/timezones.php](http://php.net/manual/en/timezones.php) for valid ones, remove the ; if there is one preceeding the date.timezone)  
date.timezone = Europe/London  

Create the site config:  
`sudo nano /etc/apache2/sites-available/nZEDb`

Paste the following:  
<VirtualHost *:80>
    ServerAdmin webmaster@localhost  
    ServerName localhost  

    # These paths should be fine  
    DocumentRoot /var/www/nZEDb/www  
    ErrorLog /var/log/apache2/error.log  
    LogLevel warn  
</VirtualHost>

Enable the site/etc:
`sudo a2dissite default`  
`sudo a2ensite nZEDb`  
`sudo a2enmod rewrite`  
`sudo service apache2 restart`  

**********************************************  
If you get the following error:
(Could not reliably determine the server's fully qualified domain name, using 127.0.1.1 for ServerName)

`sudo sh -c 'echo "ServerName localhost" >> /etc/apache2/conf.d/name' && sudo service apache2 restart`   
**********************************************

### Nginx

To install Nginx:  
`sudo apt-get install -y nginx`

You will also need a way to serve the php files to Nginx. To install php-fpm:  
`sudo apt-get install -y php5-fpm`

Configure the php file:
`sudo nano /etc/php5/fpm/php.ini`

Change the following settings:   
register_globals = Off  
max_execution_time = 120  
(you can set 1024M to -1 if you have lots of RAM)  
memory_limit = 1024M  
(change Europe/London to your settings, see [http://php.net/manual/en/timezones.php](http://php.net/manual/en/timezones.php) for valid ones, remove the ; if there is one preceeding the date.timezone)  
date.timezone = Europe/London 

Then restart php-fpm:
`sudo /etc/init.d/php5-fpm restart`

Nginx site configuration file:
`sudo nano /etc/nginx/sites-available/nZEDb`

Add:  
   server {
    # Change these settings to match your machine
    listen 80 default_server;
    server_name localhost;

    # Everything below here doesn't need to be changed
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    root /var/www/nZEDb/www/;
    index index.html index.htm index.php;

    location ~* \.(?:ico|css|js|gif|inc|txt|gz|xml|png|jpe?g) {
            expires max;
            add_header Pragma public;
            add_header Cache-Control "public, must-revalidate, proxy-revalidate";
    }

    location / { try_files $uri $uri/ @rewrites; }

    location @rewrites {
            rewrite ^/([^/\.]+)/([^/]+)/([^/]+)/? /index.php?page=$1&id=$2&subpage=$3 last;
            rewrite ^/([^/\.]+)/([^/]+)/?$ /index.php?page=$1&id=$2 last;
            rewrite ^/([^/\.]+)/?$ /index.php?page=$1 last;
    }

    location /admin { }
    location /install { }

    location ~ \.php$ {
            include /etc/nginx/fastcgi_params;
            fastcgi_pass 127.0.0.1:9000;

            # The next two lines should go in your fastcgi_params
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
   }


Make sure logs will write:
`sudo mkdir -p /var/log/nginx`  
`sudo chmod 755 /var/log/nginx` 

Make nginx server nZEDb instead of the default site:
`sudo unlink /etc/nginx/sites-enabled/default`  
`sudo ln -s /etc/nginx/sites-available/nZEDb /etc/nginx/sites-enabled/nZEDb`

## Post-Processing

If you want to run post-processing, you will need unrar, ffmpeg, media info and lame.

### Unrar

Its best to grab it straight from rarlabs. Download from [http://www.rarlab.com/download.htm](http://www.rarlab.com/download.htm) and put it under `~\unrar`

### Ffmpeg

As Ubuntu no longer ships with a proper ffmpeg, you are best to compile it. A good guide is [http://ffmpeg.org/trac/ffmpeg/wiki/UbuntuCompilationGuide](http://ffmpeg.org/trac/ffmpeg/wiki/UbuntuCompilationGuide)

### Media info
`sudo add-apt-repository ppa:shiki/mediainfo`  
`sudo apt-get update`   
`sudo apt-get install mediainfo` 

### Lame
`sudo apt-get install lame`

## Get nZEDb from github

If you dont already have git installed:
`sudo apt-get install git`

(if the folder doesn't exist, make it)  
cd /var/www/  
sudo chmod 777 .  

git clone https://github.com/nZEDb/nZEDb.git

sudo chmod 777 nZEDb  
cd nZEDb  
sudo chmod -R 755 .  
sudo chmod 777 /var/www/nZEDb/www/lib/smarty/templates_c  
sudo chmod -R 777 /var/www/nZEDb/www/covers  
sudo chmod 777 /var/www/nZEDb/www  
sudo chmod 777 /var/www/nZEDb/www/install  
sudo chmod -R 777 /var/www/nZEDb/nzbfiles  

7. Run the installer.

(change localhost for the server's IP if you are browsing on another computer)  
http://localhost/install  

8. Configure the site.

Enable some groups in view groups.

Change settings in edit site (set api keys, set paths to unrar etc..)

9. Start indexing groups.

Use scripts in misc/update_scripts (update_binaries to get articles, update_releases to create releases).

Use scripts in misc/update_scipts/nix_scripts to automate it.