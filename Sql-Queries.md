## Database administration for nZEDb

A simple list of database SQL statements cut&paste ready

## Counting, finding and/or clearing nZEDb post-processing queues.

### NFO
**Find**
* mysql> select ID,nfostatus,name from releases WHERE nfostatus between -6 and -1 and nzbstatus = 1;
   
**Clear**
* mysql> update releases set nfostatus = 1 WHERE nfostatus between -6 and -1 and nzbstatus = 1;

### Audio
_Find_
* mysql> select ID,name from releases where musicinfoID IS NULL and categoryID BETWEEN 3000 AND 3999 and nzbstatus = 1;

Clear
* mysql> update releases set musicinfoID = '-2' where musicinfoID IS NULL and categoryID BETWEEN 3000 AND 3999 and nzbstatus = 1;

### TV
Count
* mysql> SELECT COUNT(*) from releases where rageID = -1 and categoryID BETWEEN 5000 AND 5999 and nzbstatus = 1;

### Movies
Find
* mysql> select id,name FROM releases WHERE imdbID IS NULL and categoryID BETWEEN 2000 AND 2999;
Set imdbID
* mysql> update example: update releases set imdbID = 1686784 WHERE id = 3789422; 

### Misc
Count
* mysql> SELECT count(*) FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);

Find
* mysql> SELECT r.ID,name FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);





## Check progress while populating guid in releases table
* SELECT count(*) from releases where nzb_guid is null and nzbstatus = 1;