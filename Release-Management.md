# Managing Your Releases

Here a few commands/scripts that are useful for general day-to-day operations.

### Delete a specific release

* cd misc/testing/DB_scripts
* php delete_release.php guid equals 1f374b691cdba39757e5a7a59978ceb89373fb20

## Cross-posted or Duplicate Releases

_View_
* mysql> select name,fromname,size,count(*) as dupes from releases group by name,fromname,size HAVING dupes > 1;