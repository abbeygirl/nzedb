**This is still work in progress - NEEDS FORMATTING ETC..**

Prerequisite - Install XCODE , sequelpro, OSX Server APP, iterm2 app, git

**Section A - Install Hombrew**
1) In terminal issue the following commands 
	`ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"`
	
-----------------------------------------------------------------------------------------
**Section B - Install Python**
1) In terminal Issue the following commands
	`brew install python`
        `brew install python3`
	`export PATH=/usr/local/share/python:$PATH`

-----------------------------------------------------------------------------------------	
	
**Section C - Install MYSQL**
1) In terminal issue the following commands 
	`brew install mysql`
    `pip install MySQL-python`
	`pip install cymysql`
        `pip3 install cymysql`
3) In terminal issue the following command
        `ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents`
        `launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist`
	`mysqladmin -u root password NEWPASSWORD`
	
-----------------------------------------------------------------------------------------

**Section D - Find PHP.INFO used by OS X APACHE**

Create a PHP file in the root folder of your site contianing the following:
`
<?php
phpinfo();
?>
`
------------------------------------------------------------------------------------------

**Section E - Modify the php.ini file**

register_globals = Off
max_execution_time = 120
memory_limit = 1024M
date.timezone = Europe/London

-----------------------------------------------------------------------------------------

**Section F - post Processing Tools installation**

_Unrar:_
	1) Download CLI tool from rarLab's site & Extract it
	2) In terminal issue the following commands
	   `cd Downloads/rar`
	   `sudo install -c -o $USER unrar /bin`
           

_ffmpeg:_
     1) install macports
     2) In terminal issue the following commands (restart terminal app after installing macports)
      `sudo port install ffmpeg +gpl +postproc +lame +theora +libogg +vorbis +xvid +x264 +a52 +faac +faad +dts +nonfree`

_mediainfo_
  1) install dmg from mediaarea site

————————————————————————————-

**Section G - Install Pear**

1) In terminal issue the following commands
	`curl -O http://pear.php.net/go-pear.phar`
	`sudo php -d detect_unicode=0 go-pear.phar'
2) Type 1 and press enter
3) Enter /usr/local/pear at the prompt and press enter
4) type 4 and press enter
5) type /usr/local/bin at the prompt and press enter
6) Press enter
7) type Y and press enter
8) Press enter
9) Restart apache from OSX Server Admin App
—————————————————————————————————————————————————————————
**Section H - Downloading nZEDb**

    1) Create the website using Apple Server app ( I called it nzedb)
    2) In the website properties (pencil icon in Server App) select advanced and enable the allow overrides option
    3) Ensure that the setting Enable PHP web applications is ticked in the Web Server settings section of Server App
    4) In terminal issue the following commands
	cd /Library/Server/Web/Data/Sites/nzedb
	sudo chmod 777 /Library/Server/Web/Data/Sites/nzedb
	git clone https://github.com/nZEDb/nZEDb.git
	sudo chmod 777 nZEDb
	cd nZEDb
	sudo chmod 777 /Library/Server/Web/Data/Sites/nzedb/nZEDb/www/lib/smarty/templates_c
	sudo chmod -R 777 /Library/Server/Web/Data/Sites/nzedb/nZEDb/www/covers
	sudo chmod 777 /Library/Server/Web/Data/Sites/nzedb/nZEDb/www
	sudo chmod 777 /Library/Server/Web/Data/Sites/nzedb/nZEDb/www/install
	sudo chmod -R 777 /Library/Server/Web/Data/Sites/nzedb/nZEDb/nzbfiles
    5) OSX Server admin set root directory of website to /Library/Server/Web/Data/Sites/nzedb/nZEDb/www
    6) Browse to site and run Installation wizard

—————————————————————————————————————

**Section I - memcached**

	1) In Terminal issue the following commands
		`pip install python-memcached`
	2)Restart Apache from OSX Server Admin app
	3) Edit /Library/Server/Web/Data/Sites/nzedb/nZEDb/www/config.php file as follows
		`change MEMCACHE_ENABLED to true`

**Section J - Tmux**
1) In Terminal issue the following commands
' brew install tmux ' 