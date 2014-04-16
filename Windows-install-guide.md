Although windows is not offically supported, you can run it under the XAMMP stack. However you will not be able to utilize the tmux scripts.

* Download http://www.apachefriends.org/en/xampp-windows.html]XAMPP 1.8.1 and install to "C:\XAMPP\"   

* Download http://code.google.com/p/msysgit/downloads/list Git for Windows and install to "C:\XAMPP\git\"  

* Create file "C:\XAMPP\htdocs\nzedb-pull.bat" and run it to pull nZEDb from Github.

```
@echo off  
C:\XAMPP\git\bin\git.exe clone git://github.com/nZEDb/nZEDb.git C:\XAMPP\htdocs\nZEDb  
pause  
```

* Create file "C:\XAMPP\htdocs\nZEDb\nzedb-update.bat", run this file in the future to update to the latest git version of nZEDb.

```
@echo off  
C:\XAMPP\git\bin\git.exe pull  
pause  
```

* Edit "C:\XAMPP\apache\conf\httpd.conf" and change DocumentRoot "/xampp/htdocs/" to DocumentRoot "/xampp/htdocs/nzedb/www/"  

* Edit "C:\XAMPP\php\php.ini" and change max_execution_time = 30 to max_execution_time = 120 and memory_limit = 128M to memory_limit = 1024M  

* Open "C:\XAMPP\passwords.txt" and review default XAMPP passwords.  

* Run XAMPP Control Panel and start Apache and MySQL.  

* Open your browser and visit http://localhost/, follow on screen setup prompts.  

* Download and install extras like http://www.rarlab.com/download.htm for password detection, http://ffmpeg.zeranoe.com/builds/ & http://mediainfo.sourceforge.net/en/Download/Windows for post processing, and http://getgnuwin32.sourceforge.net/ for some unix features if wanted.

From there the work is just getting started but this will give you a working nZEDb site running under XAMPP on Windows. How you fine tune your site is up to you.

