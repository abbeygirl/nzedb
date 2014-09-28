**nZEDb for Windows IIS 8.5 on Windows 8.1**

This guide assumes you know how to navigate Windows and setup programs for use with nZEDb. 

* Activate IIS in **Control Panel > Uninstall Programs > Turn Windows features on or off**.  
Click **Internet Information Services**, accept the defaults as we do not need all features.

![](http://i.imgur.com/7MxiUWz.png)


* Then drill through - **Internet Information Services > World Wide Services > Application Development Features** and Select **CGI**.

![](http://i.imgur.com/5069WEd.png)
* Click OK to install these features. Windows may ask to restart. Do so. On reboot, IIS has been installed. 

* The latest version on FastCGI requires [Visual C++ Redistributable for Visual Studio](http://www.microsoft.com/en-us/download/confirmation.aspx?id=30679). Click Download and choose the x86 version even if you are using x64 Windows and download and install it. x64 does not have the required files! 

* Open IIS Manager and on the right hand side, click "Get New Web Platform Components". You will be forwarded to WebPI. Download and install it. In the search once installed, search for and Add  
>PHP 5.6.0  
>URL Rewrite 2.0  
>WFastCGI 2.1 Gateway for IIS and Python 2.7.3  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Then Install/Accept them. Also, right click and remove "Default Web Site". We wont need that.

* Download Git for Windows from http://git-scm.com/download/win and install it.
**IMPORTANT:** Pick these option during install or the install of nZEDb WILL **FAIL**!
>Use Git from the Windows Command Prompt  
>Checkout as-is commit Unix-style line endings

* Download gnuwin32 from http://sourceforge.net/projects/getgnuwin32/files/latest/download?source=files  
Install it to "C:\" , it will create a folder called "GetGnuWin32" automatically.  
You must now run (in "C:\GetGnuWin32") download.bat to download the packages and install.bat to install the gnuwin32 packages. Use all the default options when asked.

* Download the latest version of ansicon: http://adoxa.altervista.org/ansicon/dl.php?f=ansicon
extract it.  
Create a folder "C:\ansicon", copy the contents of x64 or x86 depending on 
your Windows version (64 bits or 32 bits), to "C:\ansicon".  
Open a command prompt with admin rights.  
Type: cd c:\ansicon, press enter. Type ansicon -i , press enter.

* After installation, press Windows key + R and type: control sysdm.cpl  
Go to the Advanced tab, click Environment Variables, in System variables,  
scroll down to the Path variable, click edit, in Variable Value,  
scroll to the far right, add ;C:\GetGnuWin32\gnuwin32\bin;C:\ansicon  
to your Environment Variables Path

* You now need to reboot to allow Windows to recognise your Environment Variable changes.

* Create a file @ C:\inetpub\wwwroot\nZEDB_Pull.bat
```batch
@echo off  
git clone git://github.com/nZEDb/nZEDb  
cd nZEDb  
git checkout dev  
pause
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Run this (As Administrator) to pull the nZEDb Repo.

* Create a file @ C:\inetpub\wwwroot\nZEDB_Update.bat
```batch
@echo off  
git pull  
pause  
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Run this (As Administrator) to update from the nZEDb Repo. This can be scheduled once a week if you prefer.

* Back to IIS Create a new site called nZEDb  

![](http://i.imgur.com/JNoDQe0.png)

* Click the newly made site and click Handler Mappings.On the Right hand side click **Add Module Mapping**  
and add the details as shown in the image.

![](http://i.imgur.com/wwttHqi.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Click Request Restrictions and make sure File or Folder is selected.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Click OK. Click Yes.

* Click the Site **nZEDb**, Double Click **URL Rewrite**. On the right, click **Import Rules**.  
Under configuration file select the .htaccess file @ C:\inetpub\wwwroot\nZEDb\www  
  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Import and Apply  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IIS converts the it to a web.config file and saves it. 

* Click site **nZEDb** and double click **Default Document**. Add **index.php** as a default document.  
Right click site **nZEDb** and **Edit Permissions**. Make sure user **IIS_IUSRS** has **Full Control** of the folder. Also Permissions of C:\inetpub\wwwroot\nZEDb should also be changed to the Everyone - this is equivalent to CHMOD 777

* Right click and save [this file](http://pear.php.net/go-pear.phar) to your PC @ C:\Program Files (x86)\PHP\v5.6  
Open an Administrator Command line at this location and type: PHP go-pear.phar  
  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This will install PEAR to your PHP install. Press Enter twice and agree to alter the php.ini. Press Enter.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A file has been created at C:\Program Files (x86)\PHP\v5.6\PEAR_ENV.reg.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Double click it to Add PEAR to the PATH Environment.

* Open C:\Program Files (x86)\PHP\v5.6\php.ini and find **memory_limit** change it to -1 (from 1024mb).  
Also look for session.save_path towards the very bottom under WebPIChanges.  
The default path it C:\Windows\temp which is silly as it's a Windows System folder and PHP has no access.  
Change it to **C:\inetpub\wwwroot\nZEDb\resources\tmp\sessions** and make sure that folder exists with Everyone / Full Control permissions. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Save the php.ini. If Windows does not allow saving it, save it to the desktop and copy/overwrite it with Admin privileges. 

* For good measure, reboot windows and once loaded goto http://localhost  
All Preflight checks will pass and you are ready to install nZEDb.

**YOU DO NEED A MYSQL DATABASE INSTALLATION WHICH IS BEYOND THE SCOPE OF THIS TUTORIAL.**

Any questions, please ask in IRC or the Forums.