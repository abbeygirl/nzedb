Some of these commands might require root (sudo on ubuntu).

YourUnixUserName is the user your use in the terminal. You can type > echo $USERNAME < to get it (without the brackets.)

The default group for apache2 is www-data, so we will use that.
/PathTOnZEDbInstall/ is where you installed nZEDb (/var/www/nZEDb/ for example)


***
* This first command will give ownership to the nZEDb files to your user and the apache www-data group:

`chown -R YourUnixUserName:www-data /PathTOnZEDbInstall/`

* This second command will give your user access to the group:

`usermod -a -G www-data YourUnixUserName`

* The final command will give read/write access to your user/group:

`chmod -R 774 /PathTOnZEDbInstall/`