### Setting up yydecode with nZEDb for increased yEnc decode speed.:

* Move to your home folder:

`cd ~/`

* Create a temp folder to work in and move into it:

`mkdir tempyydecode`

`cd tempyydecode`

* Download and extract the yydecode source code:

`wget http://downloads.sourceforge.net/project/yydecode/yydecode/0.2.10/yydecode-0.2.10.tar.gz`

`tar -xvf yydecode-0.2.10.tar.gz`

* Move into the yydecode folder and prepare for compiling:

`cd yydecode-0.2.10/`

`./configure`

* Run the following commands as root (sudo in ubuntu):

`make`

`sudo make install`

`make clean`

* Delete the temp yydecode folder we made and all the contents inside.

`rm -rf ~/tempyydecode/`

* Check if it installed (if you see all the commands list then it's installed):

`yydecode --help`

* By default it installs in /usr/local/bin/yydecode . You can make sure if it did:

`which yydecode`

* Now add the path to your admin site edit section.

* Finally set the permissions to read/write on (inside your nZEDb folder) /resources/tmp/yEnc/

* If you want to see if it works, run /misc/update/postprocess.php additional true

* See if you get stuff after (rB) , like m or ^ or s etc...

* If you only see (rB) you might have permission issues.