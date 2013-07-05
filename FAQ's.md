Some common mistakes/errors people encounter.

Q. I changed NNTP providers and now everything is screwed up.
A. You need to reset groups and truncate some tables. A script exists to handle this for you.

  ```php misc/testing/DB_scripts/reset_truncate.php```

Q: Sabnzbd is not working.  
A: Make sure you have the sabnzbd/ in the url.

Q: Sickbeard/Couchpotato are not working.  
A: Make sure they are not https.

Q: I do not see any releases unless I click on ALL.  
A: Go to http://localhost/profileedit untick the 4 checkmarks, click save profile.

Q: I'm not getting any covers or x type of release are not being post processed.  
A: Make sure your keys are good, all the keys that come with nZEDb are tested and work. It is still preferable to use your own keys. We do not supply a trakt key.

Q: How do I test the amazon keys?  
A: There is a script in the misc/testing/Dev_testing folder. Read the whole output if you get an error (most people are only reading the bottom part).

Q: I'm having x issue after converting from newznab +.  
A: We will not support newznab + conversions, the script is there if you want to use it, use at your own will.

Q: I'm getting lots of spam, or small files.   
A: Use blacklists. removeCrapReleases script, size settings for groups etc..

Q: The scripts and my site is slow.   
A: You will need to tune MYSQL, have a look at the [database tuning](https://github.com/nZEDb/nZEDb/wiki/Database-tuning) page.

Q: Sabnzbd says I have problems with extra lines in the NZB.    
A: There is a script in misc/testing/Dev_testing to fix those NZB files.

Q: I'm getting many releases with unusable names.   
A: There is a script in misc/testing/Release_scripts called fixReleaseNames.php
   Do not expect miracles...
 
Q: How do I run x script?   
A: type php name-of-the-script.php , most of the scripts tell you how to use them if you run them like that.

Q: My parts/binaries/collections tabls are very large.    
A: Article collections with poorly named subjects or incomplete collections are created 2 hours after the last time we have downloaded an article for that collection,
   if you keep backfilling, your parts/binaries/collections tables will get large obviously...

Q: I'm having x issue not in the readme or FAQ.    
A: Please do some research first. If you can't solve the issue, we have a channel on IRC, server : synirc, channel #nZEDB

Q: I have converted my mysql tables to InnoDB and the ibdata file keeps getting bigger, even after I optimize the tables?    
A: You should follow the recommendations found on these 2 webpages.
* http://stackoverflow.com/questions/3927690/howto-clean-a-mysql-innodb-storage-engine/4056261#4056261   
* http://www.mysqlperformanceblog.com/2011/11/06/improved-innodb-fast-index-creation   

Percona Server as of versions 5.1.56 and 5.5.11 allows utilizing fast index creation for all of the above cases, which can potentially speed them up greatly. Be sure to add to you InnoDB section: 
```
expand_fast_index_creation = 1   
innodb_merge_sort_block_size = 1G
```

Q: I'm having issues with the PREDB backfill script.    
A: https://github.com/nZEDb/pre-info    
 
Q: Can I have some information on collections/binaries/parts?    
A: http://s12.postimg.org/ity5z1xnf/Untitled.jpg    

**To Update nZEDb (Ubuntu)**  
1. cd /var/www/nZEDb  
2. sudo git pull  
3. cd /var/www/nZEDb/misc/testing/DB_scripts  
4. php patchmysql.php  
5. rm -rf /var/www/nZEDb/www/lib/smarty/templates_c/*  

**Admin of Site**  
Q. How to change registration status  
A. You will need to change in MySQL.  
update site set value = 0 where setting = 'registerstatus';   

Q. Best way to change user password (admin) when no options to send out email ? If directly in db what password options to set, like md5, password ??   
A. Create new user, change role in db, reset password from webui  

**Backfilling**  
_Hanging on Backfilling_  
	If there is a bad nzb   
Run this SQL command elect r.ID, r.guid, r.name, c.disablepreview, r.size, r.adddate from releases r left join category c on c.ID = r.categoryID where nzbstatus = 1 and (r.passwordstatus between -6 and -1) AND (r.haspreview = -1 and c.disablepreview = 0) order by adddate desc limit 1;   
then delete the one it lists  

Q. Should I run backfill and update binaries at the same time?   
A. You can, but shouldn't if you have myisam tables  

Q. I have groups activated and activated for backfill, but tmux still says no groups enabled for backfill.  I copied the setting from my desktop install which is working correctly.  
A. You still have to run update_bins on them first or they fail the query  

**Errors**  
Q. file_put_contents(/var/www/nZEDb/nzbfiles/tmpunrar/rarfile.rar): failed to open stream: Permission denied in /var/www/nZEDb/www/lib/postprocess.php on line 648  
A. Make sure your tmpunrar folder is writable by sudo chmod 777 /var/www/nZEDb/nzbfiles/ 

Q. KeyError: 'DB_SOCKET'   
Traceback (most recent call last):    
File "/var/www/nZEDb/www/../misc/update_scripts/threaded_scripts/grabnzbs_threaded.py", line 26, in     <module>    
con = mdb.connect(host=conf['DB_HOST'], user=conf['DB_USER'], passwd=conf['DB_PASSWORD'],     db=conf['DB_NAME'], port=int(conf['DB_PORT']    
), unix_socket=conf['DB_SOCKET'])      
A. cymysql has been updated to use sockets, python scripts also, you will need to:    
run sudo pip install --upgrade cymysql    
-or-   
run sudo easy_install --upgrade cymysql   
run sudo pip-3.2 install --upgrade cymysql   
run sudo pip-3.3 install --upgrade cymysql   
-and-   
add define('DB_SOCKET', '/var/run/mysqld/mysqld.sock'); to you www/config.php

**Tmux**   
Q. How do I monitor the server from another computer?   
A. Step 1 - ssh into the server   
   Step 2 - Once your are connected, type "tmux attach-session -t nZEDb" (without quotes)   

Q: How do I move from pane to pane in tmux?   
A: ctrl+a and an arrow key    

Q: How do I move from page to page in tmux? (i.e. 0 - monitor, 1 - utils, 2 - post, 3 - optimize, 4 - bash)    
A: ctrl-a followed by the number ctrl-a 1, ctrl-a 2 etc... or ctrl-a n and ctrl-a p ('n' for next, 'p' previous)

************************************************

Please do not open issues on github if the question is already asked. Take a few minutes to look at the titles of other issues. 

Do not expect us to implement requested features in a short amount of time.  

Some requested features we will not implement, you are free to clone the git. 

## Make sure you read the changelog to see if there are any new features added!