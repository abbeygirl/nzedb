Although windows is not offically supported, you can run it under the XAMMP stack. However you will not be able to utilize the tmux scripts.

* Download XAMPP from https://www.apachefriends.org/download.html and install to "C:\XAMPP\"   

* Download Git for Windows from http://git-scm.com/download/win and during installation, pick the SYSTEM PATH setting, to make it available to all users. If you already installed git, you can add it to the SYSTEM PATH by going to a command line, run `control sysdm.cpl` go to the Advanced tab, click Environment Variables, in System variables, scroll down to the Path variable, click edit, in Variable Value, scroll to the end, add the path to the git cmd folder like this for example: `;C:\Program Files (x86)\Git\cmd`

* Download Which from http://downloads.sourceforge.net/gnuwin32/which-2.20-bin.zip extract it, copy which.exe from the bin folder to C:\Windows

* Create file "C:\XAMPP\htdocs\nzedb-pull.bat" and run it to pull nZEDb from Github. Change the path to git based on where you installed it.

```
@echo off  
C:\Program Files (x86)\Git\bin\git.exe clone git://github.com/nZEDb/nZEDb.git C:\XAMPP\htdocs\nZEDb  
pause  
```

* Create file "C:\XAMPP\htdocs\nZEDb\nzedb-update.bat", run this file in the future to update to the latest git version of nZEDb. Change the path to git based on where you installed it.

```
@echo off  
C:\Program Files (x86)\Git\bin\git.exe pull  
pause  
```

* Edit "C:\XAMPP\apache\conf\httpd.conf" and change DocumentRoot "/xampp/htdocs/" to DocumentRoot "/xampp/htdocs/nzedb/www/"  

* Edit "C:\XAMPP\php\php.ini" and change max_execution_time = 30 to max_execution_time = 120 and memory_limit = 128M to memory_limit = 1024M  

* Open "C:\XAMPP\passwords.txt" and review default XAMPP passwords.  

* Run XAMPP Control Panel and start Apache and MySQL.  

* Open your browser and visit http://localhost/, follow on screen setup prompts.  

* Download and install extras like http://www.rarlab.com/download.htm for password detection, http://ffmpeg.zeranoe.com/builds/ & http://mediainfo.sourceforge.net/en/Download/Windows for post processing, and http://getgnuwin32.sourceforge.net/ for some unix features if wanted.

From there the work is just getting started but this will give you a working nZEDb site running under XAMPP on Windows. How you fine tune your site is up to you.

