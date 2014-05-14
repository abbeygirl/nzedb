**This is still work in progress - NEEDS FORMATTING ETC..**

_**Prerequisite**_ - Install XCODE , sequelpro, OSX Server APP, iterm2 app, git

***

**_Section A - Install Hombrew_**
* In terminal issue the following commands <br>
```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

***

**_Section B - Install Python_**
* In terminal Issue the following commands <br>
```
brew install python
brew install python3
export PATH=/usr/local/share/python:$PATH
```

***
	
**_Section C - Install MYSQL_**
* In terminal issue the following commands <br>
```
brew install mysql
pip install MySQL-python
pip install cymysql
pip3 install cymysql
```

* In terminal issue the following command <br>
```
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
mysqladmin -u root password NEWPASSWORD
```
	
***

**_Section D - Find PHP.INFO used by OS X APACHE_** <br>
Create a PHP file in the root folder of your site contianing the following: <br>
```
<?
php phpinfo();
?>
```

***


**_Section E - Modify the php.ini file_** <br>
```
register_globals = Off <br>
max_execution_time = 120 <br>
memory_limit = 1024M <br>
date.timezone = Europe/London <br>
```
***


**_Section F - post Processing Tools installation_**

_Unrar:_ <br>
* Download CLI tool from rarLab's site & extract it <br>
* In terminal issue the following commands <br>
```
cd Downloads/rar
sudo install -c -o $USER unrar /bin
```
_ffmpeg:_ <br>
* install macports <br>
* In terminal issue the following commands (restart terminal app after installing macports) <br>
```
sudo port install ffmpeg +gpl +postproc +lame +theora +libogg +vorbis +xvid +x264 +a52 +faac +faad +dts nonfree
```

_mediainfo_ <br>
* install dmg  - [MediaInfo DMG](http://mediaarea.net/en/MediaInfo/Download/Mac_OS)<br> 

***

**_Section G - Install Pear_** <br>

* In terminal issue the following commands <br>
```
curl -O http://pear.php.net/go-pear.phar
sudo php -d detect_unicode=0 go-pear.phar
```
* Type 1 and press enter <br>
* Enter `/usr/local/pear` at the prompt and press enter <br>
* Type 4 and press enter <br>
* Type `/usr/local/bin` at the prompt and press enter <br>
* Press enter <br>
* Type Y and press enter <br>
* Press enter <br>
* Restart apache from OSX Server Admin App <br>

***

**_Section H - Downloading nZEDb_** <br>

* Create the website using Apple Server app ( I called it nzedb) <br>
* In the website properties (pencil icon in Server App) select advanced and enable the allow overrides option <br>
* Ensure that the setting Enable PHP web applications is ticked in the Web Server settings section of Server App <br>
* In terminal issue the following commands <br>
```
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
```
* OSX Server admin set root directory of website to /Library/Server/Web/Data/Sites/nzedb/nZEDb/www <br>
* Browse to site and run Installation wizard <br>

***

**_Section I - Tmux_** <br>
* In Terminal issue the following commands <br>
```
brew install tmux
```