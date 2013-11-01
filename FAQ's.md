**Installation**  
Q: I'm having x issue after converting from newznab +.  
A: We will not support newznab + conversions, the script is there if you want to use it, use at your own will.


**3rd Party**  
Q: SABnzbd is not working.  
A: Make sure you have the sabnzbd/ in the url.

Q: Sickbeard/Couchpotato are not working.  
A: Make sure they are not https.

Q: Sabnzbd says I have problems with extra lines in the NZB.    
A: There is a script in misc/testing/Dev_testing to fix those NZB files.

Q: How do I test the amazon keys?  
A: There is a script in the misc/testing/Dev_testing folder. Read the whole output if you get an error (most people are only reading the bottom part).


**Admin of Site**  
Q. How to change registration status  
A. You will need to change in MySQL.  
update site set value = 0 where setting = 'registerstatus';   

Q. Best way to change user password (admin) when no options to send out email ? If directly in db what password options to set, like md5, password ??   
A. Create new user, change role in db, reset password from webui  

**nZEDb Script Locations and use (for linux)**

/var/www/nZEDb/misc/testing/  
nzb-import.php  

/var/www/nZEDb/misc/testing/Bulk_import_linux  
nzb-import-bulk.php  

/var/www/nZEDb/misc/testing/DB_scripts    
active_groups.php      
autopatcher.php  
backfill_groups.php  
backfill_predb.php          
convert_from_newznab.php  
convert_mysql_tables.php  
copy_from_newznab.php    
delete_releases.php  
mysql_to_ramdisk.sh             
mysqldump_tables.php     
nzb-reorg.php  
patchmysql.php            
populate_imdb_type.php    
populate_nzb_guid.php      
resetdb.php - This script removes all releases, nzb files, truncates all article tables, resets groups.  
              _example: php resetdb.php true_  
reset_Collections.php        
reset_postprocessing.php   
reset_truncate.php - This script removes releases with no NZBs, resets all groups, truncates article tables. All other releases are left alone.  

/var/www/nZEDb/misc/testing/Dev_testing  
check_nzbfolder.php    
clean_nzbs.php  
create_predb_patches.php    
fix_blank_link_nzb.php   
Local_Header.php    
Post_to_Date.php    
Testing-Date.php        
test-amazon.php    
test_misc_sorter.php  
test-rottentomato.php  

/var/www/nZEDb/misc/testing/Dev_testing/Subject_Testing  
test-backfillcollectionname.php    
test-binariescollectionname.php    
test-cleansubject.php     
test-releasecleaner.php   

/var/www/nZEDb/misc/testing/PostProc_scripts   
getAlbum.php     
getConsole.php     
getImdb.php     
getmovieCovers.php    
getTMdb.php     
getTVInfo.php     
musicTest.php   

/var/www/nZEDb/misc/testing/Release_scripts  
delete_disabled_category_releases.php  
fixReleaseNames.php  
foreignmoviesorter.php  
removeCrapReleases.php  
resetRelnameStatus.php  
resetSearchname.php  
showsleep.php

/var/www/nZEDb/misc/update_scripts  
backfill.php  
config.php 
decrypt_hashes.php  
optimise_db.php      
postprocess.php  
update_binaries.php  
update_releases.php    
update_theaters.php  
update_tvschedule.php

/var/www/nZEDb/misc/update_scripts/nix_scripts/screen/sequential    
simple.sh    
threaded.sh  

/var/www/nZEDb/misc/update_scripts/nix_scripts/screen/threaded  
helper.sh  
start.sh  

/var/www/nZEDb/misc/update_scripts/nix_scripts/tmux    
commit.sh  
monitor.php  
start.php  
tmux.conf   

/var/www/nZEDb/misc/update_scripts/nix_scripts/tmux/bin  
backfill_all_quick.php     
backfill_interval.php   
backfill_safe.php     
binaries.php  
grabnzbs.php     
optimize.php   
postprocess.php    
postprocess_amazon.php    
postprocess_new.php   
postprocess_non_amazon.php     
postprocess_pre.php     
requestID.php   
tmux-mem-cpu-load   
update_releases.php   

/var/www/nZEDb/misc/update_scripts/threaded_scripts    
backfill_safe_threaded.py     
backfill_threaded.py   
binaries_threaded.py     
grabnzbs_threaded.py   
import_threaded.py                 
partrepair_threaded.py     
postprocess_threaded.py   
postprocess_old_threaded.py       
requestid_threaded.py   

/var/www/nZEDb/misc/update_scripts/win_scripts  
optimisedb.bat       
runme.bat     
runme_with_scrape.bat     
Sleep.exe     
updatebackfill.bat     
updatebinaries.bat     
updatereleases.bat   


**To Update nZEDb (Ubuntu)**  
 1. cd /var/www/nZEDb  
 2. sudo git pull  
 3. cd /var/www/nZEDb/misc/testing/DB_scripts  
 4. php patchmysql.php  
 5. rm -rf /var/www/nZEDb/www/lib/smarty/templates_c/* 

**Backing up nZEDb (ubuntu)** (by trev_)  
 1. Stop tmux from processing.  Make sure all panes are dead.  
sudo killall tmux   
 2. Optimize the database   
/var/www/nZEDb/misc/update_scripts/nix_scripts/tmux/bin/optimize.php true  
 3. Backup the database  
mysqldump --opt -u <user> -p <password> > ~/nzedb-backup.sql  
(this will put the newly created backup in your home directory)  
 4. Backup nZEDb  
rsync -a --stats --progress  /var/www/nZEDb/ ~/nzedb-backup/    
(these are the steps I use, there are probably better ways.)  

Very simple backup script, has saved my DB more than once. Backs up nzbs and covers too : (by Nukien, with some editing by trev_)    
```
#!/bin/bash    
DATE= `date +%Y-%m-%d_%H-%M-%S` 
echo "Kicking off tar backup of nZEDb"      
sudo tar cf ${HOME}/MySQL-backups/${DATE}-nzedb.tar /etc/mysql /var/www/nZEDb/nzbfiles/[0-9]   /var/www/nZEDb/nzbfiles/[a-f] /var/www/nZEDb/www/covers/ &  
echo "===== Starting main backup"  
innobackupex --parallel=2 --no-timestamp --rsync ${HOME}/MySQL-backups/${DATE}  
echo "===== Applying logs to backup" 
sudo innobackupex --apply-log ${HOME}/MySQL-backups/${DATE}
```

Q: How do I switch branches of development?  
A: git checkout dev    
A: git checkout master  
A: git checkout jonnyboy  
(make sure your in your root directory for nZEDb)


**Tmux**   
Q. How do I monitor the server from another computer?   
A. Step 1 - ssh into the server   
   Step 2 - Once your are connected, type "tmux attach-session -t nZEDb" (without quotes)   

Q: How do I move from pane to pane in tmux?   
A: ctrl+a and an arrow key    

Q: How do I move from page to page in tmux? (i.e. 0 - monitor, 1 - utils, 2 - post, 3 - optimize, 4 - bash)    
A: ctrl-a followed by the number ctrl-a 1, ctrl-a 2 etc... or ctrl-a n and ctrl-a p ('n' for next, 'p' previous)

Q: How do I show the USP settings in powerline?  
A: add the following line to tmux.sh   
"uspsetting 229 0/"


**Adding alternate NNTP for post processing**  
 1. Under Admin click Edit Site  
 2. Scroll down to Advanced - Threaded Settings  
 3. Click the box for Alternate NNTP Ptovider    
 4. Save Settings  
 5. Once that is done, edit the file www/config.php  
define('NNTP_USERNAME_A', 'username');  <-- replace username with your info  
define('NNTP_PASSWORD_A', 'password');  <-- replace password with your info  
define('NNTP_SERVER_A', 'server');  <-- replace server with your info  
define('NNTP_PORT_A', 'port');  <-- replace port with your info  
define('NNTP_SSLENABLED_A', true);  <--or false if your not using SSL  

Q. I need to change NNTP Usenet provider (USP)     
A. You need to reset groups and truncate some tables. A script exists to handle this for you.   
  ```php (path to nZEDb)/misc/testing/DB_scripts/change_USP_provider.php```
Run the script without option for instructions.


**Processing**  
Q: I'm not getting any covers or x type of release are not being post processed.  
A: Make sure your keys are good, all the keys that come with nZEDb are tested and work. It is still preferable to use your own keys. We do not supply a trakt key.

Q: I'm getting lots of spam, or small files.   
A: Use blacklists. removeCrapReleases script, size settings for groups etc..

Q: I'm getting many releases with unusable names.   
A: There is a script in misc/testing/Release_scripts called fixReleaseNames.php
   Do not expect miracles...

Q: I'm having issues with the PREDB backfill script.    
A: https://github.com/nZEDb/pre-info

Q: I do not see any releases unless I click on ALL.  
A: Go to http://localhost/profileedit untick the 4 checkmarks, click save profile.

Q: The scripts and my site is slow.   
A: You will need to tune MYSQL, have a look at the [database tuning](https://github.com/nZEDb/nZEDb/wiki/Database-tuning) page.

Q: How do I run x script?   
A: type php name-of-the-script.php , most of the scripts tell you how to use them if you run them like that.

Q: My parts/binaries/collections tables are very large.    
A: Article collections with poorly named subjects or incomplete collections are created 2 hours after the last time we have downloaded an article for that collection,
   if you keep backfilling, your parts/binaries/collections tables will get large obviously...

Q: I'm having x issue not in the readme or FAQ.    
A: Please do some research first. If you can't solve the issue, we have a channel on IRC, server : synirc, channel #nZEDB

Q: Can I have some information on collections/binaries/parts?    
A: http://s12.postimg.org/ity5z1xnf/Untitled.jpg   

Q: My parts table is very large, what should I do?  
A: Disable update binaries, backfill, and import.  Let update releases run to clear the backlog out.  

Q: Is there was a script that would remove releases based on the fact that the nzb for them is missing?  
A: Yes, clean_nzbs.php which can be located at /var/www/nZEDb/misc/testing/Dev_testing (linux)  

**Backfilling**  
_Hanging on Backfilling_  
	If there is a bad nzb   
Run this SQL command elect r.ID, r.guid, r.name, c.disablepreview, r.size, r.adddate from releases r left join category c on c.ID = r.categoryID where nzbstatus = 1 and (r.passwordstatus between -6 and -1) AND (r.haspreview = -1 and c.disablepreview = 0) order by adddate desc limit 1;   
then delete the one it lists  

Q. Should I run backfill and update binaries at the same time?   
A. You can, but shouldn't if you have myisam tables  

Q. I have groups activated and activated for backfill, but tmux still says no groups enabled for backfill.  I copied the setting from my desktop install which is working correctly.  
A. You still have to run update_bins on them first or they fail the query  


**MySQL/InnoDB**

Q: I have converted my mysql tables to InnoDB and the ibdata file keeps getting bigger, even after I optimize the tables?    
A: You should follow the recommendations found on these 2 webpages.
* http://stackoverflow.com/questions/3927690/howto-clean-a-mysql-innodb-storage-engine/4056261#4056261   
* http://www.mysqlperformanceblog.com/2011/11/06/improved-innodb-fast-index-creation   

Percona Server as of versions 5.1.56 and 5.5.11 allows utilizing fast index creation for all of the above cases, which can potentially speed them up greatly. Be sure to add to you InnoDB section: 
```
expand_fast_index_creation = 1   
innodb_merge_sort_block_size = 1G
```

Q: I am trying to convert my mysql tables to InnoDB and mysql crashes or returns with this error.
```
ERROR 1114 (HY000): The table '#sql-4f6_2a' is full
```
A: The most likely answer is that the partition holding your mysql tmp dir is full. If you have pointed this to /dev/shm, then either drop the indexes on releases, convert to InnoDB and recreate the indexes. Alternatively, change the path to mysql temp, restart mysql, convert to InnoDB, change path back for mysql tmp, restart mysql.

Q: I am getting this error message "Lock wait timeout exceeded".

A: The application is waiting too long to gain update locks with InnoDB. Within my.cnf increase this value: innodb_lock_wait_timeout. Default is 50, generally increased to 150. 


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
add define('DB_SOCKET', '/var/run/mysqld/mysqld.sock');  
to your /var/www/nZEDb/www/config.php

Q: I noticed my update_releases did this:
|PHP Warning:  simplexml_load_file(): compress.zlib:///var/www/nZEDb/nzbfiles/a/blaaaablaaa.nzb.gz:34: parser error : Extra content at the end of the docu
19:58 <&jonnyboy> PHP Warning:  simplexml_load_file(): compress.zlib:///var/www/nZEDb/nzbfiles/a/blaaablaaaa.nzb.gz:32: parser error : Opening and ending tag mismatch: nzb

A:
Most likely a nzb is invalid.


************************************************

Please do not open issues on github if the question is already asked. Take a few minutes to look at the titles of other issues. 

Do not expect us to implement requested features in a short amount of time.  

Some requested features we will not implement, you are free to clone the git. 

## Make sure you read the changelog to see if there are any new features added!