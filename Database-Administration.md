## Database Administration for nZEDb

A simple list of database SQL statements cut & paste ready.

If you don't know what you're doing, make backups first.

If you do know what you're doing, see above


## Post-Processing Queues: Count, Find & Clear

### NFO
_Find_
* mysql> select ID,nfostatus,name from releases WHERE nfostatus between -6 and -1;
 
_Clear_
* mysql> update releases set nfostatus = 1 WHERE nfostatus between -6 and -1;

### Audio
_Find_
* mysql> select ID,musicinfoID,name from releases WHERE musicinfoID IS NULL and relnamestatus != 0 and categoryID in (3010, 3040, 3050);

_Clear_
* mysql> update releases set musicinfoID = '-2' where musicinfoID IS NULL and relnamestatus != 0 and categoryID in (3010, 3040, 3050);

### TV
_Count_
* mysql> SELECT COUNT(*) from releases where rageID = -1 and categoryID BETWEEN 5000 AND 5999;

_Find_
* mysql> SELECT ID,nzbstatus,name from releases where rageID = -1 and categoryID BETWEEN 5000 AND 5999;

### Movies
_Find_
* mysql> select id,name FROM releases WHERE imdbID IS NULL and categoryID BETWEEN 2000 AND 2999;

_Set imdbID_
* mysql> update example: update releases set imdbID = 1686784 WHERE id = 3789422; 

### Misc (Additional)
_Count_
* mysql> SELECT count(*) FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);

_Find_
* mysql> SELECT r.ID,name FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);

### Console
_Count the number in database_
* mysql> SELECT COUNT( id ) FROM releases WHERE categoryid BETWEEN 1000 AND 1999 AND nzbstatus = 1;



## Check progress while populating guid in releases table
* mysql> SELECT count(*) from releases where nzb_guid is null and nzbstatus = 1;


## Request-ID lookups 
_Count_
* mysql> SELECT count(*) FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);

_View_
* mysql> SELECT r.ID,r.passwordstatus,name FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);

## Cross-posted or Duplicate Releases

_Count_
* mysql> select name,fromname,size,count(*) as dupes from releases group by name,fromname,size HAVING dupes > 1;

_View_
* mysql> select r.id, r.name from releases r inner join (select r.name from releases r group by r.name having count(r.name) > 1) dup on r.name = dup.name;