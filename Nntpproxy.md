# Intro

The primary purpose is to reduce the number of connections nZEDB has to make to update.  A proxy connection is established with the news server, and then nZEDB will use that established connection to request files for downloads. This an important concept to understand how the configuration works, in case this guide is unclear.

Currently you can either run this under tmux or you must manually run this script under screen.

There are a number of configuration changes to make this work.

You need to have Python 2 installed. If you are on a newer operating system which ships with Python 3 (Ubuntu 13.10 for example) you will need to install 2.7.

## Ubuntu instructions:

First install Python 2.7

`sudo easy_install-2.7 -U pip`


Then add the required modules

`sudo easy_install pynntp`
`sudo easy_install socketpool`

Copy the sample config  
`cp /var/www/nZEDb/misc/update_scripts/python_scripts/lib/nntpproxy.conf.sample /var/www/nZEDb/misc/update_scripts/python_scripts/lib/nntpproxy.conf`

and make the nessasary changes  
`sudo nano /var/www/nZEDb/misc/update_scripts/python_scripts/lib/nntpproxy.conf`

If you intend to use more than one NNTP you will need to repeat the config steps creating a second config file nntpproxy_a.conf and ensure you use a different port (I suggest 9992)

Finally you will need to point your install at the NNTP proxy:  
`sudo nano /var/www/nZEDb/www/config.php`

Change your NNTP server and port to reflect the proxy settings

