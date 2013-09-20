# Managing Your Releases

Here a few commands/scripts that are useful for general day-to-day operations.

### Find a specific release based on ID

* mysql> SELECT ID,passwordstatus,name,searchname,guid FROM releases WHERE ID = _4751258_;

### Delete a specific release

* cd misc/testing/DB_scripts
* php delete_release.php guid equals _1f374b691cdba39757e5a7a59978ceb89373fb20_

## Cross-posted or Duplicate Releases

_View_
* mysql> select name,fromname,size,count(*) as dupes from releases group by name,fromname,size HAVING dupes > 1;