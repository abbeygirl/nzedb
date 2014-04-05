Some of these commands might require root (sudo on ubuntu).

YourUnixUserName is the user your use in the terminal. You can type > echo $USERNAME < to get it (without the brackets.)

The default group for apache2 is www-data, so we will use that.
/PathTOnZEDbInstall/ is where you installed nZEDb (/var/www/nZEDb/ for example)

chown -R YourUnixUserName:www-data /PathTOnZEDbInstall/ \r\n
usermod -a -G www-data YourUnixUserName \r\n
chmod -R 774 /PathTOnZEDbInstall/ \r\n