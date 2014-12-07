Although windows is not offically supported, you can run it under the XAMMP stack. However you will not be able to utilize the tmux scripts.  

* Download XAMPP from https://www.apachefriends.org/download.html and install to "C:\xampp\"  

* Download Git for Windows from http://git-scm.com/download/win and install it.  
IMPORTANT: PICK THIS OPTION WHILE INSTALLING GIT OR THE INSTALL OF nZEDb WILL FAIL (you have been warned) : 
Checkout as-is, commit Unix-style line endings  

* After installation of git, go to a command line or run window by pressing Windows key + r, type `control sysdm.cpl` and run it, go to the Advanced tab, click Environment Variables, in System variables, scroll down to the Path variable, click edit, in Variable Value, scroll to the far right, add the path to the git cmd folder like this for example: `;C:\Program Files (x86)\Git\cmd`  

* Download gnuwin32 from http://sourceforge.net/projects/getgnuwin32/files/ , the current version is GetGnuWin32-0.6.3.exe , install it to "C:\" , it will create a folder called "GetGnuWin32" automatically.  
After you finish installing it, add this: `;C:\GetGnuWin32\gnuwin32\bin` to your Environment Variables Path variable (as we did above for git).  

* You must now run (in "C:\GetGnuWin32") download.bat to download the packages and install.bat to install the gnuwin32 packages. Use all the default options when asked.  

* Download the latest version of ansicon : http://adoxa.altervista.org/ansicon/ , extract it. Create a folder "C:\ansicon" , copy the contents of x64 or x86 depending on your Windows version (64 bits or 32 bits), to  "C:\ansicon" , open a command prompt with admin rights, type `cd c:\ansicon` , press enter. Type ` ansicon -i` , press enter. Add `;C:\ansicon` to your Environment Variables Path variable (as we did above for git). This will properly display text on the command line when running php scripts.

* You should now reboot your server to allow windows to recognize your Environment Variable changes.  

* Type `which git` , if you get the file location of git.exe, then you can continue, if not, you messed up either installing git or gnuwin32 or setting the PATH variable and you have to redo the above steps.

* To make sure git downloads nZEDb with the correct line endings, type this in a cmd prompt: `git config --global core.autocrlf input`

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

* After the install, you need to go in the admin section and add the magic file info stuff, this is required.

From there the work is just getting started but this will give you a working nZEDb site running under XAMPP on Windows. How you fine tune your site is up to you.

