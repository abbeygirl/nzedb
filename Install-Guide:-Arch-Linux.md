This guide was written using Arch 218 (2015-02-15).

The nano text editor will be used extensively, please read [this](https://wiki.archlinux.org/index.php/Nano#Usage) page for command usage.

### Upgrade Arch ###

First, upgrade Arch: `sudo pacman -Syu`

Reboot: `sudo reboot now`

### Install PHP ###

`sudo pacman -S php php-pear php-gd php-fpm`

### Install MariaDB ###

`sudo pacman -S mariadb libmysqlclient`

### Install Nginx ###

`sudo pacman -S nginx`

### Configure PHP ###

`sudo nano /etc/php/php.ini`

Set `max_execution_time` to `120`

Set `memory_limit` to `1024M`

Remove the `;` in front of `date.timezone`, set `date.timezone` to your local timezone listed [here](http://php.net/manual/en/timezones.php).

Set `error_reporting` to `E_ALL` (remove `& ~E_DEPRECATED & ~E_STRICT`)

Set `log_errors` to `On`

Set `error_log` to `php-errors.log` (remove the leading `;`)

Remove the leading `;` from these lines:

`extension=dba.so`, `extension=exif.so`, `extension=gd.so`, `extension=iconv.so`,
`zend_extension=opcache.so`, `extension=openssl.so`, `extension=pdo_mysql.so`,
`extension=posix.so`, `extension=sockets.so`, `extension=zip.so`

Add `:/usr/bin/` to the end of `open_basedir`

Save / close the file.

### Configure MariaDB ###

Enable / start MySQL:

`sudo systemctl enable mysqld.service`

`sudo systemctl start mysqld.service`

Secure MySQL:

Note to not disable remote root login if you are on a remote computer (SSH).

`sudo mysql_secure_installation`

Create a MySQL user:

`sudo mysql -u root -p`

Change my_username and my_password.

`CREATE USER 'my_username'@'localhost' IDENTIFIED BY 'my_password';`

Change my_username. Change localhost to your hostname if you will use this as a remote installation.

`GRANT ALL PRIVILEGES ON *.* TO 'my_username'@'localhost' WITH GRANT OPTION;`

Again, change my_username and localhost if needed.

`GRANT FILE ON *.* TO 'my_username'@'localhost';`

Exit the MySQL client: `\q`

Edit the my.cnf file:

`sudo nano /etc/mysql/my.cnf`

Set `max_allowed_packet` to `16M`

Set `key_buffer_size` to `256M`

Add `group_concat_max_len = 8192` to the `[mysqld]` section.

Keep note of the location of `socket` for later use (should be `/run/mysqld/mysqld.sock`).

### Configure Nginx ###

Backup the original nginx.conf : `sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup`

Remove the original conf file: `sudo rm /etc/nginx/nginx.conf`

Create the nginx.conf : `sudo nano /etc/nginx/nginx.conf`

Paste the following:

    worker_processes  1;

    events {
        worker_connections  1024;
    }

    http {
        include       mime.types;
        default_type  application/octet-stream;
        sendfile        on;
        keepalive_timeout  65;

        server {
            # Change these settings to match your machine.
            listen 80 default_server;
            server_name localhost;

            # These are the log locations, you should not have to change these.
            access_log /var/log/nginx/access.log;
            error_log /var/log/nginx/error.log;

            # This is the root web folder for nZEDb, you shouldn't have to change this.
            root /srv/http/nZEDb/www/;
            index index.html index.htm index.php;

            # Everything below this should not be changed unless noted.
            location ~* \.(?:css|eot|gif|gz|ico|inc|jpe?g|js|ogg|png|svg|ttf|txt|woff|xml)$ {
                expires max;
                add_header Pragma public;
                add_header Cache-Control "public, must-revalidate, proxy-revalidate";
            }

            location / {
                try_files $uri $uri/ @rewrites;
            }

            location ^~ /covers/ {
                # This is where the nZEDb covers folder should be in.
                root /srv/http/nZEDb/resources;
            }

            location @rewrites {
                rewrite ^/([^/\.]+)/([^/]+)/([^/]+)/? /index.php?page=$1&id=$2&subpage=$3 last;
                rewrite ^/([^/\.]+)/([^/]+)/?$ /index.php?page=$1&id=$2 last;
                rewrite ^/([^/\.]+)/?$ /index.php?page=$1 last;
            }

            location /admin {
            }

            location /install {
            }

            location ~ \.php$ {
                include /etc/nginx/fastcgi_params;
                fastcgi_pass unix:/run/php-fpm/php-fpm.sock;

                # The next two lines should go in your fastcgi_params
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            }
        }
    }


Save/close the file.

Enable php-fpm: `sudo systemctl enable php-fpm.service`

Enable nginx: `sudo systemctl enable nginx.service`

Start php-fpm: `sudo systemctl start php-fpm.service`

Start nginx: `sudo systemctl start nginx.service`

### Install nZEDb ###

`sudo pacman -S git`

For optional but recommended software, see [here](https://github.com/nZEDb/nZEDb_Misc/blob/master/Guides/Installation/Ubuntu/Guide.md#step-10-installing-extra-optional-software), use pacman instead of apt-get.

Add your username to nginx's group:

`sudo usermod -a -G http my_username`

`newgrp http`

`cd /srv/http`

`sudo chown http:http /srv/http/`

`sudo chmod 775 /srv/http/`

`git clone https://github.com/nZEDb/nZEDb.git`

`sudo chmod -R 777 /srv/http/nZEDb/libs/smarty/templates_c/`

`sudo chmod -R 777 /srv/http/nZEDb/resources/`

`sudo chmod -R 777 /srv/http/nZEDb/www/`

(You can set 664 to most files and 775 to folders after the install otherwise MySQL's LOAD FILE will fail since it 's a different group than http.)

Open a browser, if on the same computer, head to localhost/install, if not, use the hostname or IP you set in the nginx conf.

On the Database Setup, enter the path we got above for the Socket Path (should be `/run/mysqld/mysqld.sock`), delete the Hostname, leave Port number empty.

On the OpenSSL page, it says the web user is www-data, on Arch it is http.

Finally, read up on [this](https://github.com/nZEDb/nZEDb_Misc/blob/master/Guides/Installation/Ubuntu/Guide.md#step-12-setting-up-nzedb) page to finish up.