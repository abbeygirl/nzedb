These are some tips to "harden" your linux install, and make your nZEDb site a bit more on the secure side.  I'm sure there are some things I have forgotten, and I will update the list when I think of it.  Please let me know if you have any questions.  :)
* Use SSL, if have registered a domain, you should use SSL, as logins are currently sent in the clear.  You can get them free from [StartSSL.com](https://www.startssl.com).  Below are the global and site settings that should be used for apache
```
SSLCipherSuite EECDH+AES:EDH+AES:-SHA1:EECDH+RC4:EDH+RC4:EECDH+AES256:EDH+AES256:AES256-SHA:!aNULL:!eNULL:!EXP:!LOW:!MD5
SSLHonorCipherOrder on
SSLProtocol all -SSLv2
```

```
<VirtualHost example.com:443>
        <Directory /var/www/nZEDb/www/>
                Options FollowSymLinks
                AllowOverride All
                Order allow,deny
                allow from all
        </Directory>

        SSLEngine On
        SSLCACertificateFile /path/to/CA/chain/file
        SSLCertificateFile /path/to/crt/file
        SSLCertificateKeyFile /path/to/private/key/file
        ServerAdmin webmaster@example.com
        ServerName example.com
        DocumentRoot /var/www/nZEDb/www
        LogLevel warn
        ServerSignature Off
</VirtualHost>
```

* Change the following settings for apache (found in conf.d/security in a deb variant). This will make the "Server: " http header only have "Apache" in it rather then the full version.

```
ServerTokens Prod
ServerSignature Off
```

* In your php.ini file for apache turn off **expose_php**, this will supress the "X-Powered by" http header, and not let people know what version of php you are running.

* In /etc/sysctl.conf add the following, after you have added it run "sysctl -p".  This prevents a time-based TCP attack(I will add the ref later).
`net.ipv4.tcp_timestamps = 0`

* Setup iptables to only allow TCP port 80/443 from the internet to your server, and only allow other traffic (ssh/mysql/etc) from your local LAN. 
In the example below, the first line will allow all connections on the loopback interface (recommended), the second line will allow return traffic to come in (response from update server/dns/etc). The next two lines will allow only new incoming TCP connections from the anywere on ports 80 and 443, and the next two lines will allow ssh (tcp/22) and mysql (tcp/3306) only from the local LAN. The last line drops all other incoming traffic.

```
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A INPUT -i eth0 -s 0.0.0.0/0 -p tcp -m tcp --dport 80 -j ACCEPT
iptables -A INPUT -i eth0 -s 0.0.0.0/0 -p tcp -m tcp --dport 443 -j ACCEPT
iptables -A INPUT -i eth0 -s 192.168.1.0/24 -p tcp -m tcp --dport 22 -j ACCEPT
iptables -A INPUT -i eth0 -s 192.168.1.0/24 -p tcp -m tcp --dport 3306 -j ACCEPT
iptables -A INPUT -i eth0 -j DROP
```
