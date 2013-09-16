## Database Administration for nZEDb

A simple list of database SQL statements cut & paste ready.

If you don't know what you're doing, make backups first.

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