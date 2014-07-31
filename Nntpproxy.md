## Intro

If you need a connection limiting (non-pooling) nntp-proxy, [this one](https://github.com/nieluj/nntp-proxy) works well.

------

The primary purpose is to reduce the number of connections nZEDb has to make to update.  A proxy connection is established with the news server, and then nZEDb will use that established connection to request files for downloads. This an important concept to understand how the configuration works, in case this guide is unclear.

Currently you can either run this under tmux or you must manually run this script under screen. The proxy is `nntpproxy.py` and a tmux helper script is available `nntpproxy.php`.

There are a number of configuration changes to make this work.

You need to have Python 2 installed. If you are on a newer operating system which ships with Python 3 (Ubuntu 13.10 for example) you will need to install 2.7.

##Installation

### Ubuntu instructions:

First, check that you have Python 2.7 installed:
```
python
Python 2.7.3 (default, Sep 26 2013, 16:35:25)
[GCC 4.7.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
Skip this if it's already installed:
```
sudo easy_install-2.7 -U pip
```

Then add the required modules. This is best installed as the user that will run the scripts.
```
sudo apt-get install -yqq python-setuptools python-pip python-dev python-software-properties
pip install --user socketpool
```
Copy the sample config  
```
cp /var/www/nZEDb/misc/update/python/lib/nntpproxy.conf.sample /var/www/nZEDb/misc/update/python/lib/nntpproxy.conf
```
and make the necessary changes  
```
nano /var/www/nZEDb/misc/update/python/lib/nntpproxy.conf
```
example
```
{
    "usenet": {
        "host": "news.supernews.com",
        "port": 443,
        "username": "your_user_name",
        "password": "your_password",
        "use_ssl": true
    },
    "proxy": {
        "host": "localhost",
        "port": 9991
    },
    "pool": {
        "size": 20,
        "pooling-time": 30
    }
}
```
The pool size is not a max or limiter. It just maintains that number of connections when the scripts are idle.
Pooling-time is the amount of seconds to keep the NNTP connection alive when it is un-used.


If you intend to use more than one NNTP you will need to repeat the config steps creating a second config file nntpproxy_a.conf and ensure you use a different port (I suggest 9992)

Finally you will need to point your install at the NNTP proxy:  
```
nano /var/www/nZEDb/www/config.php
```
Change your NNTP server and port to reflect the proxy settings.

example
```
define('NNTP_USERNAME', '');
define('NNTP_PASSWORD', '');
define('NNTP_SERVER', 'localhost');
define('NNTP_PORT', '9991');
define('NNTP_SSLENABLED', false);
```
Enable NNTP Proxy in site edit.
Tmux scripts run the proxy without anything further, unless you are running Complete Sequential. If running Complete Sequential you will need to uncomment the section for NNTP Proxy.

To use NNTP Proxy with the screen scripts, add the following to start.sh:
```
    if ! $SCREEN -list | grep -q "PROXY"; then
        cd $NZEDB_PATH && $SCREEN -dmS PROXY $PYTHON -OOu ${THREAD_PATH}/nntpproxy.py
    fi
```

If you run things manually, you can use a tmux helper script:
```
php nntpproxy.php
```
or, run it manually:
```
python nntpproxy.py
```

If you use the default configuration file, you do not need to specify it. Otherwise you must point to it:
```
python nntpproxy.py /var/www/nZEDb/misc/update/python/lib/my_nntpproxy.conf
```