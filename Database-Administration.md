## Database Administration for nZEDb

Miscellaneous SQL statements that you may find useful, cut & paste ready.

## Check progress while populating guid in releases table
* mysql> SELECT count(*) from releases where nzb_guid is null and nzbstatus = 1;

## Request-ID lookups 
_Count_
* mysql> SELECT count(*) FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);

_View_
* mysql> SELECT r.ID,r.passwordstatus,name FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);