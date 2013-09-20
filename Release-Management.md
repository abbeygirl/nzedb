# Managing Your Releases

Here a few commands/scripts that are useful for general day-to-day operations.

### Find a specific release based on ID
```
for i in {1..9999}; do ps aux | grep postprocess_new.php | grep -v grep | sed -e 's/://g' | awk '{print $13,$15 }' | grep 4751258; done
```
outputs the ID and GUID of the release.

or via SQL

```
mysql> SELECT ID,passwordstatus,name,searchname,guid FROM releases WHERE ID = 4751258;
```

### Delete a specific release

```
cd misc/testing/DB_scripts
php delete_release.php guid equals 1f374b691cdba39757e5a7a59978ceb89373fb20
```

## Cross-posted or Duplicate Releases

_View_
* mysql> select name,fromname,size,count(*) as dupes from releases group by name,fromname,size HAVING dupes > 1;