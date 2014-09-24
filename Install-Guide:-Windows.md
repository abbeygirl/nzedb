Although windows is not offically supported, you can run it under the XAMMP stack. However you will not be able to utilize the tmux scripts.

* Download XAMPP from https://www.apachefriends.org/download.html and install to "C:\xampp\"   

* Download Git for Windows from http://git-scm.com/download/win and install it.  
IMPORTANT: PICK THIS OPTION WHILE INSTALLING GIT OR THE INSTALL OF nZEDb WILL FAIL (you have been warned) : 
Checkout as-is, commit Unix-style line endings

* After installation of git, go to a command line or run window by pressing Windows key + r, type `control sysdm.cpl` and run it, go to the Advanced tab, click Environment Variables, in System variables, scroll down to the Path variable, click edit, in Variable Value, scroll to the far right, add the path to the git cmd folder like this for example: `;C:\Program Files (x86)\Git\cmd`  

* Download gnuwin32 from http://sourceforge.net/projects/getgnuwin32/files/ , the current version is GetGnuWin32-0.6.3.exe , install it to "C:\" , it will create a folder called "GetGnuWin32" automatically.  
After you finish installing it, add `;C:\GetGnuWin32\bin` to your Environment Variables Path variable (as we did above for git)

* You can run the the download.bat and install.bat scripts in C:\GetGnuWin32\ to install all the packages (it can take a while depending on the speed of your internet connection), otherwise you can follow the next 3 parts of the guide to install the main required packages:

* Download file : http://sourceforge.net/projects/gnuwin32/files/file/5.03/file-5.03-bin.zip , extract the zip, copy file.exe and magic1.dll from the bin folder to "C:\GetGnuWin32\bin" overwrite all files if asked.  

* Download the file dependencies: http://sourceforge.net/projects/gnuwin32/files/file/5.03/file-5.03-dep.zip , extract the zip, copy regex2.dll and zlib1.dll from the bin folder to "C:\GetGnuWin32\bin" overwrite all files files if asked.  

* Download which: http://sourceforge.net/projects/gnuwin32/files/which/2.20/which-2.20-bin.zip , extract the zip, copy which.ext from the bin folder to "C:\GetGnuWin32\bin" overwrite all files files if asked.  

* You should now reboot your server to allow windows to recognize your Environment Variable changes.

* Create a file here: "C:\xampp\htdocs\nzedb-pull.bat" , Add the text below, change the path to git based on where you installed it. After saving it, double click it to download nZEDb.

```
@echo off  
"C:\Program Files (x86)\Git\bin\git.exe" clone git://github.com/nZEDb/nZEDb.git C:\xampp\htdocs\nZEDb  
pause  
```

* Create a file here: "C:\xampp\htdocs\nZEDb\nzedb-update.bat" , Add the text below, change the path to git based on where you installed it. Run this file in the future to update to the latest git version of nZEDb.  

```
@echo off  
"C:\Program Files (x86)\Git\bin\git.exe" pull  
pause  
```

* Edit "C:\xampp\apache\conf\httpd.conf" and change DocumentRoot "/xampp/htdocs/" to DocumentRoot "/xampp/htdocs/nZEDb/www/" and change `<Directory "C:/xampp/htdocs">` to `<Directory "C:/xampp/htdocs/nZEDb/www">`  

* Edit "C:\xampp\php\php.ini" , change `max_execution_time = 30` to `max_execution_time = 120` , `memory_limit = 128M` to `memory_limit = 1024M` , `date.timezone` to your local one listed here: http://php.net/manual/en/timezones.php example : `date.timezone=America/New_York` , remove the ; in front of `extension=php_fileinfo.dll`  

* Open "C:\xampp\passwords.txt" and review default XAMPP passwords.  

* Edit "C:\xampp\mysql\bin\my.ini" , add `local_infile = 1` to the `[mysqld]` section, change `max_allowed_packet` to 16M , change `key_buffer` to 256M , add `group_concat_max_len = 8192` to the `[mysqld]` section.

* Run XAMPP Control Panel and start Apache and MySQL.  

* Open your browser and visit http://localhost/, follow on screen setup prompts.  

* Download and install extras like http://www.rarlab.com/download.htm for password detection, http://ffmpeg.zeranoe.com/builds/ & http://mediainfo.sourceforge.net/en/Download/Windows for post processing.  

From there the work is just getting started but this will give you a working nZEDb site running under XAMPP on Windows. How you fine tune your site is up to you.

