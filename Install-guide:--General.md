If you don't find a specific guide for your OS, it is advised to use the [Ubuntu install guide](https://github.com/nZEDb/nZEDb_Misc/blob/master/Guides/Installation/Ubuntu/Guide.md). The info here is brief and intended for a general overview of the software components needed. 

Note: The nntp-proxy feature is deprecated. To get started use the PHP versions of the scripts. Installation of python is optional.

## Requirements 
### PHP  
php version 5.5 or higher (PHP 5.6+ is required for 32 bit Operating Systems, as it fixes a bug in a feature nZEDb uses)  
sessions enabled  
memory limit at 1024 or more  
minimum execution time of 120+ seconds  
make sure you update the php.ini for both web and cli  
OpenSSL (if connecting to ssl usenet provider)  
php register_globals off  
exif support (check the ini file).  

### GD Imaging Library w/PHP integration  

### PEAR

### MySQL  
minimum version 5.5.3  
max_allowed_packet = 16M  
group_concat_max_len = 8192  
timezone set to php's 

### Apache  
script timeout of at least 60 seconds  
mod_rewrite enabled  
.htaccess allow override on  

### 3rd Party API Keys (recommended to get your own api keys)  
tmdb (signup @ http://api.themoviedb.org/2.1/)  
amazon (signup @ https://affiliate-program.amazon.com/gp/advertising/api/detail/main.html)  
rottentomatoes (signup @ http://developer.rottentomatoes.com)  
trakt.tv (https://trakt.tv/api-docs)  
anidb (http://anidb.net)  

### Deep rar password inspection  
unrar version 3.9 or higher (unrar 5 highly recommended for newer releases)  
7zip  

### Thumbnailing and media info (if deep rar inspection is enabled)  
ffmpeg or avconv  
mediainfo  
lame

###Python 2.* or 3.*   
Python 2.* or 3.* - If Python is installed, the module also must be installed:
Python 2.*       
`apt-get install python-setuptools`  
`python -m easy_install`  
`easy_install cymysql`  
-or-    
Python 3.*    
`apt-get install python3-setuptools`    
`python3 -m easy_install pip`   
`pip3 install cymysql`

## INSTALLATION  
There is an installer in \install\ try it first by creating your website,  
copying the application files there, and browsing to http://yournewznabserver/install.  

Refer to the list of requirements above if you encounter any errors during install, or the FAQ in the README

Once installed activate only one or two groups to test with first (a.b.teevee is a good choice), this
will save you time if it is not working correctly.

Run the update_binaries.php and update_releases.php scripts in \misc\update_scripts\ via command-line.

If updating was successful then you can continue to setup your site and configure the update scripts for
auto-updating.


RUNNING OUTSIDE OF WEB ROOT 
set /.htaccess RewriteBase to your virtual directory


SAMPLE APACHE VHOST CONFIG  
add this to your existing VHOST file modifying your values for ServerName, Server Alias, and DocumentRoot.
You should find this under /etc/apache2/sites-enabled/ (000-default).  
```
<VirtualHost *:80>   
<Directory /domains/www/example.com/newz/www/>  
Options FollowSymLinks  
AllowOverride All  
Order allow,deny  
allow from all  
</Directory>  

ServerAdmin admin@example.com  
ServerName example.com  
ServerAlias www.example.com  
DocumentRoot /domains/www/example.com/newz/www  
LogLevel warn  
ServerSignature Off  
</VirtualHost>  
```