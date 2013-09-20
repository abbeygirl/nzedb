Here a list of statements useful for managing the post-processing queues.

***

An example *nix ps command to display any post-proccessing task taking longer than 60 seconds:
* ps aux | grep postprocess_new.php | grep -v grep | sed -e 's/://g' | awk '{if ($10 > 60) print $13,$15 }'

This outputs the ID and guid of the release. This SQL query will display the name
* SELECT ID,passwordstatus,name,searchname FROM releases WHERE ID = nnnnnnn;

nnnnnnn = ID, the first number displayed by the ps command


***

## Various SQL Queries for the Post-Processing Queues

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
```
mysql> SELECT count(*) FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);
```
_Find_
```
mysql> SELECT r.ID,r.passwordstatus,name FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);
```

_Clear (Untested, do a backup first!)_
```
mysql> untested! > update releases set r.passwordstatus = 0 where r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);
```

### Console
_Count the number in database_
* mysql> SELECT COUNT( id ) FROM releases WHERE categoryid BETWEEN 1000 AND 1999 AND nzbstatus = 1;