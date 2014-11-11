Post-Processing attempts up to 6 times for each release, prioritising new releases over the ones it has tried previously. Should there be many thousands queued for post-processing it may appear that the releases are not being processed, however this is seldom the case.

This SQL query provides a simple overview:

```
SELECT passwordstatus, count(*) FROM releases WHERE haspreview = -1 GROUP BY passwordstatus;
```


***

## Various SQL Queries for the Post-Processing Queues

### NFO
_Count Queued_
```
> SELECT nfostatus,COUNT(*) FROM releases WHERE nfostatus IN ( -6, -5, -4, -3, -2, -1 ) group by nfostatus;
```
 
_Find Queued_
```
> SELECT ID,nfostatus,guid,searchname FROM releases WHERE nfostatus IN ( -6, -5, -4, -3, -2, -1 );
```

_Clear Queued_
```
> update releases set nfostatus = 0 WHERE nfostatus < 0;
```

### Audio
_Count Queued_
```
> select count(*) from releases WHERE musicinfoID IS NULL and nzbstatus = 1 and categoryID BETWEEN 3000 AND 3999;
```

_Find Queued_
```
> select ID,musicinfoID,name from releases WHERE musicinfoID IS NULL and nzbstatus = 1 and categoryID BETWEEN 3000 AND 3999;
```

_Clear Queued_
```
> update releases set musicinfoID = '-2' where musicinfoID IS NULL and nzbstatus = 1 and categoryID BETWEEN 3000 AND 3999;
```

### TV
_Count Queued_
```
> SELECT COUNT(*) FROM releases r, category c WHERE r.categoryid = c.id AND c.parentid = 5000 AND rageid = -1;
```

_Find Queued_
```
> SELECT r.ID,passwordstatus,name from releases r, category c WHERE r.categoryid = c.id AND c.parentid = 5000 AND rageid = -1;
```

_Clear Queued_
```
> update releases set rageID = 1 where rageID = -1 and categoryID BETWEEN 5000 AND 5999;
```

### Movies
_Find Queued_
```
> select id,passwordstatus,name FROM releases WHERE imdbID IS NULL and categoryID BETWEEN 2000 AND 2999 AND nzbstatus = 1;
```

_Set imdbID_
```
> update example: update releases set imdbID = 1686784 WHERE id = 3789422; 
```

_Reset for re-postprocessing_
```
> update releases set imdbID = NULL WHERE imdbID = 0 and categoryID BETWEEN 2000 AND 2999; 
```

### Misc (Additional)
This is the sum of PC(4000), Pron(6000) and Misc(7000).

_Count Queued_
```
> SELECT count(*) FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);
```
_Find Queued_
```
> SELECT r.ID,r.passwordstatus,adddate,name FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);
```

_Clear Queued_
```
> update releases r left join category c on c.ID = r.categoryID set passwordstatus = 0 where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);
```


### Console
_Count Queued_
```
> SELECT COUNT(*) FROM releases r, category c WHERE r.categoryid = c.id AND c.parentid = 1000 AND consoleinfoid IS NULL;
```